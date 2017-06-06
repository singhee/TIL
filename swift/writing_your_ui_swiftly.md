# UI를 보다 Swift스럽게 만들어 볼까요?
[Realm - Writing your UI swiftly](https://news.realm.io/kr/news/sommer-panage-writing-your-ui-swiftly/)
이 강연은 Swift의 언어적인 구조와 속성을 이용해서 애플리케이션과 UI 코드를 더 간단하게 작성하는 방법을 알려 드리는 강연이다. UI 레이어를 작성하는 동안 흔히 저지르는 실수를 살펴보고 개선할 수 있는 방법을 살펴 보도록 하자. 특히 열거형과 유용한 서드 파티 Swift 라이브러리, 프로토콜을 통한 뷰 통합 등으로 모델링 뷰 상태를 파악하는 방법도 알려준다.

## 소개
저는 Sommer Panage이며, 샌프란시스코의 작은 스타트업인 Chorus Fitness의 리드 엔지니어입니다. Chorus Fitness에서 프로덕션 앱을 작업하면서 스스로의 앱 아키텍처와 패턴을 고안했습니다. 그 과정에서 동일한 코드를 작성한 곳에서 발생하는 패턴을 발견했습니다.

이 강연에서는 그 패턴에 대한 네 가지 이야기를 말씀드리고 Swift 언어 자체의 기능을 사용해서 코드를 개선할 수 있는 방법을 공유하겠습니다. 아래처럼 스타워즈 API로 영화 초반에 올라가는 스토리를 보여주는 예제 앱을 통해 설명하겠습니다.

## 슈뢰딩거의 결과
첫 번째 이야기는 슈뢰딩거의 결과라고 부르겠습니다.

###백엔드에서 데이터 가져오기:
```Swift
func getFilms(completion: @escaping ([Film]?, APIError? -> Void) {
	let url = SWAPI.baseURL.appendingPathComponent(Endpoint.films.rawValue)
	let task = self.session.dataTask(with: url) { (data, response, error) in
		if let data = data {
			do {
				let jsonObject = try JSONSerialization.jsonObject(with: data, options: [])
				if let films = SWAPI.decodeFilms(jsonObject: jsonObject) {
				completion(films, nil)
				} else {
				completion(nil, .decoding)
				}
			} catch {
				completion(nil, .server(originalError: error))
			}
		} else {
			completion(nil, .server(originalError: error))
		}
	}
	task.resume()
}
```

여기서 데이터가 반환되면 완료 블럭에서 다시 전송합니다. 만약 데이터가 부적절한 경우 데이터에는 nil을 넣고 에러와 함께 완료 블럭을 호출합니다. 이 경우 서버에서 응답을 받거나 에러를 받게 되겠죠. 하지만 아래 UI 코드를 보면 해당하는 경우가 없습니다. 결과는 네 가지가 가능하며, 이 중 둘은 말이 안됩니다.

```Swift
override func viewDidLoad() {
	super.viewDidLoad()

	apiClient.getFilms() { films, error in
		if let films = films{
			//Show film UI
			if let error = error {
				//Log warning ... this is weird
			}
		} else if let error = error {
			// Show error UI
		} else {
			// No results at all? Show error UI I guess?
		}
	}
}
```
해결책은 성공/결과 객체 혹은 실패/에러 객체처럼 서버와의 상호 작용을 다르게 모델링하는 것입니다.

Rob Rix의 [Result](https://github.com/antitypical/Result)라는 프레임워크를 사용해서 이 시나리오에 대한 솔루션을 구현하는 방법을 설명하겠습니다. 간단하면서도 우리의 의도를 정확하게 포착할 수 있습니다.

```Swift
public enum Result<T, Error: Swift.Error>: ResultProtocol {
	case success(T)
	case failure(Error)
}
```

###열거형 및 관련 값 참고 사항
열거형에는 성공이나 실패 두 가지 경우가 가능합니다. 성공할 경우 옵셔널이 아닌 타입 T라는 결과 객체를 갖게 되죠. 데이터가 무엇이든 마찬가지입니다. 실패에는 옵셔널이 아닌 에러 객체를 갖는데, 두 가지 경우만 다루면 됩니다.
```Swift
func getFilms(completion: @escaping ([Film]?, APIError? -> Void) {
	let task = self.session
		.dataTask(with: SWAPI.baseURL.appendingPathComponent(Endpoint.films.rawValue)) { (data, response, error) in
			let result = Result(data, failWith: APIError.server(originalError: error!))
				flatMap { data in
					Result<Any, AnyError>(attempt: { try JSONSerialization.jsonObject(with: data, options: []) })
						.mapError { _ in APIError.decoding }
				}
				.flatMap { Result(SWAPI.decodeFilms(jsonObject: $0), failWith: .decoding) }

			completion(result)
		}
	}
	task.resume()
}
```
UI에 반영하면 다음과 같습니다.

```Swift 
override func viewDidLoad() {
	super.viewDidLoad()

	apiClient.getFilms() { result in
		switch result {
		case .success(let films): print(films) // Show my UI!
		case .failure(let error): print(error) // Show some error UI!
		}
	}
}
```
결과 열거형을 사용해서 서버 상호작용의 성공과 실패를 더욱 정확하게 모델링할 수 있으므로 뷰 컨트롤러 코드를 간결하게 만들 수 있습니다.

## 작은 레이아웃 엔진
###스토리보드
일반적으로 저는 프로덕션 앱에서 스토리보드를 사용하지 않습니다. 그 이유는 먼저, 팀에서 스토리보드로 작업하는 것이 경험상 훨씬 어려웠기 때문입니다. XML diff를 봐도 변경 사항이 명확하지 않을 때가 많으며, 병합할 때 충돌이 나는 경우 해결하는 것이 너무나 고통스럽습니다.

또한, UI로 작업할 때 종종 같은 색상과 글꼴 및 여백을 반복적으로 설정해야 합니다. 이런 값들은 모두 상수로 결정되는 것이 좋은데 스토리보드는 이를 지원하지 않죠.

인터페이스 빌더 파일과 코드에 있는 연결은 컴파일 시간에 강제되지 않습니다. 그 결과 버튼과 탭 버튼 메서드 사이를 연결하고 메서드 이름을 변경하면 프로젝트가 빌드되긴 하지만 런타임 때 충돌이 발생해버립니다.

###코드로 만드는 오토 레이아웃
스토리보드를 사용하지 않는 경우 저는 코드로 오토 레이아웃을 만들고 있습니다. 예제 앱의 메인 화면은 테이블뷰로, 이 테이블뷰는 부모와 같은 크기입니다.

iOS 9의 레이아웃 앵커를 사용해서 이 레이아웃을 설정했습니다.

레이아웃 코드를 더욱 읽기 좋고 작성하기 쉽도록 하기 위해 Robb Bohnke의 다른 프레임워크인 Cartography를 사용했습니다.

Cartography를 사용하면 오토 레이아웃의 제약 조건을 선언식으로 쉽고 깔끔하게 설정할 수 있습니다.
```Swift
init() {
	super.init(frame: .zero)
	addSubview(tableView)

	// Autolayout: Table same size as parent
	constrain(tableView, self) { table, parent in
		table.edges == parent.edges
	}
}
```
다음 코드에서 좀더 복잡하게 Cartography를 사용하는 방법을 볼 수 있습니다. 테이블 뷰의 셀에서 사용하는 코드인데 본질적으로 오토 레이아웃을 선형 방정식 집합으로 표현할 수 있어서 이해하기 쉽습니다.
```Swift
private let margin: CGFloat = 16
private let episodeLeftPadding: CGFloat = 8

override init(style: UITableViewCellStyle, reuseIdentifier: String?) {
	super.init(style: style, reuseIdentifier: reuseIdentifier)

	contentView.addSubview(episodeLabel)
	contentView.addSubview(titleLabel)

	constrain(episodeLabel, titleLabel, contentView) { episode, title, parent in
		episode.leading == parent.leading + margin
		episode.top == parent.top + margin
		episode.bottom == parent.bottom - margin

		title.leading == episode.trailing + episodeLeftPadding
		title.trailing <= parent.trailing - margin
		title.centerY == episode.centerY
	}
}
```
에피소드 번호는 컨텐트 뷰 안에 약간의 여백을 두고 위치하며, 제목에도 약간의 여백이 있고 에피소드에 중앙 정렬됩니다.

Cartography 프레임워크를 사용하면 Swift 오퍼레이터 오버로드의 훌륭한 기능을 활용할 수 있고 이를 통해 코드로 오토 레이아웃을 아주 쉽게 작성할 수 있습니다


## 뷰 상태
뷰를 사용하면 보통 세 가지 상태에서 데이터를 가지고 있는 것을 볼 수 있습니다.

* 데이터가 로딩 중일 때
* 데이터가 성공적으로 로드됐을 때
* 에러가 있을 때 ()해당 에러를 보여주는 UI 상태 표시)
아래 코드에서 어떻게 서로 다른 뷰 상태를 처리할 수 있는지 보여드리겠습니다.
```Swift 
/// MainView.swift

var isLoading: Bool = false {
	didSet {
		errorView.isHidden = true
		loadingView.isHidden = !isLoading
	}
}

var isError: Bool = false {
	didSet {
		errorView.isHidden = !isError
		loadingView.isHidden = true
	}
}

var items: [MovieItem]? {
	didSet {
		tableView.reloadData()
	}
}
```
뷰 상태를 나타낼 때 일반적으로 `isLoading`나 `isError`를 나타내는 플래그를 사용합니다. 하지만 그다지 훌륭한 방법은 아닙니다.

만약 `isError`와 `isLoading`이 실수로 true로 설정된 경우 실제 상태가 어떤지 알 수 없으므로 해당 플래그로는 발생 가능한 상태를 전부 설명할 수 없습니다. 뷰는 세 개의 상태를 가지고 있고 이 중 둘은 정보와 관련있습니다.

해결책은 연관된 값을 갖는 열거형을 사용하는 것입니다.
```Swift
final class MainView: UIView {
	enum State {
		case loading
		case loaded(items: [MovieItem])
		case error(message: String)
	}

	init(state: State) { ... }
	// the rest of my class...
}
```
우리가 원하는 정확한 상태로 뷰를 초기화할 수 있게 된 것이 보이시나요? 초기화 단계부터 이후까지 뷰는 항상 하나의 상태에 있게 됩니다.

모든 뷰 관리는 바로 여기에서 시작됩니다. `getFilms`를 호출하기 전에 `ViewState`를 loading으로 설정하고, 결과에 따라 loaded나 error로 설정하면 됩니다. 뷰 로직을 한 장소에 집중할 수 있죠.



## 반복 코드
스타워즈 인트로의 유명한 노란 글자를 보여주는 두 번째 뷰 컨트롤러 역시 앞서 말한 세 가지 뷰 상태를 똑같이 갖습니다. 마지막으로 이같은 반복 코드에 대해 말씀드리겠습니다.

서로 관련없는 객체가 공유하는 일련의 동작이 있는 경우죠. 이 경우 관련없는 객체는 메인 뷰 컨트롤러와 크롤링 뷰 컨트롤러입니다. 이런 상황을 프토로콜로 단순화할 수 있습니다.

프로토콜은 특정 작업이나 기능 조각에 적합한 메서드, 속성과 다른 요구사항들의 청사진을 정의합니다. 프토토콜은 클래스, 구조체, 열거형에 모두 적용할 수 있고 이들 요구사항을 실제 구현하도록 할 수 있습니다.

이 경우 세 가지 뷰 상태와 관련된 동작을 나타내면 되겠죠?

구체적으로는 뷰에 데이터 로딩, 데이터 로딩 완료, 혹은 실패를 처리하려고 합니다. 이를 위해서는 ViewState 열거형이 필요합니다. 로딩 뷰와 에러 뷰가 필요하며 상태가 변하면 호출할 업데이트 함수도 필요할 겁니다.

```Swift
protocol DataLoading {
	associatedtype DataLoading
	
	var state: ViewState<Data> { get set }
	var loadingView: loadingView { get }
	var errorView: ErrorView { get }
	
	func update()
}

enum ViewState<Content> {
	case loading
	case loaded(data: Content)
	case error(message: String)
}

```
이런 것들을 프로토콜에 넣습니다. 이는 로딩 패턴을 정의하는 동작 집합이며 ViewState 열거형을 제너릭으로 만들어서 필요한 것을 로드하도록 할 수 있습니다.
```Swift
// default protocol implementation
extension DataLoading where Self: UIView {
	func update() {
		switch state {
		case .loading:
			loadingView.isHidden = false
			errorView.isHidden = true
		case .error(let error):
			loadingView.isHidden = true
			errorView.isHidden = false
			Log.error(error)
		case .loaded:
				loadingView.isHidden = true
				errorView.isHidden = true
		}
	}
}
```
관련없는 객체가 공유하는 기능을 프로토콜로 분해해서 코드의 중복을 피하고 모든 로직을 한 곳에 통합할 수 있습니다.
```Swift
// DataLoading in Main View
final class MainView: UIView, DataLoading {
	let loadingView = LoadingView()
	let errorView = ErrorView()
	
	var state: ViewState<[MovieItem]> {
		didSet {
			update()
			tableView.reloadData()
		}
	}
}

// DataLoading in Crawl View
class CrawlView: UIView, DataLoading {
	let loadingView = LoadingView()
	let errorView = ErrorView()
	
	var state: ViewState<String> {
		didSet {
			update()
			crawlLabel.text = state.data
		}
	}
}
```



