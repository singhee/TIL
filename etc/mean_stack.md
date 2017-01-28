# MEAN Stack 
MEAN Stack의 각 앞글자는 독립적인 기술 이름에서 철자를 따온 것이다.
각각 `MongoDB`, `Express JS`, `Angular JS`, `Node JS` 기술을 의미한다.

4가지 기술 모두 Javascript 언어에 그 뿌리를 가진다. 각 기술들이 동일한 Javascript 언어로 웹 어플리케이션을 제작하기 위한 모든 것을 작성 가능하다는 것은 큰 장점으로 다가올 수 있다. 과거에 서버는 JSP/PHP/ASP로 구성, 데이터베이스는 MySQL/MS-SQL/Oracle로 구성, 프론트엔드에는 javascript로 구성하는 방식들이 사용되었고, 현재에도 사용되고 있다. 하지만, 개발 후 유지/보수 차원에서 볼때, 단일 언어로 작성된 웹 어플리케이션은 매우 큰 장점을 가진다.

## Node.js
: 웹 서버와 서버측 스크리브, 기존 서버의 대부분의 기능을 작성할 수 있다.
 - 특징
 	- javascript end-to-end
 		- 자바스크립트로 서버, 클라이언트 스크립트를 둘 다 작성 가능
        - 클라이언트 개발자와 서버 개발자가 같은 언어를 사용할 수 있다.
        - 쉽게 서버에 적용 가능
  	- Event-driven scalability
		- 웹 요청을 동일한 쓰레드에서 처리 가능 (기존에 사용하던 아파치 서버에서는 요청에 따라 쓰레드를 생성, 요청을 처리하기 위해서는 복수개의 쓰레드가 기다려야 하는 상황)
		- 웹 서버의 규모를 조절가능하다.
  	- Extensibility
		- 새로운 모듈이 많고, 추가 적용방법이 간단
  	- Fast Implementation 
		- 개발하기가 쉽다

2. MongoDB
: "humongous"에서 유래, 가볍고 빠르며 규모 조절성이 뛰어난 NoSQL 데이터베이스이다.
- 특징
  - document orientation
  		- 문서 지향적, 데이터가 양쪽 스크립트에서 다루는 것과 아주 유사한 형태로 DB에 저장
		- 데이터를 레코드<-> 오브젝트 변환할 필요가 없다.
  - high performance
  - high availablity
		- 몽고DB의 복제 모델은 고성능을 유지하면서 높은 규모의 조절성을 유지 가능
  - high scablity
		- 데이터가 여러 서버에 분산(Sharding)됨으로, 수평적으로 규모를 조절하기 쉽다.
  . no SQL Ingection

3. Express
 : 웹 서버 역할, Node.js에서 실행, 모듈 설정, 구현, 제어가 쉽다.
 - 특징
 	- 경로 관리 : URL end point 를 정희하기 쉽다.
 	- 에러 처리 : document not found 에러와 그 외 다른 에러들을 처리하는 에러 핸들링 기능이 내장
 	- 쉬운 통합 
  	- 쉬운 쿠키 관리
  	- 세션 관리와 캐쉬 관리

4. AngularJS
: MVC 프레임워크를 사용해서 잘 설계된 좋은 구조의 웹 페이지와 애플리케이션을 구현 가능
 - 특징
 	- data binding
 		- scope 구조를 활용, 데이터를  HTML 요소로 완벽하게 결합 가능
	- clean code
  	- compatibility 
		- 기존 코드 재사용이 쉽고, jQuery 활용 등 호환성이 좋다.