# [Android] Frament 

## Fragment의 개념

Android에서 `Activity`의 기본개념은 **한 화면에 보이는 모든것을 관리하는 개념**이다. 다양한 디바이스, 다이나믹한 어플리케이션 개발을 위해서 Activity는 개념에 맞지 않아서 등장 한것이 `Fragment`라는 개념이다. 

기존 Activity는 *하나의 화면에 여러개 사용 할 수 없게 설계* 되어 있는 반면, Fragment는 **Activity와 비슷한 Lifecycle을 가지면서 여러가지 화면을 넣을 수 있는 방법을 지원**해준다. Fragment는 Android 3.0(Honeycomb)부터 API를 지원해 왔으며 그 이하 버전에서는 Support.v4의 FragmentActivity를 사용하면 동일하게 사용가능 하다.

![Figure_1. Android Fragment](../images/android_fragment.png)

## Fragment의 특징
1. Fragment는 Activity와 비슷한 LifeCycle을 가진다.
2. 하나의 Activity에서 다수의 Fragment를 사용할 수 있다.
3. Fragment는 Activity에서만 존재하며, 단독으로 실행 될 수 없는 구조이다.
4. Fragment는 Activity와 마찬가지로 **Back Stack**을 사용할 수 있으나, Activity처럼 다양한 Stack방식을 지원하지 않는다.
> *BackStack 이란?*
*Application을 사용할 때 활성화 된 화면(Activity 또는 Fragment)에서 작업하다가, Back 버튼 등을 눌러 이전 화면으로 돌아갈 수 있을 것이다. 이러한 돌아가는 구조를 지원하기 위해서, 내부적으로 화면이 전환이 되더라도 기존의 화면을 없애는 것이 아니라 메모리에 저장해 놨다가, 돌아갈 때 저장된 화면을 띄워주는 구조가 필요하다. 이런 구조를 구현하기 위해 내부적으로 BackStack 이라는 구조를 사용한다. 각각의 화면이 전환될 때 마다, 그 화면을 BackStack 안에 저장한다. 그리고 Back 버튼 등으로 돌아가기를 할 경우에는 Stack에서 하나씩 꺼내 이전 화면으로 전환을 한다.* 
5. Fragment는 Activity 위에서만 존재하기 때문에 다수의 Fragment를 동시에 띄울 때 메모리가 문제 될 수 있으므로 너무 복잡한 구조는 지양해야 한다.
6. 태블릿의 경우 Activity를 사용하면 많은 공간이 낭비된다. 또한 이러한 공간 낭비를 막기위해 View를 구성하게 되지만, 이런 관리들이 너무 복잡하게 되면 사용자들의 혼란을 초래 할 수 있다.
7. 모바일의 경우 ViewPager를 이용하여 작은 화면을 사용자들이 좀 더 다이나믹하게 사용 할 수 있는 용도로 바꿀 수 있다.
8. Fragment는 태블릿과 좀 더 다이나믹한 어플리케이션 개발에 필수로 사용된다.

