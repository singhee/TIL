# ViewController

뷰 컨트롤러는 `UIViewController`의 하위 클래스 인스턴스로 다음과 같은 역할을 한다.
1. 뷰 계층 구조를 관리한다.
2. 한 뷰의 계층 구조를 구성하는 뷰 객체들을 만들고, 
3. 그 뷰 객체들과 관련된 이벤트를 처리한다.

## ViewController의 역할
View Controller는 말 그대로 View를 제어하는 컨트롤러 객체이다. View Controller는 view 프로퍼티를 가지고 있는데, 프로퍼티로 가지고 있는 뷰와 그 서브뷰의 레이아웃이나 모양, 컨텐츠를 변경할 수 있고 뷰 내의 컨트롤을 사용자가 조작할 때 호출되는 액션을 처리하는 등의 역할을 한다. 그 외에도 View Controller는 뷰의 라이프 사이클을 관리하고, 시스템으로부터 전달되는 메세지를 받아 이에 대해 대응하는 역할도 수행한다. 모든 View가 View Controller를 가질 필요는 없다. 주로 View Controller는 앱의 ‘전체화면’ 영역을 차지하는 뷰마다 1개씩 있으면 된다. (화면 전체를 차지하는 뷰의 서브 뷰들은 컨트롤러에서 아웃렛으로 처리하면 된다.) 그러나 특정한 서브 뷰의 라이프 사이클 관리가 필요 하거나, 뷰에서 많은 기능을 처리하는 경우에는 그에 대한 View Controller를 만들어 주는 것도 좋다. 


## ViewController와 View
ViewController는 뷰와 매우 밀접하게 연관되어 있다. 뷰 없는 ViewController는 없기 때문에 보통은 ViewController는 nib 파일을 통해서 뷰와 한 덩어리인 것처럼 취급되기도 한다. 따라서 별도의 nib 파일로 묶여서 세트를 이루는 경우가 많다. nib 파일(은 쉽게 쓰자고 만드는 것인데 쓰는 건 쉽지만 그 내용이 말로 설명하면 어렵다)을 사용하는 경우에 대해서는 설명을 조금 미루고, 여기서는 뷰 컨트롤러를 생성하는 방법에 대해 알아보자.

ViewController 역시 여느 객체와 마찬가지로 alloc, init을 통해서 생성하면 된다. 그런데 ViewController와 뷰 객체는 뗄레야 뗄 수 없는 관계이므로 ViewController의 뷰를 지정해 줘야 한다. 여기에는 두 가지 경우가 있을 수 있다.

1. 별도의 뷰 객체를 생성해서 setViwe: 메소드를 통해 ViewController의 뷰를 지정해준다.
2. 스토리보드를 사용한다.
3. loadView를 오버라이드하여, ViewController의 뷰를 스스로가 직접 만들도록 한다.
4. 아무것도 하지 않는다.

ViewController는 생성 직후에는 뷰에 대해 아무것도 하지 않는다. 통상 ViewController의 초기화 메소드에서도 자신의 뷰에 대해서 별다른 조치를 취하지 않아도 된다. ViewController에서 뷰는 외부에서 자신의 view 프로퍼티를 액세스하지 않는 이상은 크게 의미가 없기 때문이다. ViewController가 관리해야 하는 뷰가 따로 있다면, 첫번째 방법대로 ViewController를 만든 후, 뷰를 세팅해주면 된다.

만약 ViewController의 외부에서 view 프로퍼티를 액세스하면 해당 프로퍼티의 getter 메소드에 의해 뷰 객체를 리턴해줄 것이다. 그리고 그 시점까지 뷰가 nil 이라면 ViewController는 뷰를 그제서야 준비한다. 즉, 뷰가 필요한 시점에 결정된다. 

다시 ViewController -view 접근자를 보자. 이 프로퍼티가 nil이면 뷰 컨트롤러는 자신에게 loadView라는 메시지를 보낸다. 이 때 만약 ViewController가 스토리보드에 포함된 클래스라면, ViewController는 스토리보드 속에서 자신의 뷰를 찾아 그것을 로드해서 뷰 객체를 만든다. 그렇지 않은 경우라면 ViewController는 기본 UIView 클래스의 인스턴스를 임의로 하나 생성해서 자신의 뷰 속성에 세팅한다. 만약 스토리보드를 사용하지 않는다면 loadView 메소드를 오버라이딩하여 프로그래밍으로 ViewController의 뷰를 생성해주면 된다. 그러나 역시 가장 쉬운 방법은 nib 파일을 이용하는 것이겠다.


>nib 파일을 이용하는 경우에는 nib 파일로부터 뷰 컨트롤러를 초기화하는 순간, UIKit 프레임워크에 의해 nib 파일이 로딩되면서 nib 파일 내의 모든 최상위 객체들과 그 하위 객체들이 인스턴스화(instantiate)된다. 따라서 초기화와 동시에 뷰 및 뷰의 서브뷰가 생성되어 있는 상태가 된다.


## View의 Life Cycle
ViewController가 직접 관리하는 뷰(view 프로퍼티)는 ViewController와 밀접한 연관을 맺고 있는데, 그 중하나는 ViewController가 이 메인 뷰의 일종의 델리게이트처럼 동작하는 것이다. 뷰가 로드되고 화면에 나타나거나 사라지거나 하는 등 뷰의 라이프 사이클 상의 스테이지에 변화가 오면 ViewController는 이러한 변화에 따라 메시지를 받게 된다. 이러한 메시지는 view-로 시작하는 인스턴스 메시지들이 있다.

- `viewDidLoad` : 뷰가 메모리로 로드된 직후에 호출된다.
- `viewWillAppear` : 뷰가 컨트롤러의 뷰 계층구조에 추가되고 화면에 표시되기 직전에 호출된다.
- `viewDidAppear` : 뷰가 화면에 표시되면 노출된다.
- `viewWillLayoutSubviews` : 뷰의 bound가 변경되면 뷰는 하위 뷰의 레이아웃을 변경해야 하는데, 그 작업이 이루어지기 직전에 호출된다.
- `viewDidLayoutSubviews` : 뷰의 서브뷰 레이아웃이 변경된 후 호출된다.
- `viewWillDisappear` : 뷰가 화면에서 사라지기 직전 호출된다.
- `viewDidDisappear` : 뷰가 화면에서 사라진 후 호출된다.


> 뷰 컨트롤러가 시스템으로부터 메모리 경고를 받으면 뷰를 해제하려고 하는데, iOS6에서부터는 뷰 자체가 자동으로 메모리에서 해제되지 않는다. 따라서 iOS6이후의 환경에서는 `viewWillUnload`나 `viewDidUnload`는 더 이상 호출되지 않는다.








