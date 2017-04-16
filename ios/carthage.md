# Carthage 
`Carthage`는 Swift언어 기반의 라이브러리 관리도구이다. Cocoa Libarary 관리도구로 유명한 `CocoaPods`는 자동으로 작업공간을 생성하고 업데이트하는등의 다양한 기능이 있지만, `Carthage`는 심플하고 유연하고 불편한 것들을 없앤 도구이다.

그렇다면 왜 `Carthage`를 사용할까? 그것은 바로 **컴파일시간이 짧아지기 때문이다.** 이유는 `Carthage Library`를 설치하면 해당 Library를 미리 빌드하고 `.framework`을 만들어주기 때문에 `CocoaPods`에 비해 컴파일 시간을 단축시킬 수 있다. 만약 `Carthage`버전 Library가 있다면 `Cocoapods Library`를 사용하지 않는것이 좋다.
`Carthage`는 dynamic framework를 만드는 것으로 iOS8이상 대응이고 그 이하는 도입하기 어렵다. 그런 이유로 iOS앱 개발시 지원운영체제 버전을 iOS8이하버전도 고려한다면 Carthage는 사용할 수 없다.

## Cocoapod의 불편함
* 소스를 모두 받아서 컴파일 시 함께 빌드하는 방식을 사용하기 때문에, 프로젝트에 다른 open source들을 많이 사용한다면 (dependency 가 높다면) 컴파일 속도가 현저히 느려진다.
* `.workspace` 가 생기고 난 후, 원래 프로젝트에 더해 Pods프로젝트가 생기게 되며, 이 때문에 pod update를 하게 되었을 때 Pods 프로젝트와 내 프로젝트 모두에 영향을 끼친다.  
* Ruby Version, XCode Version, Mac OS Version 등 컴파일 환경에 영향을 크게 받는다.

## Carthage의 장점
* 커뮤니티 활성화가 높다(버그가 있으면 대체로 1일이내 수정 PR이 전송된다)
* 환경 의존도 낮다 (100% Swift로 되어 있어 CocoaPods와 다르게 각각의 환경에서 Ruby버전차이로 발생하는 문제가 없다)
* 단순하다 (Carthage 다운로드를 하면 바로 사용가능하다.)

## Carthage의 단점
`Carthage`의 단점도 물론 존재한다. 그것은 implementation 파일을 볼 수 없다는 점이다. 

