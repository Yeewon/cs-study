# Multiplexing and demultiplexing

## Multiplexing

multiple socket들을 하나로 만들어 주는 것 = 합치는 것
socket으로부터 data를 모으고 헤더정보를 캡슐화해서 network layer로 전달한다.

## demultiplexing

transport layer segment data를 올바른 socket으로 전달
**cf.** **segment = data + header** (source, dest port ◦◦◦)

### 1. UDP

connectionless demultiplexing

destination **IP**, **port number** 만을 사용한다. = 아무나 다 올 수 있다.

TCP, UDP, IP의 header 정보 매우 중요.

protocol의 동작을 이해하기 위해서는 정보가 중요하다.

### UDP header

source port #, dest port #, length, checksum (error check)

**UDP 역할**

1. multiplexing, demultiplexing
2. error check

### 2. TCP

connection-oriented demux

**source IP / port #, dest IP / port #**

4개 중 하나라도 다르면 다른 소켓으로 demux.

각각 client를 위한 고유의 socket을 생성해 놓는다. 👉 자원을 많이 소모한다.
