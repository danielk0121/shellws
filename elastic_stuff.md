# aggregation bucket size 초과 에러 처리

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

