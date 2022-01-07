## 1. client-server architecture

### server = web server

- 24시간 동작 해야한다.
- 고정된 IP 주소를 갖는다.
- 거의 모든 서비스는 80번 port를 사용한다. why? 찾아가기 쉽도록.

- What transport service does an app need? (4가지)
  1. data integrity : 데이터 유실 없이 온전하게 목적지까지 도착
  2. timing : 언제까지 도달했으면 좋겠는지 (시간)
  3. throughput : 최소 용량 (양)
  4. security : 안전하게 도착

**transport layer는 data integrity에 대한 서비스만 제공해준다.**

_cf._
**IP** = 인터넷 상에 존재하는 컴퓨터를 지칭하는 주소
**PORT** = IP 중에서도 특정 process를 지칭하는 주소

## client = web browser

- 불규칙적으로 연결된다.
- 동적인 IP 주소를 갖는다.
- **do not communicate directly with each other (직접 통신 x)**

## 2. peer-to-peer(P2P) architecture

## HTTP

= hypertext transfer protocol

- **TCP** 사용
  따라서 HTTP 교환되기 이전 TCP connection을 해야한다.
- **stateless**
  = server는 client에 관한 상태 정보를 저장하지 않는다.
  단순히 요청 들어오면 응답 보내는 역할만 한다.

## HTTP connections

1. **non-persistent HTTP**
   메세지 주고 받고 TCP connection을 끊는다.

   TCP connection 👉 client sends HTTP request message 👉 server receives and sends response messae 👉 server, client closes TCP connection

   위의 과정을 10번 반복한다.

2. **persistent HTTP** ✅ HTTP 사용
   TCP connection을 재사용한다.
