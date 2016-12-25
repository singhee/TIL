# [Computer Network] 

## 네트워크 계층(Network Layer)
1. sender host에서 receiver host로 패킷 전달
- sender host는 트랜스포트 계층에서 세그먼트를 받아 데이터그램(datagram)을 캡슐화
- receiver host는 데이터그램에서 세그먼트를 추출하여 트랜스포트 계층에 전달
2. 모든 host와 router에 네트워크 계층 프로토콜 내장
3. router는 입력 링크의 IP 데이터그램의 헤더 필드를 조사하여 출력 링크로 전달

### 네트워크 계층의 주요 기능
1. forwading
- 라우터의 입력 링크에 도착한 패킷을 적절한 출력 링크로 이동
2. routing
- 출발지에서 목적지까지 path 결정
- routing alogorithm

### 연결 설정(Connection Setup)
1. 일부 네트워크 계층에서의 또 다른 주요 기능은 연결 설정
- 인터넷을 제외한 ATM, frame relay, X.25
2. 데이터그램이 전송되기 전에 두 host와 전달되는 라우터들간에 가상 연결(virtual connection) 설정
3. Network 와 Transport Layer의 계층 연결 서비스
- Network Layer : 두 host간, 가상 연결인 경우 전달되는 라우터 포함
- Transport Layer : 두 process 간

### 네트워크 서비스 모델
1. 개별 패킷(데이터그램)을 위한 서비스
- guranteed delivery 
- 특정 지연 시간 이내의 보장된 전달
2. 패킷 흐름(flow)을 위한 서비스ㅡ
- in-order된 패킷 전달
- minimum bandwidth
- 보장된 패킷 간 간격 : 두 개의 패킷 전송 사이 sender의 시간 간격이 receiver에서의 시간간격과 같음을 보장

