# elastic search 6.x 에서 nori 사용
- 사용하려면 플러그인을 설치해야 한다
- 플러그인 설치 후 재시작해야 한다
- 플러그인 설치 및 적용을 위한 재시작을 각 노드에서 각각 해줘야 함
  - 하나의 노드에서만 설치한다고 모든 노드에 적용되는 것이 아님
```
# 설치
bin/elasticsearch-plugin install analysis-nori

# 삭제
bin/elasticsearch-plugin remove analysis-nori

# 설치 확인
ls -al elasticsearch-6.4.3/plugins/analysis-nori

# 설치 테스트(nori analyzer 찾을 수 없다는 에러 발생시 엘라스틱 서치 노드 재시작)
curl -H 'Content-Type:application/json' -X POST http://localhost:9203/_analyze?pretty -d '{"analyzer":"nori","text":"노리 테스트 입니다."}'

{
  "tokens" : [
    {
      "token" : "노리",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "테스트",
      "start_offset" : 3,
      "end_offset" : 6,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "이",
      "start_offset" : 7,
      "end_offset" : 10,
      "type" : "word",
      "position" : 2
    }
  ]
}
```
