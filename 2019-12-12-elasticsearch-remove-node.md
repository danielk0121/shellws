# 사용한 엘라스틱 버전
```
7.4.0
```

# 클러스터 전체 노드에 샤드 라우팅 all 옵션 적용
```
curl -XPUT http://localhost:9200/_cluster/settings -H "Content-type: application/json" -d \
'{ 
	"transient": { 
		"cluster.routing.allocation.enable":"all"
	}
}'
```


# 샤드 라우팅 속도 증가를 위해, 모든 샤드의 리플리카를 0으로 설정
```
curl -XPUT localhost:9200/_settings -H 'Content-type: application/json' -d \
'{"index": {"number_of_replicas":0}}'
```


# 제거할 노드 ip 추가
curl -XPUT -H "Content-type: application/json" http://localhost:9200/_cluster/settings -d \
'{
  "transient": {
    "cluster.routing.allocation.exclude._ip": "192.168.100.107,192.168.100.106"
  }
}'


# 클러스터 설정 확인
```
curl -XGET http://localhost:9200/_cluster/settings?pretty

# response
{
  "persistent" : {
    "search" : {
      "max_buckets" : "200000"
    },
    "xpack" : {
      "monitoring" : {
        "collection" : {
          "enabled" : "true"
        }
      }
    }
  },
  "transient" : {
    "cluster" : {
      "routing" : {
        "allocation" : {
          "enable" : "all",
          "exclude" : {
            "_ip" : "192.168.100.107,192.168.100.106"  <--- 이부분 확인
          }
        }
      }
    }
  }
}
```


# 각 노드당 사용 디스크, 사용 샤드 상태 확인
```
curl -XGET localhost:9200/_cat/allocation?v

# response
# rpnode4 는 이전에 제거한 노드
# rpnode3 은 샤드 개수가 현재 42개로, 제거 되고 있음
shards disk.indices disk.used disk.avail disk.total disk.percent host            ip              node
     0           0b     7.5gb    906.5gb      914gb            0 192.168.100.107 192.168.100.107 rpnode4
    71      538.9gb   560.3gb    353.7gb      914gb           61 192.168.100.104 192.168.100.104 rpnode1
    69      529.6gb   539.1gb    382.8gb      922gb           58 192.168.100.105 192.168.100.105 rpnode2
    42      378.4gb   386.2gb    527.8gb      914gb           42 192.168.100.106 192.168.100.106 rpnode3

```


# 샤드 이동 실시간 확인
```
curl -XGET "http://localhost:9200/_cat/recovery?active_only&v"

# response
# source_node, target_node 를 보면 샤드가 이동하는걸 볼 수 있다
index           shard time type stage source_host     source_node target_host     target_node repository snapshot files files_recovered files_percent files_total bytes       bytes_recovered bytes_percent bytes_total translog_ops translog_ops_recovered translog_ops_percent
outlog-20191008 0     8.4m peer index 192.168.100.106 rpnode3     192.168.100.105 rpnode2     n/a        n/a      126   125             99.2%         126         11578592835 9619871450      83.1%         11578592835 0            0                      100.0%
outlog-20191116 0     2.7m peer index 192.168.100.106 rpnode3     192.168.100.105 rpnode2     n/a        n/a      84    79              94.0%         84          8150472875  2613972210      32.1%         8150472875  0            0                      100.0%
```
