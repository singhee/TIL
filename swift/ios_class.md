# 지나칠 수 있는 Swifft 클래스
## 앱에 구현된 클래스 
### UIApplication
iOS앱에 하나의 앱에 대해 반드시 하나의 UIApplication이 있고 싱글톤으로 구현되어 있다. 인스턴스는 sharedApplication클래스와 메소드의 호출에 의해 얻어진다.

#### 역할
1) 윈도우 관리: 상태표시줄을 표시하는 윈도우와 앱 본체를 이동하는 윈도우 2개를 관리하고 있다. 개발자는 앞의 윈도우에 접근한다.

2) 이벤트 전송: 이벤트가 발생하면 이벤트 큐에 모으고 있고 이벤트큐에 쌓인 이벤트를 이벤트 루프가 차례대로 꺼내서 UIApplicationDelegate객체와 UIWIndow객체로 전송한다. 이에 따라 이벤트 메소드가 호출된다.

### 이벤트
#### 종류
크게 두가지 라이프사이클 이벤트와 사용자 액션 이벤트가 있다.

* 라이프사이클 이벤트: 기기 및 앱에서 일하는 이벤트, 예로 전화가 오면 알림이 나오는 앱 시작화면의 표시등에 대한 이벤트를 말한다.
* 사용자 액션 이벤트: 화면 터치 및 기기를 기울이는 등에 대한 이벤트

#### 주요 속성
* idleTimerDidabled : 자동 절전모드 설정
* applicationIconBadgeNumber : 앱배지 설정
* networkActivityIndicatorVisible : 표시관련 (화면, 시간, 배터리, 통신상태등의 표시) 스타일 설정
* statusBarHidden : 상태표시줄 표시 설정

#### 주요 메소드 
* openURL : 지정한 사이트를 호출
* sharedApplication : 인스턴스 리턴

#### 주요 델리게이트 메소드
* applicationWillResignActive : 앱이 활성화되기 직전에 호출
* applicationDidEnterBackground : 앱이 백그라운드되었을 때 호출
* applicationWillEnterForeground : 앱이 백그라운드에서 포그라운드되기 직전에 호출
* applicationDidBecomeActive : 앱이 활성화될 때 호출
* applicationWillTerminate : 앱이 백그라운드 실행중에 종료될 때 호출

### UIApplicationDelegate 프로토콜
앱에서 통지되는 이벤트 메소드가 정의되어 있다.

#### 이벤트 구동
* 앱 및 기기에서의 동작에 대한 처리하는 실행파일수
* 이벤트가 발생하면 운영체제가 감지하여 이벤트에 따라 특정 객체 메소드가 호출됨
* 이벤트 구동에서는 해당 이벤트에 의해 호출되는 메소드에서 프로그램을 구현함

### UIView
* 화면의 사각형 영역을 표시하는 기능을 제공한다.

### UIWindow
* 윈도우 표시 기능을 제공함
* UIView를 상속하고 view역할을 함
* 사용자가 실행한 탭과 흔들기등의 조치를 받아 이벤트를 처리할 수 있는 객체를 찾고 보냄

### UIViewController
* 화면에 표시되는 뷰 모니터링 및 ViewController의 전환을 제공함

### UIStoryboard
* Interface Builder의 스토리보드 파일에 저장되어 있는 뷰 컨트롤러 객체를 저장하는 기능을 제공함


## 개발자가 작성하는 클래스
### AppDelegate
* UIApplicationDelegate프로토콜을 구현한 클래스
* 앱 라이프사이클 이벤트에 따라 UIApplication 객체가 AppDelegate객체의 이벤트 메소드를 호출함

### 앱 시작시 각 클래스의 실행형태
1. main()함수가 UIApplicationMain()호출
2. UIApplicationMain()이 UIApplication클래스의 인스턴스를 생성
3. UIApplicationMain()이 AppDelegate클래스의 인스턴스를 생성
4. UIApplicationMain()이 info.plist를 읽어들임
5. UIApplication객체가 이벤트루프를 실행함
6. UIApplication객체가 Storyboard를 읽어들임
7. UIApplication객체가 ViewController클래스의 인스턴스를 생성함
8. AppDelegate객체가 UIWindow클래스의 인스턴스를 생성함
9. AppDelegate객체의 application: didFinishLaunchingWithOptions 메소드 호출

### 뷰 계층
* 상단
* View : 버튼등 화면 컴포넌트
* Window : 사용자 액션을 받고 전달
* sharedApplication : window관리 및 이벤트 감지 및 전송
* 하단
 
## Storyboard 관련 클래스
### UIStoryboard
* 스토리보드 파일에 저장되어 있는 뷰컨트롤러 객체를 저장하는 기능을 가진 클래스
* 화면별 UINib객체로 분할하여 관리함

### UINib
* 화면 디자인에 대한 정보를 유지하는 기능
* 이 클래스가 UIViewController객체를 생성함

### UIStoryboardSegueTemplate
* ViewController전환 정보를 관리하는 클래스

### UIStoryboardSegue
* 전환전 및 전환대상 UIViewController객체를 관리하는 클래스
* 전환시 UIStoryboardSegueTemplate객체에 의해 인스턴스화함

스토리보드파일을 앱시작시 우선 XML파일을 읽어들이고 화면의 디자인정보를 UIStoryboard객체가 viewController의 전환정보를 UIStoryboardSegueTemplate를 유지한다. 이런 모든 정보를 한번에 관리할 정보량이 너무 많기 때문에 화면별 UINib객체로 분할하여 정보를 관리하고 있다.

## 화면 전환시 각 클래스의 실행
1. 사용자 액션에 의한 이벤트 통지를 받고 viewController에 연결된 UIStoryboardSegueTemplate객체의 perform메소드가 호출됨
2. UIStoryboardSegueTemplate객체의 UIStoryboard객체에서 전환하면 UIViewController객체 인스턴스를 얻음
3. UIStoryboardSegue객체를 생성하고 변환한 소스코드 및 전환대상 UIViewController객체 정보를 설정함
4. 원래 ViewController객체의 prepareForSegue메소드를 호출




