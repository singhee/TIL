#[Computer Network] Transport Layer

## 신뢰성 있는 데이터 전송의 원리 (Principles of Reliable Data Transfer)

### 파이프라인 프로토콜 (Pipelined Protocol)
1. Sender가 ACK응답을 기다리지 않고 여러 패킷을 전송한다.
2. SeqNum 의 범위는 증가하여 전송 중인 패킷은 유일한 SeqNum 을 가진다. 
3. Sender 측과 Receiver측이 패킷을 버퍼링 해야 한다. 

### Pipelining Protocol 개요
1. GBN (Go-Back-N) 
- Sender는 파이프라인에서 최대 N개의 ACK 없이 패킷 전송 허용한다.
- Receiver는 누적된 ACK(cumulative ACK)만을 전송, 수신된 패킷들의 SeqNum에 갭이 있으면 ACK 응답하지 않는다.
- Sender는 가장 오래된 '전송 되었지만 ACK 응답 없는 패킷'에 대한 타이머를 가진다. 타이머가 만료되면 ACK 응답 없는 모든 패킷들을 재전송한다.

2. Selective Repeat
 - Sender는 파이프라인에서 최대 N개의 ACK 없이 패킷 전송 허용한다.
 - Receiver는 개별 패킷들에 대해 ACK 응답한다.
 - Sender는 '전송 되었지만 ACK 응답 없는 패킷'들 각각에 대해 개별 타이머를 관리하며, 타이머 만료 시 ACK 응답 없는 패킷만 재전송한다.

