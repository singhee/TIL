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








