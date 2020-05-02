### 주요 키워드
- 확률적 자료구조 기반 알고리즘을 사용함
- 근사치
- approximation aggregation 에 속함
- precision_threshold (0 ~ 40,000)
- 일반적으로 cardinality가 precision 보다 적다면 거의 항상 100% 정확

### 참고자료 1
https://peung.tistory.com/2
```
엘라스틱서치 에서 제공하는 approximation aggregation으로 필드에서 고유한 값의 대략적인 개수를 계산한다.

HyperLogLog++(HLL) algorithm (확률적 자료구조 기반 알고리즘)
엘라스텍서치의 cardinality 는 HLL 알고리즘을 기반한다.

HLL 알고리즘의 특징 : 

정확도(precision) 설정 가능
단, 정확도를 올릴 수록, 메모리 사용량이 증가
cardinality가 낮은 집합일 수록 정확도가 높아진다.
메모리 사용량이 고정적
메모리 사용량은 정확도에 비례

정확도 설정

precision_threshold (0 ~ 40,000)
일반적으로 cardinality가 precision 보다 적다면 거의 항상 100% 정확
메모시 사용률 =  precision_threshold * 8 bytes
```

### 사용 예
```
GET shopdata_mobile_main_20200228/_search
{
  "track_total_hits": true,  # total hits doc 개수 표시 여부
  "size": 0,  # hits doc 문서 표시 개수, 집계 결과만 응답으로 원하는 경우 0
  "aggregations": {
    "approxUniqueUseridCount": {  # 집계 이름
      "cardinality": {  # 유사 집계 중 카디날리티 집계
        "field": "userid",   # 집계 필드
        "precision_threshold": 10000  # 정확도, size 개수보다 크면 거의 100% 정확한 값
      }
    }, 
    "groupby": {  # 집계 이름
      "terms": {  # terms 집계
        "field": "userid",  # 검색할 필드
        "size": 10000,  # 검색할 최대 버킷 개수
        "shard_size": 50000,  # 설정 안하거나 값이 무효한 경우, size * 1.5 + 10 기본값 사용
        "show_term_doc_count_error": true,   # 각 버킷 결과에 doc_count_error_upper_bound 를 표시함
        "order": {
          "_count": "desc"  # 버킷 정렬 조건
        }
      }
    }
  }
}

# response
{
  "took": 966,  # 검색에 소요된 ms
  "timed_out": false,
  "_shards": {
    "total": 12,  # 검색에 사용된 인덱스의 총 샤드 개수
    "successful": 12,  # 검색에 사용된 인덱스의 샤드 개수
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 59256535,  # 검색에 사용된 인덱스들의 총 doc 개 수
    "max_score": 0,
    "hits": []
  },
  "aggregations": {
    "approxUniqueUseridCount": {
      "value": 3397
    },
    "groupby": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": "ssgcom",
          "doc_count": 7721470,
          "doc_count_error_upper_bound": 0
        },
        {
          "key": "ssgcom1",
          "doc_count": 7089744,
          "doc_count_error_upper_bound": 0
        },
        .....skip...
```
