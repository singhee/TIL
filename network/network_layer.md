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

## Virtual Circuit 과 Datagram 네트워크
### Network Layer의 연결형 및 비연결형 서비스
1. Datagram Network : Network Layer에서 비연결형 서비스(connectionless service)만을 제공
2. Virtual Circuit Network : Network Layer에서 연결형 서비스(connection service)만을 제공
3. Transport Layer 서비스와 유사하지만 Network Layer서비스는 다음과 같은 차이가 있다.
- host 사이의 서비스
- 연결형이나 비연결형 서비스 하나만을 제공
- End point뿐만 아니라 Network core의 라우터에서도 구현

### Virtual Circuit
1. source와 dest 호스트 간의 경로가 전화망과 같이 동작
- 성능 지향형(performance-wise)
- 출발지와 목적지 경로를 따라 네트워크 동작
2. 데이터 전송 전에 각 콜(call)의 연결 설정, 해제
3. 각 패킷은 (목적지 호스트 주소가 아닌) VC 식별자(번호)를 포함하여 전송
4. 경로 상의 라우터는 현재 연결에 대한 상태를 유지
5. 링크, 라우터 자원(대역폭, 버퍼)들이 VC에 할당
- 전용 자원(dedicated resources) = 예측 가능한 서비스(predictable service)

### Datagram Network
1. 네트워크 계층에서 콜 설정이 없음
2. 라우터는 End point 간 연결에 대한 상태 정보 필요 없음
- 네트워크 수준에서의 "연결" 개념 없음
3. 패킷은 목적지 호스트 주소를 사용하여 전달
- 같은 source와 dest를 갖는 패킷들이 다른 경로를 거칠 수 있음
4. Forwarding Table 을 구성하여, 목적지 주소의 프리픽스(prefix)를 table entry와 대응
- 최장 프리픽스 대응 규칙(longest prefix matching rule) 적용. 가장 긴 대응 엔트리를 찾아서 해당 링크 인터페이스로 패킷을 전달
- 32비트 IP주소는 약 40억개 이상이기 때문에, 만약 라우터 Forwarding Table이 목적지 주소마다 하나의 entry를 갖는다면 아주 큰 테이블이 필요할 것이다




