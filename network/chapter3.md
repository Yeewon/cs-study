`transport → application layer` **service**

1. multiplexing
2. error checking

## Multiplexing

multiple socket들을 하나로 만들어 주는 것 = 합치는 것

socket으로부터 data를 모으고 헤더정보를 캡슐화해서 network layer로 전달한다.

## demultiplexing

transport layer segment data를 올바른 socket으로 전달

**cf.** **segment = data + header** (source, dest port ◦◦◦)

## 1. UDP

connectionless demultiplexing
destination **IP**, **port number** 만을 사용한다. = 아무나 다 올 수 있다.
TCP, UDP, IP의 header 정보 매우 중요.
protocol의 동작을 이해하기 위해서는 정보가 중요하다.

### UDP header

source port #, dest port #, length, checksum (error check)

**UDP 역할**

1. multiplexing, demultiplexing
2. error check

## 2. TCP

- connection-oriented demux

**source IP / port #, dest IP / port #**
4개 중 하나라도 다르면 다른 소켓으로 demux.
각각 client를 위한 고유의 socket을 생성해 놓는다. 👉 자원을 많이 소모한다.

### 특징

1. **point-to-point** : sender socket 하나와 receiver socket 하나의 통신을 책임
2. **reliable, in-order byte stream** : 유실되지 않고 순서대로 byte 단위로
3. **pipelined** : window 사이즈 만큼 한꺼번에 쏟아 붓는다.
4. **full deplex data** : 데이터가 양방향으로 흐른다.
5. **connection-oriented**
6. **flow controlled** : receiver가 받을 수 있는 만큼 보낸다.
7. **cumulative acks**
8. **single retransmission timer** : 타이머 1개 사용

### TCP segment structure ⭐

**receive window** : receive buffer에 얼만큼 빈 공간이 있는지 계속 상대방에게 feedback을 줘야한다. 이 공간 크기를 위함
**seq = 42**
data의 첫 byte의 순서번호
**ACK 79** = 78번 까지 잘 받았다. 79번 내놔라.
**TCP timeout value 설정**

- too long : segment loss에 너무 느린 리액션
- too short : 리액션은 짧지만 불필요한 재전송

**RTT ( Round Trip Time, Timeout)**
다른 router를 지나고, queueing delay 때문에 RTT가 모두 다르다.

### flow control ⭐

`sender`가 `receiver`가 처리하지 못할 정도의 data를 보내는 것을 방지한다.

- **그렇다면 보내는 양? 보내는 속도?**
  속도 단위 → bps (bit/sec)
  단위 시간당 보내는 양이 늘어나면 속도도 빨라진다.
  결국 같은 개념이다.

💡 **HDR의 receive buffer size가 0이면?**
sender가 주기적으로 segment를 보낸다. _(대신 data 부분은 없이)_
**why?** ACK를 받아 receive buffer size check를 위해

### TCP 3-way handshake

3번의 메세지를 주고 받는다. = data를 주고받자고 예약

- **why 3?**
  if 2-way handshake
  server는 client가 대답을 들었는지 확인 불가능하다.

**closing a connection**
👉 FIN(bit=1)을 보낸다.

**끝나도 timed wait 유지.**

- **why?**
  client 가 보낸 ACK 가 유실되면 server는 timeout되어 data를 다시 보낸다. 따라서 ACK가 확실히 갈 때까지 기다린다.

💡 sender는 network와 receive 중 상태가 안 좋은 것에 맞춘다.
계속 check 해야한다.
**receive**는 `flow control` 로 정확히 알 수 있고,
**network**는 `congestion control`로 정확히 알 수 있다 .

### congestion control

network는 public(공공 자원)이다.
접속량이 증가하면 막힌다. 재전송이 많이지면 더 막힌다.
따라서 TCP는 네트워크가 막힐 것 같으면 속도를 줄이고 반대인 경우에는 속도를 높인다.

### TCP congestion control ( 혼잡 제어 )

1. **slow start**
   네트워크 상황을 모르기 때문에 적은 양을 보내어 시작한다.
   너무 천천히 증가시키면 오래 걸리기 때문이다.
2. **additive increase**
   `threshold` 까지 `exponential` 하게 증가시킨다.
   이후는 `linear` 하게 증가.
3. **multiplicative decrease**
   ❗**TCP의 packet loss 탐지**
   1. **timeout** 👉 처음부터 시작
   2. **3 duplicate ACK** 👉 절반으로 줄인다.

`**cwnd** = congestion window`

cwnd 변동이 심하다. cwnd에 좌우된다.
이는 네트워크 상황에 따라 변화한다.

## Principles of Reliable Data Transfer

**RDT** Protocol(Reliable Data Transfer Protocol)

- rdt 1.0 : error 없다.
- rdt 2.0
  - **Error detection**
    checksum으로 bit error 확인
  - **Feedback**
    ACK(correctly), NAK(had errors)
  - **Retransmission** (재전송)
    몇 개 필요? 2개 (비교만 가능하면 된다.)
- rdt 2.1
  **ACK, NAK error → seq #**
  duplicat packet 핸들링을 위해 sequence number를 사용한다.
- rdt 2.2
  굳이 ACK/NAK 사용? → **ACK만 사용. + seq #**
- rdt 3.0
  **rdt 2.2 + sender에 timer 추가**
