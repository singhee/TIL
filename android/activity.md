# Activity

> Activity 는 일종의 Application 구성 요소로서, 사용자와 상호작용할 수 있는 화면을 제공한다. Activity 마다 창이 하나씩 주어져 사용자 인터페이스를 끌어올 수 있다. 
이 창은 일반적으로 한 화면을 가득 채우지만, 작은 창으로 만들어 다른 창 위에 띄울 수도 있다.

> 하나의 Application 은 여러 개의 Activity 가 느슨하게 묶여 있는 형태로 구성된다. 각각의 Activity 는 여러 가지 작업을 수행하기 위해 또 다른 Activity 를 시작할 수 있다. 
새로운 Activity 가 수행되면 이전에 수행되던 Activity 는 중단되지만, 중단된 Activity 는 'Back Stack' 에 보존된다. 



## Activity 생성

Activity 를 생성하려면 Activity 의 하위 클래스를 생성해야 한다. 
이 하위 클래스에서는 Activity 의 생성, 중단, 재개, 소멸 시기 등과 같은 생명 주기의 다양한 상태 간 Activity 가 전환될 때 시스템이 호출하는 [생명주기 콜백 메소드](https://github.com/singhee/TIL/blob/master/android/lifecycle_callback.md)를 구현해야한다.

 	// 가장 중요한 콜백 메소드 두 가지
 	// 1. onCreate()	2. onPause()
	onCreate()
		: 이 메소드는 반드시 구현해야 한다. 시스템은 Activity 를 생성할 때 이 메소드를 호출한다. 
		: Activity 의 필수 구성 요소를 초기화 한다. 
		: setContentView() 를 호출해야 Activity 의 사용자 인터페이스 레이아웃을 정의할 수 있다.

	onPause()
		: 이 메소드를 호출한다는 것은 사용자가 Activity 를 떠난다는 첫 번째 신호이다. (Activity 소멸의 의미는 아니다.)


## 사용자 Interface 구현
한 Activity 에 대한 사용자 인터페이스는 `View` 클래스에서 파생된 개체가 제공한다. View 를 사용하여 레이아웃을 정의하는 가장 보편적인 방식은 Application 리소스에 저장된 XML 레이아웃 파일을 사용하는 것이다. 그러면, Activity 의 동작을 정의하는 소스코드와는 별개로 사용자 인터페이스 디자인을 유지할 수 있다. setContentView() 로 Activity 의 레이아웃을 설정하고, 해당 레이아웃의 리소스 ID 를 전달할 수 있다.

## [매니페스트 (Manifest)](https://github.com/singhee/TIL/blob/master/android/menifest.md)에서 Activity 선언하기
시스템에서 Activity 를 액세스할 수 있게 하려면, manifest 파일에서 선언해야 한다.
``` android
<manifest ... >
  <application ... >
      <activity android:name=".ExampleActivity" />
      ...
  </application ... >
  ...
</manifest >
```

## Activity 생명주기(Lifecycle)
콜백 메소드를 구현하여 Activity 의 생명주기를 관리하는 것은 유연한 Application 개발에 중요한 역할을 한다. Activity 의 생명주기는 다른 Activity 와의 관계, Activity 의 작업과 BackStack 등에 영향을 받는다.

Activity 의 상태는 기본적으로 `활성(Activity)`, `일시정지(Paused)`, `정지(Stopped)` 상태로 나눠진다.

	활성(Activity)
		: 현재 화면에 Activity 가 표시되는 상태이며, 사용자와 상호작용을 할 수 있는 상태를 말한다. 
		일반적으로 Activity 가 화면에 표시되고 있을 때 Activity 는 활성 상태이다. (running 상태라고도 한다.)

	일시정지(Paused) 
		: 화면에서 Activity 가 보이지만, 사용자와 상호작용은 할 수 없는 상태이다. 
		즉, 배경이 투명한 Activity 나 화면 전체를 가리지 않는 다른 Activity 에 의해 Activity 의 일부가 가려진 경우가 해당된다.
		Dialog 또한 Activity 의 일부를 가리지만, 이는 Activity 의 일부이기 때문에 일시정지 상태로 바뀌지 않는다.

	정지(Stopped)
		: 다른 Activity 에 의해 완전히 가려진 상태이다. 정지된 Activity 도 여전히 살아있는 Activity 이다. 
		  (Activity 개체가 메모리에 보관되어 있고, 모든 상태와 정보를 유지하지만 창 관리자에 붙어있지 않다.)


	Activity 가 일시정지 또는 정지된 상태이면, 시스템이 이를 메모리에서 삭제할 수 있다. 
	그러기 위해서는 종료를 요청(finish()) 하거나 해당 Activity 의 프로세스를 중단 시킨다.


 - 하나의 Activity 의 Entire Lifetime 은 `onCreate()` 와 `onDestroy()` 호출 사이를 말한다. Activity 는 onCreate() 에서 전체 상태에 대한 설정을 수행한 다음 나머지 리소스를 모두 `onDestroy()` 에 해제해야 한다. 

 - Activity 의 Visible Lifetime 은 `onStart()` 에서 `onStop()` 호출 사이를 말한다. 이 사이에서는 사용자가 Activity 를 화면에서 보고 상호작용을 할 수 있다.

 - Activity 의 Foreground Lifetime 은 `onResume()` 에서 `onPause()` 호출 사이를 말한다. 이 사이에서는 해당 Activity 가 화면에서 다른 모든 Activity 앞에 표시되며 사용자 입력도 해당 Activity 에 집중된다.



![Figure_1. Activity Lifecycle](../images/activity_lifecycle.png)

 











