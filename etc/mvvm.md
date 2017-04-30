# MVVM(Model-View-ViewModel)
MVVM은 Microsoft의 John Gossman이 WPF(Windows Presentation Foundation)와 Siverlight의 아키텍쳐중 하나로 2005년 자신의 블로그에 공개를 했으며, 현재도 WPF와 Siverligh쪽에서 많이 쓰이고 있는 패턴이다.

![Figure_1. MVVM Pattern](../images/MVVM.png)
> [사진출처 - Microsoft Developer](https://msdn.microsoft.com/en-us/library/hh848246.aspx)

MVVM은 MVC에서 컨트롤러가 뷰모델로 교체된 형테이고 뷰모델은 UI레이어 아래에 위치한다. 뷰모델은 뷰가 필요로 하든 데이터와 Command 객체를 노출해 주기 때문에 뷰가 필요로하는 데이터와 액션은 담고 있는 컨테이너 객체로 볼 수도 있다. 

MVVM과 MVC의 가장 다른 점은 Command와 Data Binding이라고 할 수 있다. Command를 통하여 Behavior를 View의 특정한 ViewAction(Event)와 연결할 수 있으며, ViewModel의 속성과 특정 View의 속성을 Binding 시켜 줌으로써 ViewModel 속성이 변경 될때마다 View를 업데이트 시켜줄 수 있다. 이것이 MVVM 패턴의 특징이며, Controller와 View와의 의존성을 완벽히 분리 할 수 있다는 장점이 생긴다.