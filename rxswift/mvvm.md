# MVVM(Model-View-ViewModel) in Swift 
MVVM은 Microsoft의 John Gossman이 WPF(Windows Presentation Foundation)와 Siverlight의 아키텍쳐중 하나로 2005년 자신의 블로그에 공개를 했으며, 현재도 WPF와 Siverligh쪽에서 많이 쓰이고 있는 패턴이다.

![Figure_1. MVVM Pattern](../images/MVVM.png)
> [사진출처 - Microsoft Developer](https://msdn.microsoft.com/en-us/library/hh848246.aspx)


## MVC의 한계
MVC 패턴은 가장 널리 사용된 코딩 패턴이자 iOS개발에서도 자주 쓰이는 패턴이다. MVC의 목적은 
첫째, Model과 View를 완전히 분리시켜 이들의 재사용성(Reusability)을 높이고, 둘째, Model과 View 각각의 역할을 분명히 하여 스파게티 코드(Separation of Concern)를 방지 하는 것이다. 이를 위해 앱의 비즈니스 로직과 데이터를 담당하는 코드는 Model에, UI를 담당하는 코드는 View에, 그리고 Model과 View를 이어주는 역할을 하는 코드들은 Controller에 따로 분리하도록 한다. 

MVC 에서 Controller는 Model에 저장될 데이터와, View에서 보여질 데이터를 업데이트하며 컨트롤한다. Model데이터가 변하면 Controller에게 Notify하고, View에 유저 액션이 들어올 경우 Controller에게 Delegate한다. Model과 View는 절대 서로를 알 수 없다. 그래야 독립성이 유지되어 재사용이 가능해지고, 코드가 엉키지 않는다. 

이러한 MVC구조는 분명 장점이 많지만 한계도 존재한다. 가장 대표적인 것이 Model에 넣기도 애매하고, View에 넣기도 애매한 코드들은 모두 Controller에 들어가게 되어 Controller가 비대해진다는 점이다. 이를 `Massive View Controller`의 문제라고도 한다. 이 문제를 해결하기 위해 MVVM패턴이 대안으로 제시되고 있다. 

## What is MVVM ?
MVVM은 MVC에서 Controller가 View Model로 교체된 형테이고 뷰모델은 UI레이어 아래에 위치한다. 그렇다고 MVVM에서 Controller가 사라진 것은 아니다. iOS의 경우 View Model은 Data Binding부분이 다소 취약해 Controller를 완전히 없앨 수는 없다. 대신, Controller는 ViewModel을 바라보고 있으며, ViewModel은 Model을 바라보고 있어야 한다. 새로 추가된 View Model은 Controller를 대신해 Formatting을 비롯한 코드들을 다루게 된다. 이에 따라 Controller의 부담이 줄어들게 된다. 

ViewModel은 뷰가 필요로 하는 데이터와 Command 객체를 노출해 주기 때문에 뷰가 필요로하는 데이터와 액션을 담고 있는 컨테이너 객체로 볼 수도 있다. 하지만, ViewModel은 View의 존재에 대해서는 모르기 때문에 Swift에서는 Delegate 또는 Closure를 이용하여 ViewModel의 데이터를 전달할 수 있다. 

따라서 MVVM의 주요 특징은 다음과 같다.
```
* Controller는 더 이상 Model에게 말을 걸을 수 없다. View Model을 통한다.
* Model과 View Model이 하나의 짝을 이루고, View와 Controller가 짝을 이루는 구조가 된다.
* View Model은 Presentation Logic을 다루게 된다. 하지만 UI는 다루지 않는다.(UIKit import 금지)
```

MVVM과 MVC의 가장 다른 점은 Command와 Data Binding이라고 할 수 있다. Command를 통하여 Behavior를 View의 특정한 ViewAction(Event)와 연결할 수 있으며, ViewModel의 속성과 특정 View의 속성을 Binding 시켜 줌으로써 ViewModel 속성이 변경 될때마다 View를 업데이트 시켜줄 수 있다. 이것이 MVVM 패턴의 특징이며, Controller와 View와의 의존성을 완벽히 분리 할 수 있다는 장점이 생긴다.

그리고 이를 더 효율적으로 할 수 있도록 하는것이 바로 [RxSwift](https://github.com/singhee/TIL/blob/master/rxswift/RxSwift.md) 이다.

## MVVM를 적용한 RxSwift
RxSwift는 ReactiveX(Rx) 라이브러리의 Swift버전이다. 일반적으로 iOS에서 객체간 소통하는 방법으로는 `Callback, Delegate, Notification` 이렇게 3가지가 있다. Async한 환경에서 이들 3가지 수단을 통해 객체가 업데이트된 상황을 객체 간 전달할 수 있다. 
RxSwift는 객체간 소통을 하는데 있어서 `Observable Pattern`을 사용한다. 이는 MVC에서 Model이 Controller에게 자신의 업데이트 상태를 전달해줄 때 Notification이나 Key Value Observing(KVO)를 이용하는것과 같은 패턴이다. RxSwift는 관찰하고 있는 값이 업데이트되는 것과 관련하여 Notification이나 KVO보다 더 간결하고 강력한 방법을 제공한다. MVVM에서도 객체간 자신의 업데이트 상태를 알려주는데 RxSwift를 이용할 수 있다. 

----------------------------------------------------------
[출처](http://blog.naver.com/PostView.nhn?blogId=jdub7138&logNo=220979742234&parentCategoryNo=99&categoryNo=&viewDate=&isShowPopularPosts=true&from=search)

