# elasticsearch 구글링 기록

### aggregation bucket size 초과 에러 처리

```
GET /_cluster/settings?flat_settings=true

PUT /_cluster/settings
{
  "persistent":{
    "search.max_buckets":50000
  }
}

또는 elasticsearch.yml 에서 변경
```

### response hits.total.value:10000, hits.total.relation:gte 자꾸만 10000 이 뜨는데, 뭘까
```
# hits.total 에는 검색에 사용된 총 document(elastic 에서 한개 doc 을 뭐라고 부르는지 모르겠다) 개수를 의미함
# response ex
{
  ...
  "hits" : {
    "total" : {
      "value" : 10000,  // 10000 개 doc
      "relation" : "gte"  // gte 즉, 10000 개 이상이 검색에 hits 되었다
    },
    "max_score" : null,
    "hits" : [ ]  // 검색 결과 내용
  },
  ...
}

# request
GET myindex/_search
{
  "track_total_hits": true,  // 전체 hits 검색에 사용된 doc 개수를 추적함
  "size": 10,
  "query": ...
}

# response
{
  ...
  "hits" : {
    "total" : {
      "value" : 10982573,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  ...
}
```

### count api
```
# 28 124 451
GET outputlog-20191010/_count

# 28 112 457
GET outputlog-20191010/_search
{
  "track_total_hits": true
}

# 28 112 457
GET output*/_count
{
  "query": {
    "bool": {
      "must": [
        {"range":{"logtime":{ 
          "gte": "20191010_000000", "lte": "20191010_235959" 
        }}}, 
        { "terms": { "resBody.platformCodeWM": ["W","M"], "boost": 1 } }, 
        { "terms": { "resBody.ad": ["SR","CW"], "boost": 1 } }
      ]
    }
  }
}

# 8539
GET output*/_count
{
  "query": {
    "bool": {
      "must_not": [
        {"range":{"logtime":{ 
          "gte": "20191010_000000", "lte": "20191010_235959" 
        }}}, 
        { "terms": { "resBody.platformCodeWM": ["W","M"], "boost": 1 } }, 
        { "terms": { "resBody.ad": ["SR","CW"], "boost": 1 } }
      ]
    }
  }
}

# 28,124,451 - 28,112,457 = 11,994 
# 11,994 vs 8,539  ??? ==> 확인이 필요함
```
