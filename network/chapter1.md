# chapter1

노드 개수 셀 수 없을 만큼 많다.
우리는 가장자리에 위치
가운데 = router

# 💡 network edge

### connection-oriented service

**TCP**

1. **reliable :** 신뢰성 있다.
2. **in-order byte-stream data transfer :** 순서 그대로 도착 지점에 도착
3. **flow control** : receiver에 맞게 전달해준다.
4. **congestion control :** 네트워크 상황에 맞춰서 능력치 만큼 보내준다**.**

단점

- 비용이 든다 (비용= computing resource, network resource)

**UDP**

1. connectionless
2. **unreliable data transfer**
3. no flow control
4. no congestion control

**그런데 왜 써?**
오디오 패킷(편지 봉투)이 몇개 유실되어도 관계 없는 음성 전화의 경우 사용한다.

**프로토콜**
정말 중요한 메세지를 주고 받기 위한 서로간의 준비 동작

# 💡network core

메세지 전달 방식 : packet, circuit switching
(인터넷에서는 packet swtiching 사용)

### 1. circuit switching

= end-end resources allocated to, reserved for ‘call’ between source & dest
= 가는 길을 미리 예약을 해놓고 특정 사용자만을 위해 만들어 놓은 것
예전 유선 전화망

_cf. circuit = channel_

### 2. packet switching

= store and forward
packet 단위로 받아서 그때 그때 보내준다.

**packet delay 4가지**

1. **processing delay**
   packet 검사, 목적지 확인
   → router 성능 향상하면 개선
2. **queueing delay**
   packet을 임시 저장하는 buffer, queue가 router에 존재
   → 제일 중요. 조절할 수 없다. delay의 근원이다.
   사람들이 몰리면 큐가 넘쳐서 packet loss가 발생한다.
   _TCP는 reliable 하다고 했는데? 어떻게 처리해?_
   → 재전송한다.

   1. 직전 router가 재전송  
      중간 router들은 no-brain이다.
      전달만 해준다.
   2. 처음부터 재전송 ✅

3. **transmission delay**
   packet은 Data이다 = bit의 집합
   td = packet length / link bandwidth
   → 케이블 bandwidth 늘린다.
4. **propagation delay**
   마지막 비트가 link에 올라와서 router 까지 도달하는 시간 = 빛의 속도

### packet vs circuit

1. circuit
   10명의 사용자만 사용이 가능
2. packet
   실제로 데이터 주고 받는 시간보다 아닌 시간이 많다.
   분산되어서 사용자에 제한이 없다.
   결국 확률이 중요하다.