## TCP(Tansmission Control Protocol)
### 1. Full Duplex Data 
- 같은 연결 상에서 양방향 데이터 흐름
- 최대 세그먼트 크기(MSS : Maximum Segment Size

### 2. Connection-Oriented
데이터 교환 전에 Sender, Receiver 의 상태를 초기화하는 핸드셰이킹(handshaking, 제어 메시지들의 교환)

### 3. Flow Contol 
Sender는 Receiver의 수신한계를 넘어서는 Send를 하지 않는다.

### 4. TCP Segment Structure
![TCP_Segment_Structure](../images/TCP_Segment_structure.jpg)
- Sequence Number : 세그먼트에서 첫 번째 바이트의 바이트 스트림 번호. 시작 SeqNum은 임의로 선택
- ACK : 상대방으로부터 기대하는 다음 바이트의 SeqNum. 누적 ACK(cumulative ACK)

## 신뢰적 데이터 전달(Reliable Data Transfer)
### TCP 신뢰적 데이터 전달
1. 비신뢰적인 인터넷 네트워크 계층(IP 서비스) 상위 계층에서 신뢰적인 데이터 전달 서비스를 제공한다.
- Pipelined Segment
- Culmulative ACKs
- 단일 Retransmission Timer

2. 재전송(Retransmission) 시점
- timeout 이벤트
- duplicated ACKs

3. Simplified TCP sender
- duplicated ACKs 무시
- 흐름제어, 혼잡제어 무시

### TCP Sender Event
1. 전송/재전송과 관련된 TCP Sender Event
- 상위 어플리케이션으로부터 수신된 데이터
- Timer Timeout
- ACK 수신

2. "상위 애플리케이션으로부터 수신된 데이터" Event
- 순서번호를 포함한 세그먼트 생성
- 순서번호는 세그먼트에서 첫 번째 바이트와 바이트 스트림 번호
- 타이머가 이미 동작중에 있지 않으면 타이머 시작
- 타이머의 Expiration Interval (TimeOutInterval)

3. "타이머 타임아웃" Event
- 타임아웃을 유발한 세그먼트를 재전송
- 타이머를 다시 시작

4. "ACK 수신" Event
- 이전에 응답 받지 못한 세그먼트의 ACK 이면, 해당 세그먼크를 ACK 응답된 세그먼트로 표시. 아직 ACK응답 받지 못한 세그먼트들이 존재하면 타이머를 시작

5. Fast Retransmit
- 타임아웃 주기가 상대적으로 길어질 수 있음(손실된 패킷을 다시 보내기 전까지 오래 기다림)
- 중복 ACK를 사용하여 손실된 패킷들을 감지. Sender는 종종 많은 개수의 세그먼트들을 연속적으로 보내기 때문에, 세그먼트가 손실되면 많은 중복 ACK가 발생
- Sender가 같은 데이터에 대해 3개의 중복 ACK를 수신하게 되면 ACK 응답된 세그먼트의 다음 세그먼트가 손실되었다고 가정. 타이머가 만료되기 전에 재전송



## 흐름 제어(Flow Control)
Sender가 너무 많은 데이터를 너무 빠르게 전송하여 수신자의 버퍼를 Overflow 시키는 것을 방지하는 서비스.

## 연결관리 (Connection Management)
### TCP 연결 관리 (TCP Connection Management)
- TCP Sender/Receiver는 데이터 교환 전에 Handshake
- 연결(Connection)을 설정(서로 연결할 의사가 있음을 확인)
- 연결 파라미터 값들 합의

### TCP 3-way handshake
1. Client는 Server에 SYN 세그먼트를 보낸다.
- 세그먼트 헤더의 SYN bit flag 세트
- 최초의 순서번호를 기술
- 데이터는 없음

2. Server는 SYN 을 받고, SYNACK 세그먼트를 응답한다.
- Server는 TCP 버퍼와 변수 할당
- Server의 최초 순서번호를 기술

3. Client는 SYNACK을 받고, ACK 응답한다.
- Client는 TCP 버퍼와 변수를 할당
- ACK 응답에 데이터(세그먼트 payload)가 포함될 수 있음


### TCP 연결 종료
1. Client는 Server에 FIN 세그먼트를 보낸다.
- 세그먼트 헤더의 FIN bit flag 세트
2. Server는 FIN을 받고, ACK 세그먼트를 응답, FIN 세그먼트를 보낸다.
3. Client는 FIN을 받고, ACK 세그먼트를 응답한다.
- 대시 시간(timed wait)동안 기다린 후 연결 종료됨
4. Server는 ACK을 받고, 연결을 종료한다.




## 혼잡제어의 원리(Principles of Congestion Control)
### 혼잡제어의 원리 
1. 혼잡(Congestion)
- 네트워크가 감당할 수 없을 정도로 너무 많은 source에서 너무 많은 데이터를 너무 빨리 보내는 것이 원인
- 혼잡 원인을 처리하기 위해서는 혼잡을 일으키는 Sender들을 억데하는 매커니즘이 필요
- 혼잡 제어는 흐름 제어와는 다르다

2. 네트워크와 혼잡의 결과
- 패킷 손실(라우터의 버퍼 overflow)
- 긴 패킷 지연(라우터 버퍼에서 queueing)

3. 혼잡제어는 네트워킹의 기본적인 중요한 문제의 상위 10개 목록에 포함된다.

### 혼잡제어에 대한 접근법
1. 종단간의 혼잡제어 (end-end congestion control)
- 네트워크 계층에서 혼잡제어를 위한 피드백이나 지원이 없다
- 종단 시스템에서 관찰된 패킷 손실 및 지연에 기초하여 혼잡 추측
- TCP가 사용하는 방법
2. 네트워크 지원 혼잡 제어(network-assisted congestion control)
- 라우터가 종단 시스템에게 혼잡 상태에 관한 피드백 제공
- 라우터가 Sender에게 자신의 전송률 전송(ATM ABR)

### ATM ABR 혼잡제어
1. ATM ABR(available bit rate)프로토콜
- 탄력적인 데이터 전송 서비스
- 네트워크 부하가 적은 경우 : Sender는 여분의 가용한 대역폭 이용
- 네트워크 혼잡할 때 : Sender는 미리 정해진 최소의 전송률로 억제
- ATM 용어 : 스위치(라우터), 셀(패킷)
2. RM 셀 (Resource Management cell)
- Sender가 보내고, 데이터 셀들 사이에 산재
- 스위치가 RM셀의 비트들을 세트(네트워크 지원)
- Receiver는 RM 셀을 Sender에게 다시 보냄









