# iOS -  Auto Layout
아이폰은 크기가 다양하게 진화됨에 따라 앱도 다양한 크기에서 제대로 볼 수 있도록 디자인 해야한다. iOS 에서 UI레이아웃을 설정하는 방법에는 크게 세 가지 방법이 있다. 세 가지 방법에 대해 알아보고 장단점에 대해 파악해 보면 다음과 같다.  

|    |하드코딩|Autoresizing Mask|Auto Layout|
|-----|------|------------------|-------------
|장점 | 프로그래머의 자율성이 높음. 원하는대로 설정이 가능| 슈퍼뷰 프레임의 변화에 따라 뷰의 프레임을 정의해서 외부 변화에 적용 가능| 프레임 대신 constraint를 통한 관계중심으로 파악해서 내부 외부 변화에 동적으로 적용|
|단점 | 모든 변화를 직접 파악해서 처리해야 함| 전체 레이아웃 중에서 지원하는 범위가 작음 | 익숙해지기 어렵고 디버깅이 어려움|

위에서 볼 수 있듯이 Xcode는 다양한 화면 크기에 자동으로 레이아웃을 조절해주는 기능인 `AutoLayout` 을 지원해주기 시작했다. 또한 화면크기가 변경되는 것만이 아닌, 화면을 체크하고 화면방향에 따라 화면 가로세로 비율에 따라 변경되는 경우도 지원해준다.  

`AutoLayout`으로 다른 화면크기로 표시할 때 `Constraint(제약)` 이라는 위치와 크기에 대한 표시규칙을 기반으로 자동으로 레이아웃을 변경하여 화면에 잘 나오도록 한다.  즉, 화면 크기가 변경될 때 **무엇을 기준으로 배치할 것인가를 결정하는 것**이 `Constraint`이다. 

결국 간단히 말해서, `AutoLayout`은 View hierarchy 안 모든 view의 크기와 위치를 constraint를 기반으로 동적으로 계산하는 것이다.

## AutoLayout 기본제약 - Constraint
AutoLayout의 가장 기본은 `Constraint`이다. 이는 다른 화면크기로 표시될 때 요소(Label, Button, ImageView, View..) 를 무엇을 기준으로 배치할 것인가를 지정하는 규칙이다. 

**무엇을 기준으로 배치할 것인가**에 대해서는 `Align(정렬)`과 `Pin(고정)` 으로 2가지가 있다.  요소들을 화면 정가운데에 맞추고 싶거나 다른 요소와 모아서 정렬하고자 할 때 `Align`을 사용한다. 그리고 화면 상하 좌우 가장자리에 고정 표시할 때와 크기를 고정하고 싶다면 `Pin`을 사용한다.  모든 레이아웃은 기본적으로 이 두가지를 사용한다. 


### Constraint 지정 방법
AutoLayout Constraint를 설정하는 방법은 기본적으로 편집기 영역 오른쪽 아래 줄지어 있는 버튼을 통해 할 수 있다. 3번째 버튼이 Align, 4번째 버튼이 Pin 버튼이다. 마지막 버튼은 Resolve Auto Layout Issue를 해결해주는 버튼이다.

#### Pin
Pin 버튼을 사용하면 요소를 핀으로 고정하도록 위치 및 크기를 지정할 수 있다. 화면 크기가 변해도 고정된 위치나 크기는 변하지 않는다.

Pin의 항목을 보면 다음과 같다.
* “Spacing to nearest neighbor” 
: 요소의 상하좌우를 고정하는 여부를 지정하는 제약조건으로 빨간 점선을 클릭하면 실선으로 변경되면서 고정된다. 값을 입력해도 점선이 실선으로 변경되고 고정된다. 값 입력란에 ▼가 표시되지만 이를 클릭하면 어디에 고정할지를 선택할 수 있다. 예로 위쪽을 고정하고 싶을 때 상태표시줄에 고정하거나 화면상단에 고정하거나를 선택할 수 있다. 또한 기본적으로 화면 좌우로 조금 여유가 있는데 이 여백을 사용하지 않을 경우 [Constrain to margins]를 체크하지 않는다.
* Width: 요소 너비를 지정하여 고정
* Height: 요소 높이를 지정하여 고정
* Equal Widths: 여러 요소를 같은 너비로 처리시
* Equal Heights: 여러 요소를 같은 높이로 처리시
* Aspect Ratio: 요소의 종횡비를 유지

