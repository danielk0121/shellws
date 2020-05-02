# elasticsearch mapping example

### for outputlog-yyyyMMdd index
```
PUT _template/outputlog-ver2
{
  "version": 2,
  "index_patterns": [
    "outputlog-*"
  ],
  "settings": {
    "index": {
      "number_of_shards": "2",
      "refresh_interval": "5s"
    }
  },
  "mappings": {
    "_meta": {
      "created_by": "dkkwon, outputlog"
    },
    "properties": {
      "@timestamp": { "type": "date" },
	  "@version": { "type": "keyword" },
      "host": { "type": "keyword" },
      "type": { "type": "keyword" },
      "logtime": { "type": "keyword" },
      "kr_timestamp": { "type": "date" },
      "tags": { "type": "keyword" }, 
      "resBody": {
        "properties": {
          "auid": { "type": "keyword" },
          "cfox": { "type": "keyword" },
          "ad": { "type": "keyword" },
          "siteCode": { "type": "keyword" },
          "freq": { "type": "long" },
          "subad": { "type": "keyword" },
          "ergabt": { "type": "keyword" },
          "txid": { "type": "keyword" },
          "kr_date": { "type": "keyword" },
		  "platformCodeWM": { "type": "keyword" },
          "userId": { "type": "keyword" },
          "s": { "type": "keyword" },
          "keyIp": { "type": "keyword" },
          "itlTpCode": { "type": "keyword" },
          "success": { "type": "boolean" },
          "logtime": { "type": "keyword" },
          "platformCode": { "type": "keyword" },
          "sgType": { "type": "keyword" }, 
		  "frameCode": { "type": "keyword" },
          "gridCount": { "type": "long" },
          "rcmdInfoList": {
            "properties": {
              "parentPcode": { "type": "keyword" },
              "pcode": { "type": "keyword" },
              "rcmdPcode": { "type": "keyword" },
              "rcmdTypeCode": { "type": "keyword" }
            }
          },
          "message": {
            "type": "text",
            "fields": {
              "keyword": {
                "ignore_above": 256,
                "type": "keyword"
              }
            }
          }
        }
      }
    }
  }
}
```
