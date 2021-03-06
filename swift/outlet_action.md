# IBOutle & IBAction

## 아웃렛 (IBOutlet)
아웃렛은 IBOutlet 키워드를 사용하여 선언하는 인스턴스 변수들이다. 아웃렛이 하는 역할은 정말로 단순하다. 컨트롤러 헤더 파일에 선언한 객체를 인터페이스 빌더가 알아 볼 수 있도록 해준다. xib파일 안의 객체와 연결을 하고자 하는 모든 인스턴스 변수들은 IBOutlet 키워드로 다음과 같은 형식으로 선언이 되어야 한다.

```swift
@property (nonatomic, retain) IBOutlet UILabel *newLabel;
```

아웃렛으로 선언된 인스턴스 변수는 프로젝트가 빌드되면서 가장 먼저 전처리기에서 번역되고, 그것이 xib 파일과 연결된 객체라는 사실이 컴파일러에게 전달된다. 프로젝트의 실행과 관련된 컴파일에서는 아무런 영향을 미치지 않는것이다. 하지만, 이것이 없다면 인터페이스 빌더는 어떻게 객채들과 연결해야 할지 모른체 빌드를 실행하게 될 것이다. 마치 지금 당장 전화를 걸어야 하는 상황에서 어떤 전화기를 가지고 어디로 전화를 해야 할찌 모르는 상황과 비슷하다고 할 수 있다.

## 액션(IBAction)
액션은 IBOutlet과 마찬가지로 컨트롤러 헤더파일에서 하나의 메소드 형태로 선언되어 그 역할을 하게 된다. IBAction이 선언되면 이 메소드가 액션 메소드라는 것을 인터페이스 빌더에게 알려주게 되며, 컨트롤러를 통해서 호출이 가능해 진다. 액션 메소드는 헤더파일에서 다음과 같은 형식으로 선언된다.

```swift
 (IBAction)newAction:(id)sender;
```

메소드의 형식을 갖는 IBAction는 void를 리턴 타입으로 가진다. 액션 메소드는 변수값을 리턴하지 않는다는 것이다. 액션 메소드는 하나의 인자값을 갖게 되는데, 이것은 sender라는 이름의 id 타입으로 정의되고, 포인터 값이 전달된다. 이것은 똑같은 액션 메소드를 호출하는데 있어서 어떤 액션을 통해서 메소드를 호출하였는지 구분하는 구분자의 역할을 하게 된다. 만약 버튼이 하나밖에 없는 것 처럼, 액션의 구분이 필요하지 않다면 뒷부분의 '(id)sender' 를 제거하고 작성하면 가능하다. 하지만 버튼이 여러게 있는데 '(id)sender' 를 제거하면 모든 버튼이 동일한 역할만을 하게 될 것이다.