#### Align
Align 버튼을 사용하여 선택되어 있는 여러 요소들의 위치의 정렬이 가능하다. 예를들어 여러개의 Button의 왼쪽 정렬 및 Label을 화면 가운데에 오도록 하는데 사용할 수 있다. 요소를 하나만 선택한 경우에는 수평, 수직 방향으로 맞추는 것만 가능하다. 

Align의 항목을 보면 다음과 같다
* Leading Edges: 여러 요소의 왼쪽에 맞춤
* Trailing Edges: 여러 요소의 오른쪽 가장자리에 맞춤
* Top Edges: 여러 요소 상단에 맞춤
* Bottoms Edges: 여러 요소의 하단에 맞춤
* Horizontal Centers: 여러 요소의 수평방향  중심위치에 맞춤
* Vertical Centers: 여러 요소의 수직 방향의 중심위치에 맞품
* Baselines: 여러 레티블등의 텍스트 아래를 맞춤
* Horizontal Center in Contrainer: 표시 영역의 수평방향 중심에 맞춤[
* Vertical Center is Container: 표시 영역의 수직방향 중심에 맞춤

#### Resolve Auto Layout Issue
이 기능은 Constraints 및 배치가 문제가 있을 때 조절해주는 버튼이다. 선택한 요소에 대해서만 사용하는 Selected Views와 화면의 모든 요소에 대해서 실행하는 All Views in View Controller 라는 두가지 항목이 있고 어느 쪽에도 같은 기능이 나열되어 있다.

* Update Frames
Constraints 및 Interface Builder에서 위치가 어긋나 있는 경우 제약에 따라 Interface Builder에서 위치를 이동한다.
* Update Constraints
Constraints 및 Interface Builder에서 위치가 어긋나 있을 때 Interface Builder에서의 위치에 따라 제약을 변경한다.
* Add Missing Constraints
Constraints가 부족한 경우 부족한 제약을 자동으로 추가한다.
* Reset to Suggested Constrains
설정되어 있는 Constraints를 삭제하고 알맞다고 판단되는 제약을 자동 추가한다.
* Clear Constraints
지정된 제약조건을 삭제한다.

### 드래그로 Constraint 지정하기
Align과 Pin 버튼을 사용하지 않고 드래그만으로 지정하는 방법도 있다. 요소를 Control + 클릭 또는 마우스 오른쪽을 클릭하여 드래그하는 방법이다.  드래그한 방향과 분리 위치에 의해 알맞은 Constraints 목록이 나오며 그 중에서 선택해 설정한다.

#### 위치를 지정하는 경우
위치를 지정 하려면 요소를 위에서 제시한 방법으로 클릭하여 상하좌우 요소 외부의 어느 한쪽으로 끌어 놓는다. 

#### 너비나 크기를 고정하는 경우
너비나 크기를 고정하려면 요소를 클릭하여 그 요소 중 좌우(width) 또는 상하(height)로 끌어 놓는다.

### Constraint 없이 사용하기 : Stack View
`Stack View`는 constraint없이 레이아웃을 배치 가능하게 한다. 
[Figure_1. Stack View](../images/ios_stackview.png)
* axis: 스택 뷰의 가로 혹은 세로 정렬 방향 결정 (UIStackView)
* orientation: 스택 뷰의 가로 혹은 세로 정렬 방향 결정 (NSStackView)
* distribution: 축에 맞춘 내부 뷰의 레이아웃 결정
* alignment: 축과 수직 방향의 정렬 결정
* spacing: 이웃 뷰와의 거리 결정

### Auto Layout Guideline
* 뷰의 프레임, 바운드나 센터 속성으로 뷰의 위치를 정하지 말라. 
* constraint 로직을 줄여주는 stack view를 적극 활용하라. 
* 뷰와 가장 가까운 이웃과 constraint를 설정하라. 
* 이웃한 두 개의 버튼이 있다면 첫 버튼의 trailing edge로부터 두번째 버튼 leading edge의 contraint를 설정하라. 첫 버튼을 건너 뛰어서 뷰의 가장자리로부터 contraint를 만들지 마라. 
* 가급적 뷰의 높이나 너비를 고정하는 것을 피하라. 
* Auto Layout을 사용하는 이유는 변화에 동적으로 반응하기 위함이다. 고정 크기를 사용하면 뷰의 반응성을 낮춘다. 필요시에는 뷰의 최소 크기나 최대 크기를 설정하라. 
* contraint 설정이 어렵다면 Pin과 Align 도구를 사용하라. control+드래그보다 느리 지만 정확한 값과 관련 아이템을 설정하고 확인할 수 있다. 
* 아이템의 프레임이 자동으로 갱신되는 경우를 주의하라. 아이템의 크기와 위치가 충 족되는 constraint가 없다면 갱신이 되지 않을 수 있다. 화면 밖에 위치한 뷰의 높이나 너비가 0이 돼서 사라지는 경우도 발생할 수 있다. 
* 자유롭게 아이템 프레임을 갱신할 수 있으며 갱신 내용을 취소할 수도 있다. 
* 레이아웃 내의 모든 뷰가 의미 있는 이름을 갖도록 하라. 도구 사용시 뷰를 식별하는데 도움이 된다. 
* 시스템은 자동으로 텍스트나 타이틀을 통해 라벨과 버튼의 이름을 만든다. 다른 뷰의 경우 Identity inspector에서 설정하거나 document outline 상의 뷰를 더블 클릭 해서 이름을 만들 수 있다. 
* right과 left 대신 leading과 trailing constraint를 사용하라. 
* iOS의 semanticContentAttribute 프로퍼티나 OSX의 userInterfaceLayoutDirection 프로퍼티를 통해 뷰가 leading과 trailing edge를 계산하는 방법을 조정할 수 있다.
* iOS에서 뷰 컨트롤러의 루트 뷰 가장자리로부터 constraint를 만들 때 아래 contraint를 사용하라. 
	* Horizontal constraints: layout margin으로부터 0 포인트 constraint를 사용하라. 시스템은 디바이스와, 앱이 뷰 컨트롤러를 보여 주는 방식을 기반해서 자동으로 거리를 맞춘다.  margin을 포함해 루트 뷰를 채우는 텍스트 객체의 경우 layout margin 대신 가독성 있는 컨텐트 가이드를 사용하라.  배경 이미지처럼 루트 뷰를 모두 채우는 아이템의 경우 뷰의 leading과 trailing edge를 사용하라. 

	* Vertical constraints: 뷰가 bar 아래를 확장하는 경우 top과 bottom margin을 사용하라. 이는 스크롤 뷰에 흔히 사용되는 패턴이나, 컨텐츠 의 초기 위치를 맞추려면 스크롤 뷰의 contentInset을 수정하고 scrollIndicatorInsets 프로퍼티를 수정해야 할 수 있다.  뷰가 bar 아래를 확장하지 않으면 레이아웃 가이드의 top과 bottom으 로부터 constraint를 만들라. 
	
* 코드에서 뷰를 생성할 경우, translatesAutoresizingMaskIntoConstraints 프로퍼티를 NO로 설정하라. 그렇지 않으면 기본 설정으로 뷰의 프레임과 autoresizing mask를 기반으로 시스템이 자동 contraint들을 만든다. 커스 텀 constraint를 만들 경우 충돌이 발생하므로 미완성 레이아웃이 된다. 
* OSX와 iOS가 레이아웃을 계산하는 방식이 다름에 유의하라. 
* OSX에서는 Auto Layout으로 윈도우 컨텐츠와 윈도우 크기를 모두 수정할 수 있다. 
* iOS에서는 시스템이 씬의 크기와 레이아웃을 결정하므로, Auto Layout으로 는 씬의 컨텐츠만을 수정할 수 있다. 
* 이런 차이가 우선 순위 결정에 따라 레이아웃 디자인에 큰 영향을 미칠 수 있다.





