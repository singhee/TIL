# iOS -  Auto Layout
아이폰은 크기가 다양하게 진화됨에 따라 앱도 다양한 크기에서 제대로 볼 수 있도록 디자인 해야한다.  그래서 Xcode는 다양한 화면 크기에 자동으로 레이아웃을 조절해주는 기능이 추가되었다. 그것이 바로 `AutoLayout` 이다. 또한 화면크기가 변경되는 것만이 아닌, 화면을 체크하고 화면방향에 따라 화면 가로세로 비율에 따라 변경되는 경우도 지원해준다. 

iOS 앱 화면 디자인시 Drag&Drop 으로 배치한 요소들은 논리적 픽셀의 절대좌표로 표시되고 있기 때문에 다른 크기의 기기에서 보면 벌어지거나 퍼지게 된다.  

`AutoLayout`으로 다른 화면크기로 표시할 때 `Constraint(제약)` 이라는 위치와 크기에 대한 표시규칙을 기반으로 자동으로 레이아웃을 변경하여 화면에 잘 나오도록 한다.  즉, 화면 크기가 변경될 때 **무엇을 기준으로 배치할 것인가를 결정하는 것**이 `Constraint`이다. 

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