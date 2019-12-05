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

### response body 일부분만 응답받기 - filter_path url parameter 사용
```
GET output*/_search?filter_path=aggregations.count_groupby.buckets.doc_count
```

### painless script 에러 - 필드가 없는 doc 때문에 발생
```
POST outlog-20191025/_search
{
  "size": 0, 
  "aggs": {
    "a0": {
      "terms": {
        "script": {
          "lang": "painless",
          "source": """
            if(doc['resBody.itlTpCode'].size()!=0) {       <---- 처리 안해주면 아래줄 .value 코드에서 에러 난다
              String itlTpCode = doc['resBody.itlTpCode'].value;
              if("01".equals(itlTpCode)) return 'mobon';
              else return 'openRTB';
            }
            return 'notExists';
          """
        }, 
        "size": 10
      }
    }
  }
}

{
  "took" : 6878,
  "timed_out" : false,
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "a0" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "mobon",
          "doc_count" : 27610880
        },
        {
          "key" : "openRTB",
          "doc_count" : 18152668
        },
        {
          "key" : "notExists",
          "doc_count" : 4135
        }
      ]
    }
  }
}

```

### bucket_sort 에서 doc_count 사용하는 방법
```
GET outlog-20191204/_search?typed_keys
{
  "size" : 0,
  "_source" : false,
  "stored_fields" : "_none_",
  "query" : { "term" : { "resBody.userId" : { "value" : "ssfshop", "boost" : 1.0 } } },
  "aggregations" : {
    "rcmdTypeCode": {
      "terms": {
        "field": "resBody.rcmdInfoList.rcmdTypeCode",
        "size": 10
      }, 
      "aggs": {
        "userId": {
          "terms": {
            "field": "resBody.userId",
            "size": 1
          }, 
          "aggs": {
            "rcmdPcode": {
              "terms": {
                "field": "resBody.rcmdInfoList.pcode", 
                "size": 1000
              }
            }, 
            
            // _count 내장 필드가 버킷 집계 결과에서 doc_count 를 의미함
            "cntdesc":{"bucket_sort":{"sort":[{"_count": {"order": "desc"}}]}}
          }
        }
      }
      
    }
  }
}
```
