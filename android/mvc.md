# MVC (Model-View-Controller) 와 Android
	Android Application 은 MVC 라는 아키텍처에 맞추어 설계된다. 
	Application 의 어떤 객체든 모델 객체 또는 뷰 객체 또는 컨트롤러 객체가 되어야 한다는 것이 MVC 의 주요 관점이다.

## Model 객체
Model 객체는 Application 의 데이터와 '비즈니스 로직'을 갖는다. Model class 는 앱과 관계가 있는 것들을 모델링하기 위해 설계된다. Model 객체는 데이터를 보존하고 관리하는 것이 유일한 목적이기 때문에 사용자 인터페이스를 모른다. Application 의 모든 Model 객체들은 Model Layer 을 구성한다.

## View 객체
View 객체는 자신을 화면에 그리는 방법과 터치와 같은 사용자의 입력에 응답하는 방법을 안다. 쉽게 말해서, 화면에서 볼 수 있는 것이라면 그것은 View 객체이다. Android 는 구성 가능한 View class 를 풍부하게 제공한다. Application 의 View 객체들은 View Layer 을 구성한다. 

## Controller 객체
Controller 객체는 View 와  Model 객체를 결속시키며, '애플리케이션 로직'을 포함한다. Controller 객체는 View 객체에 의해 의해 촉발되는 다양한 이벤트에 응답하고, Model 객체 및 뷰 계층과 주고받는 데이터의 흐름을 관리하기 위해 설계된다. Android 에서 Controller 는 일반적으로 Activity 나 Fragment 또는 Service 의 서브 클래스이다.

