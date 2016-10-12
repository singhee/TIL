# SIP(Session Initiation Protocol, 세션 설정 프로토콜)

```
 - VoIP(IP 네트워크를 활용하여 음성을 데이터 패킷으로 변환해서 통화를 가능하게 하는 통신 서비스 기술) 를 구현하기 위한 기술
 - 멀티미디어 통신에 있어 세션이나 호(Call)를 관리하는 Application Layer Protocol.
 - 멀티미디어 데이터 전송 자체보단 Signaling 을 통한 멀티미디어 통신 관리에 중점
 - 실시간 멀티미디어 데이터 전송은 RTP 가, 어플리케이션 레벨의 프로토콜은 SIP
 - SIP 에서 세션은 전화를 걸고 받기 위한 전화번화와 같은 정보를 송수신하는 것을 의미
 - H.323 (음성위주 서비스) -> SIP (멀티미디어 위주 서비스)
```

### SIP Session
1. Internet multimedia conferences (다자간 회의)
2. Internet telephone calls (음성 전화)
3. Internet video session (영상 전화)
4. Multimedia distribution (멀티미디어 분배)
5. Subscriptions and Nofitifations for Events (이벤트 신청 및 통지)
6. Publications of State (상태 정보 배포)

```
 * SIP Message  
 : SIP header ( 편지의 봉투 역할, 메세지 바디의 종류 표시)
 : message body 

```
### SIP 가 사용하는 전송 프로토콜 (Transport Layer) 
- TCP (Transport Control Protocol)
- UDP (User Data Protocol)

`SIP 사용 Port Number : 5060, 5061`

### SIP Component 
1. User Location : 통신에 참가할 단말 결정
2. User Availability : 통신에 참여할 착신측의 통화 가능여부 결정
3. User Capabilities : 통신간에 사용될 미디어 및 미디어 파라미터 결정
4. Session Setup : 착신측 및 송신측에 세션 파라미터 생성
5. Session Management : 세션의 종료 및 전환, 세션 파라미터 변경, 부가 서비스 연동

### SIP Server
1. Proxy Server : UAC(User Agent Client) 의 SIP 콜 받아 자신이 콜을 대신 만들어 줌
2. Redgister Server : 사용자의 에이전트로부터 레지스터 요청을 수신하여 사용자의 위치 정보를 유지
3. Redirect Server : 사용자가 직접 요청 할 수 있는 상대방의 URL 알려줌
4. Location Server : Proxy Server 나 Redirect Server 로 부터 SIP 콜의 목적지 노드의 주소가 요청되면 이를 Resoulution 해주는 역할을 함








