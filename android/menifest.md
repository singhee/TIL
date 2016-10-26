# Manifest
> AndroidManifest.xml 은 안드로이드 어플리케이션에 대한 각종 정보를 기술한 어플리케이션 명세서라 할 수 있다.
```
// 기술 내용

android application 을 위한 Java Package 정의한다.
Application 을 구성하는 컴포넌트들 (Activity, Service, Broadcast Receiver, Content Provider) 을 기술한다.
Application 을 구성하는 Component 들에 대한 Class name 정의, 처리할 수 있는 기능은 Intent-filter 를 정의한다.
Application 이 안드로이드 플랫폼의 제한된 API 에 대한 접근과 다른 Application 의 제한된 Component 를 사용하기 위해 필요한 권한 설정
외부에서의 자신의 Component 에 대한 사용 권한을 설정할 수 있다.
Application 구동을 위한 최소한의 SDK 버전을 정의한다.
Application 이 사용하는 다른 추가적인 라이브러리를 정의한다.
```	