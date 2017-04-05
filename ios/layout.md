# layout 변경

## 레이아웃 변경이 발생하는 시점
UIView의 크기가 변경되면, 크기가 변경된 UIView의 서브뷰들은 위치와 크기가 조정되어야 한다. 이 때, UIView는 자동과 수동으로 View들의 layout을 조정하는 방법을 제공한다. 레이아웃의 변경은 다음의 이벤트에 발생한다.

```
1. UIView의 bounds 사이즈의 변경
2. Root View 의 변화를 유발하는 Interface orientation (세로모드, 가로모드)변화
3. UIView의 view layer 변화 유발 또는 layout을 요청하는 Core Animation sublayers 의 설정
4. UIView의 setNeedsLayout 또는 layoutIfNeeded 메소드가 호출될 경우
5. UIView의 layer에서 setNeedsLayout 이 호출되는 경우. 
```

그렇다면, 레이아웃 변경과 관련 되어서 필요한 몇 가지 기능에 대해 알아보도록 하자. 

### setNeedsLayout
이 메소드는 'needLayout' 에 대한 flag 를 `True`로 셋팅하는 역할을 한다. `TRUE`로 셋팅하는 것은 해당 뷰가 새로 layout이 필요함을 알려주는 역할을 한다. 그렇기 때문에 해당 뷰의 subview를 업데이트 하고 싶을 때 main thread에서 호출 해주면 된다. 그러면 해당 뷰가 `Redraw` 되는 시점에서 `layoutSubViews`가 호출되게 된다. 대부분의 경우 `setNeedsLayout`을 직접 호출할 필요는 없다. 왜냐하면 디폴트 값이 이미 `True`로 설정되어 있기 때문에, 뷰의 프레임이 변할 때마다 자동으로 subView들도 `layoutSubviews`가 되어 Redraw 된다. 

### layoutIfNeeded
`needLayout` 의 flag가 `True`일 때, 뷰가 Redraw 되는 시점까지 기다리지 않고 
바로 Redraw 시키고 싶을 때 호출한다.

### drawRect
뷰에 무언가를 그릴 때는 drawRect: 함수를 이용하게 된다. 뷰의 내용, 혹은 뷰의 하위 뷰의 어떤 그래픽적인 변경이 있어서 뷰를 업데이트 해야 할 필요가 있을 때 이 메소드가 호출된다. View 에 어떤 내용을 그리는 것은 성능 상 이슈로 반드시 drawRect: 에서만 해야한다.s