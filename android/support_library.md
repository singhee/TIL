# 안드로이드 Design Support Library
안드로이드 Design Support Libary를 얘기하기에 앞서, Material Design에 대해 알 필요가 있다.

## Material Design 
Material Design은 Google I/O 2014에서 소개된 구글의 디자인 철학이자 가이드라인이다. 모바일 뿐만 아니라 데스크탑을 아우르는 새로운 디자인 가이드라인을 제시함으로써 통일된 UI/UX를 제공하겠다는 것이 목적이다. Material Design은 플랫폼에 상관없이 매우 일관성 있는 디자인 경험을 제공할 뿐만 아니라, 가이드라인이 잘 제시되어 있어 적용하기 쉽다.

> "Material Design에서 표면과 그림자는 물리적인 구조를 형성하여, 사용자들이 화면 상의 어떤 부분을 터치할 수 있고 움직일 수 있는지 쉽게 이해할 수 있도록 돕습니다. 모든 움직임에는 의미가 있으며, 화면 요소들 간의 관계를 명확히 하고 세세한 디테일을 통해 사용자에게 이러한 관계를 알려줍니다." 
> ["구글 디벨로퍼 코리아 블로그 - 머티리얼 디자인 (Material Design) 이란?"](https://developers-kr.googleblog.com/2014/07/this-is-material-design.html)


## 안드로이드와 Material Design
Material Design이 Android 롤리팝과 함께 공개되면서 기본적으로 Material Design이 적용되었는데, 이 Material Design을 직접 안드로이드 앱 개발에 구현하는 것이 매우 어려웠다. v7-appcompat, cardview, recyclerview 등 일부 Material Design을 적용하기 위한 서Support Library가 제공되었지만, 그 기능이나 요소들이 매우 제한적이어서 별도의 Custom Library를 사용해야 했다. 

## 안드로이드 Design Support Library
위에서 언급한 Material Design의 불편함을 덜어주고자 구글은 Google I/O 2015에서 Material Design을 더욱 쉽게 적용할 수 있는 **'Android Design Support Libary'**를 공개했다. Design Support Library덕분에 Android 앱에 Material Design을 통일된 방식으로 쉽게 적용할 수 있게 되었다. 

### 안드로이드 Design Support Libary 시작하기
Design Support Library를 시작하기 위해서는 다른 라이브러리 처럼 `gradle`에 `dependency`를 추가해준다.
`compile 'com.android.support:design:24.2.0'`

### Navigation View
`Navigation Drawer`는 사용자들이 앱 내에서 각기 다른 Section들을 쉽게 오갈 수 있도록 해 주는 중요한 요소이다. 새로 추가된 `NavigationView`는 기존의 `DrawerLayout`내부에 위치하면서 이런 `Navigation Drawer`를 구현하는데 필요한 프레임워크와 함께 메뉴 리소스를 통해 쉽게 네비게이션 메뉴를 설정할 수 있도록 해 준다. 

기본 레이아웃은 다음과 같다. 
```
<android.support.v4.widget.DrawerLayout  
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <!-- your content layout -->
    <FrameLayout
        android:id="@+id/main_content_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <android.support.design.widget.NavigationView
        android:id="@+id/navigation_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:headerLayout="@layout/navigation_header"	// 1.
        app:menu="@menu/drawer" />	// 2.

</android.support.v4.widget.DrawerLayout>  
```
Navigation View에서 살펴볼 핵심 요소는 2가지 이다.

### Navigation View - HeaderLayout
`app:headerLayout` 속성은 `Drawer`의 header section의 레이아웃을 정의하는 데 쓰인다. 이 부분은 네비게이션 아이템들의 바로 위에 위치하게 되는데, 주로 프로필 섹션으로 사용된다.

### Navigation View - Menu
`app:menu` 속성은 `Drawer`내부에 사용될 네비게이션 메뉴를 지정하는 데 사용된다. 물론 `inflateMenu()`를 통해 코드를 이용하여 메뉴를 수동으로 구성할 수도 있다.


### Coordinator Layout
`CoordinatorLayout`은 터치나 그래그와 같은 이벤트가 일어날 때 여러개의 하위 view들이 서로 상호 작용을 할 수 있도록 해 주는 레이아웃이다.
`CoordinatorLayout`은 다음과 같은 두 가지 속성을 제공하여 화면에 있는 view들이 다른 view들을 기준으로 하여 상호작용 할 수 있도록 해 준다.
	
	1. layout_anchor : 다른 view를 anchor로 설정할 수 있다.
	2. layout_anchorGravity : 위에서 설정한 anchor를 기준으로한 gravity를 설정한다.

### App Bar
`CoordinatorLatout`을 활용하면 스크롤에 따른 view의 모양을 변화시킬 수 있다. 대표적인 예로 `App Bar`(예전의 Action bar)을 들 수 있다. 이미 우리는 레이아웃에서 `Toolbar`를 사용한다. Design Library는 `Toolbar`를 한 단계 더 발전시킨 `AppBarLayout`을 제공한다. 이것을 사용하면 `Toolbar`와 함께 사용되는 여러가지 레이아웃(예를들면 `TabLayout`)들이 다른 뷰에서 스크롤 이벤트가 일어날 때 `Toolbar`와 어우러져 자연스럽게 반응 하도록 할 수 있다.

```
<android.support.design.widget.CoordinatorLayout  
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

     <! -- Your Scrollable View -->
    <android.support.v7.widget.RecyclerView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior" />


    <android.support.design.widget.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

        <android.support.v7.widget.Toolbar
                  ...
                  app:layout_scrollFlags="scroll|enterAlways" />

        <android.support.design.widget.TabLayout
                  ...
                  />

    </android.support.design.widget.AppBarLayout>
</android.support.design.widget.CoordinatorLayout>  
```
위의 코드는 사용자가 `RecyclerView`를 스크롤 할때 `AppBarLayout`이 스크롤 이벤트에 응답할 수 있게 한다.
















































































