# 안드로이드에서 Context의 의미
> Context is the Interface to global information about an application environment. - Android Developer
> 컨텍스트는 애플리케이션의 environment에 대한 전역 정보를 위한 인터페이스이다. 

안드로이드 코드에서 메소드의 매개변수로 Context를 넘겨야 하는 경우가 종종 있었다. Context가 정확히 무엇이고 어떤 역할을 하는지 알아보고자 한다. 쉽게 말하면 Context는 "현재 사용되고 있는 어플리케이션(또는 액티비티)에 대한 포괄적인 정보를 지니고 있는 객체"로 이해할 수 있다. Context를 참조하는 방법은 `getBaseContext()`, `getApplicationContext()`, `ActivityName.this` 를 사용하여 참조할 수 있다. 이 세 가지의 방법은 차이가 있다. [참고](http://stackoverflow.com/questions/10347184/difference-and-when-to-use-getapplication-getapplicationcontext-getbasecon)

## Application Context vs Activity Context

안드로이드 프레임워크에서 Context는 다음과 같이 두 가지로 구분지을 수 있다.
       
Application Context | Activity Context 
--------------------|-------------------
application life-cycle에 접목되는 개념 | activity life-cycle에 접목되는 개념
하나의 애플리케이션의 실행-종료까지 동일한 객체 | 액티비티가 `onDestroy()`된 경우 사라지는 객체
`getApplicationContext()`, `getApplication()` | `getBaseContext()`, `ActivityName.this`

위 표와 같이 Application Context는 애플리케이션 자체의 생명주기에 영향을 받는다. 따라서 항상 애플리케이션의 생명 주기와 함께 한다. 그러나 Activity Context는 액티비티의 생명주기와 함께 작동해, `onDestroy()`와 함께 사라진다. 즉, 액티비티에 대한 환경 정보들이 Context에 있고, 이 Context(또는 Activity)에 `Intent`를 통해 다른 액티비티를 띄우면, 액티비티 스택에 쌓이게 되는 것이다.

이 두가지 Context를 잘 구분하여 목적에 맞도록 알맞은 종류의 Context를 참조하도록 해야 되는데, View를 manipulate 할 경우에 activity Context 를 참조하고 기타의 경우 application Context를 참조하면 된다. 

## getContext(), getApplicationContext(), getBaseContext(), this
- Activity.getApplicationContext() : 현재 활성화된 Activity만이 아닌 Application 전체에 대한 Context가 필요한 경우에 사용한다.
- View.getContext() : 현재 활성화된 activity에 대한 Context 참조 시 사용. `this`를 사용하는 것과 같은 맥락이다.
- ContextWrapper.getBaseContext() : 어느 Context에서 다른 Context를 참조해야 하는 경우 `ContextWrapper`객체를 사용하는데, 그 `ContextWrapper` 안에 있는 context를 참조하는 경우에 사용한다.
