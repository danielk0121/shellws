## spdy
```
출처 : https://d2.naver.com/helloworld/140351

최근 Google의 SPDY 프로토콜을 Facebook에서도 지지하며 Facebook도 SPDY 구현을 시작했다는 기사가 발표됐습니다.

SPDY는 HTTP 2.0에 포함될 예정입니다.

SPDY는 'speedy'라는 단어를 기반으로 Google이 만든 조어로, Google이 자신들의 'Make the Web Faster' 노력의 하나로 제안한 새로운 프로토콜입니다. 

SPDY의 특징을 간단히 살펴보자.

항상 TLS(Transport Layer Security) 위에서 동작
HTTP 헤더 압축
바이너리 프로토콜
Multiplexing
Full-duplex interleaving과 스트림 우선순위
Server Push
웹 사이트를 재작성할 필요 없음

웹 사이트를 재작성할 필요 없음
server push와 같이 추가 구현을 요구하는 기능을 제외하면, SPDY 적용 자체를 위해 웹 사이트 자체가 바뀌어야 할 필요는 없다. 다만 브라우저와 서버가 SPDY를 지원해야만 한다. SPDY는 브라우저 사용자에게 완전히 투명하게 적용 가능하다. 즉, spdy:// 같은 프로토콜 scheme이 없다. 또한 브라우저에서도 SPDY 프로토콜 사용 여부에 대한 어떤 표시도 하지 않는다.
```