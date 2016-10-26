# Activity

> Activity 는 일종의 Application 구성 요소로서, 사용자와 상호작용할 수 있는 화면을 제공한다. Activity 마다 창이 하나씩 주어져 사용자 인터페이스를 끌어올 수 있다. 
이 창은 일반적으로 한 화면을 가득 채우지만, 작은 창으로 만들어 다른 창 위에 띄울 수도 있다.

> 하나의 Application 은 여러 개의 Activity 가 느슨하게 묶여 있는 형태로 구성된다. 각각의 Activity 는 여러 가지 작업을 수행하기 위해 또 다른 Activity 를 시작할 수 있다. 
새로운 Activity 가 수행되면 이전에 수행되던 Activity 는 중단되지만, 중단된 Activity 는 'Back Stack' 에 보존된다. 



## Activity 생성

Activity 를 생성하려면 Activity 의 하위 클래스를 생성해야 한다. 
이 하위 클래스에서는 Activity 의 생성, 중단, 재개, 소멸 시기 등과 같은 생명 주기의 다양한 상태 간 Activity 가 전환될 때 시스템이 호출하는 콜백 메소드를 구현해야한다.

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

## [매니페스트 (Menifest)](https://github.com/singhee/TIL/blob/master/android/menifest.md)에서 Activity 선언하기
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

