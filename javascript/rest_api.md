# [REST API](http://meetup.toast.com/posts/92) - Representational State Transfer API

## 1. REST API의 탄생
REST는 Representational State Transfer라는 용어의 약자로서 2000년도에 **Roy Fielding** 에 의해 최초로 소개되었다. **Roy Fielding**은 HTTP의 주요 저자 중 한 사람으로 그 당시 웹(HTTP) 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 REST를 발표하였다고 한다. 

## 2. REST 구성
REST API는 다음의 구성으로 이루어져 있다. 
- 자원(Resource) - URI
- 행위(Verb) - HTTP METHOD
- 표현(Representations) 

## 3. REST의 특징
1) Uniform (유니폼 인터페이스)
Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말한다. 

2) Stateless (무상태성)
REST는 무상태성 성격을 갖는다. 다시 말해 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다. 때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다. 

3) Cacheable (캐시 가능)
REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용하는 것이 가능하다. 따라서 HTTP가 가진 캐싱 기능이 적용 가능하다. HTTP프로토콜 표준에서 사용하는 `Last-Modified` 태그나 `E-Tag`를 이용하면 캐싱 구현이 가능하다. 

4) Self-descriptiveness (자체 표현 구조)
REST의 또 다른 큰 특징 중 하나는 REST API 메세지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현구조로 되어 있다는 것이다.

5) Client-Serve 구조
REST 서버는 API제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.

6) 계층형 구조
REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다. 

## 4. REST API 디자인 가이드 
REST API설계 시 가장 중요한 항목은 다음의 2가지로 요약할 수 있다. 
```
항목 1. URI는 정보의 자원을 표현해야 한다.
항목 2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.
```
### 4-1. REST API 중심 규칙
1) URI는 정보의 자원을 표현해야 한다.(리소스명은 동사보다는 명사를 사용)
```
GET /members/delete/1
```
위와 같은 방식은 REST를 제대로 적용하지 않은 URI이다. **URI는 자원을 표현하는데 중점을 두어야 한다.** delete와 같은 행위에 대한 표현들이 들어가서는 안된다. 

2) 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현한다.
위의 잘못 된 URI를 HTTP Method를 통해 아래와 같이 수정할 수 있다.
```
DELETE /members/1
```
회원정보를 가져오는 URI는 다음과 같이 작성하면 된다.
```
GET /members/show/1		(x)
GET /members/1			(o)
```

회원을 추가할 때의 URI는 다음과 같이 작성한다.
```
GET /members/insert/2	(x)	- GET 메서드는 리소스 생성에 맞지 않다.
GET /members/2			(o)
```

#### [참고]HTTP METHOD의 알맞은 역할
METHOD  | 역할
--------|----------
POST	| POST를 통해 해당 URI를 요청하면 리소스를 **생성**한다.
GET		| GET을 통해 해당 리소스를 조회한다. 리소스를 **조회**하고 해당 도큐먼크에 대한 자세한 정보를 가져온다.
PUT 	| PUT을 통해 해당 리소스를 **수정**한다.
DELETE 	| DELETE를 통해 리소스를 **삭제**한다.

다음과 같은 식으로 **URI는 자원을 표현하는 데에 집중**하고 **행위에 대한 정의는 HTTP METHOD를 통해 하는 것**이 REST한 API를 설계하는 중심 규칙이다. 
























