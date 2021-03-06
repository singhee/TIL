# 14 must knows for an iOS developer
[14 must knows for an iOS developer](https://swiftsailing.net/14-must-knows-for-an-ios-developer-5ae502d7d87f)

## 1. 코드 관리
"당신의 입사를 축하한다! 이제 저장소(repository)에서 코드를 가져와서 일을 시작하자."
"잠깐, 뭐라고?"

모든 프로젝트는 심지어 당신 혼자서 개발하는 프롤젝트라도 코드 관리가 필요하다. 가장 일반적인 코드 관리 방식은 `Git`과 `SVN`이다. 

`SVN`은 중앙 집중식 버전 관리 시스템이다. 이것은 중앙식 저장소이기 때문에 작업을 위한 사본이 생성되고, 저장소에 접근을 위한 네트워크 연결이 필요하다. 변경 기록은 서버의 저장소에서만 볼 수 있다. 그리고 작업 중인 사본은 항상 가장 최신 버전으로 유지된다.

아래는 SVN과 함께 가장 많이 사용하는 툴이다. 
[versionsapp](http://versionsapp.com/)

`Git`은 분산형 버전 관리 시스템이다. 당신은 작업 가능한 로컬 저장소를 가지게 될 것이다. 그리고 이 저장소는 필요할 때만 인터넷을 연결해서 서버와 동기화시켜주면 된다. 그리고 서버와 로컬 저장소는 각자가 모두 완벽한 변경 기록을 갖고 있다. 

아래는 Git과 함께 가장 많이 사용하는 툴이다.
[sourcetreeapp](https://www.sourcetreeapp.com/)

## 2. 설계 방식(Architecture patterns)
만약 당신이 프로젝트를 시작하기 전이라면 어떤 설계(혹은 아키텍처)를 할 것인지 정해야 한다. 만약 당신이 시작한 프로젝트가 아니라면, 이미 구현되어있는 설계를 따라야 한다.

MVC, MVP, MVVM, VIPER 등 모바일 앱 개발에는 다양한 패턴이 존재한다. 이 중에서 iOS개발에 주로 사용되는 패턴을 간단하게 살펴보자.

### MVC
모델(Model), 뷰(View), 컨트롤러(Controller)의 각각 첫 글자를 딴 이름이다. 컨트롤러는 모델과 뷰 사이에서 다리 역할을 한다. 뷰와 컨트롤러는 매우 강하게 연결되어 있어서, 컨트롤러에 모든 것을 쓰게 된다. 무슨 말이냐고? 간단히 말하자면, 당신이 복잡한 뷰를 만들수록 당신의 컨트롤러가 어마어마하게 커질 거라는 이야기다. 이걸 피하는 방법이 몇 가지가 있는데, 모두 MVC의 규칙을 해치는 방법들이다. 그리고 MVC 패턴의 다른 단점 중에 하나는 테스트다. 만약 당신이 테스트를 한다면, 아마 3가지 구성 요소 중 모델만 테스트를 할 수 있을 것이다. 왜냐면 모델만이 다른 것과 분리가 되어있기 때문이다. MVC 패턴을 사용할 때 장점은 이것이 직관적이라는 것과 대부분의 iOS개발자들이 이것에 익숙하다는 것이다. 

### MVVM
모델(Model), 뷰(View), 뷰모델(ViewModel)의 앞글자를 딴 형태이다. 보통 리엑티브 프로그래밍으로 이뤄지고, 뷰와 뷰모델 사이에 연결고리(Bindings)가 있다. 이 연결고리를 통해서 뷰모델은 모델에 변화를 주고, 그리고 이건 다시 자동적으로 뷰를 업데이트하면서, 뷰모델도 변경한다. 뷰모델은 뷰와 연결고리는 가지고 있되, 뷰에 대해서 아무것도 알지 못하기 때문에 테스트를 용이하게 할 수 있고, 많은 양의 코드를 줄일 수 있다. 

더 자세한 설명이나 다른 패턴에 대한 정보가 필요하다면, 다음 글을 읽어보기를 권한다.
[iOS Architecture Patterns - Demystifying MVC, MVP, MVVM and VIPER](https://medium.com/ios-os-x-development/ios-architecture-patterns-ecba4c38de52#.mcwyo5qv3)

'어느 설계를 택하느냐'가 그렇게 중요해 보이지 않을지도 모른다. 하지만 잘 구조화되고 정리된 코드는 두통을 미연에 방지할 수 있다. 모든 개발자가 종종 저지르는 큰 실수는 시간을 아껴보겠다며 코드를 정리하지 않는 것이다. 만약 당신이 동의하지 않는다면, 벤자민 프랭클린의 이 말을 마음에 새기자

> 일 분 정리할 때마다 한 시간씩 절약할 수 있다.
> - 벤자민 프랭클린

목표는 직관적이고 읽기 쉬운 코드를 짜서, 쉽게 프로그램을 완성하고 유지 보수하는 것이 되어야 한다. 

## 3. Objective-C vs Swift
앱을 만들기 위해서 프로그래밍 언어를 결정할 때, 각 언어가 어떤 특징을 가지고 있는지 알아야 한다. 나보고 선택하라고 한다면, 난 개인적으로 스위프트를 추천한다. 왜냐고? 솔직히 말해서 Objective-C는 스위프트에 비해 나은 점이 별로 없다. 그리고 초기에 Objective-C로만 쓰여있던 거의 모든 예제와 튜토리얼이 이제는 스위프트로 업데이트가 완료 되었다.

스위프트는 많은 부분에서 우수하다. 읽기 쉽고, 사람의 언어인 영어와 닮아있고, C로 구현되지 않았기 때문에 전통적인 규칙들을 따르지도 않는다. Objective-C를 아는 사람이라면, 더 이상 문장 끝에 `;`이 필요 없고, 메서드도 괄호가 필요 없고, if문을 쓸 때도 괄호로 감싸주지 않아도 된다. 그리고 유지 보수에도 용이한데,(Objective-C의) `.h`와 `.m`파일 구분 없이 `.swift`파일 하나만 있으면 된다. 이건 Xcode와 LLVM 컴파일러가 의존성을 찾아서 자동으로 빌드를 구성해주기 때문이다. 무엇보다 쓸데없는 코드를 적을 필요가 없다. 

## 4. Reactive한 코딩, 혹은 그렇지 않은 코딩?
함수형 리엑티브 프로그래밍은 새로운 유행이다. 이 프로그래밍 방법의 목적은 비동기적인 동작들과 이벤트/데이터 흐름을 쉽게 하는 것이다. 스위프트는 Observable 인터페이스를 통해서 리액티브 프로그래밍을 구현한다. 

간단히 코드를 통해서 예시를 들어보겠다. 팀과 그의 여동생 제니가 게임 콘솔을 새로 사고 싶어 한다고 말을 해보자. 팀이 매주 부모님으로부터 5유로를 받고 있고, 제니도 마찬가지다. 그런데 제니는 매주 신문 배달을 하면서 5유로를 더 벌고 있다. 만약 그들이 같은 금액을 모은다면, 우리는 그 게임 콘솔을 살 수 있을지 매주 확인을 해볼 수 있겠다. 매번 각자의 보유 금액이 변화하면 둘의 합계가 계산된다. 그리고 만약 그 합이 콘솔을 살만큼 충분하다면 이 메세지를 `isConsoleAttainable`이라는 변수에 담아보자. 우리는 이 값을 `구독(subscribe)` 함으로 인해서 메시지를 확인할 수 있다. 

```Swift
// Savings
let timmySavings = Variable(5)
let jennySavings = Variable(10)

var isConsoleAttainable = Observable
						.combineLatest(timmy.asObservable(), jenny.asObservable()) {$0 + $1}
						.filter {$0 >= 300}
						.map {"\($0) is enough for the gaming console!"}
// Week2
timmySavings.value = 10
jennySavings.value = 20
isConsoleAttainable.subscribe(onNext: {print($0)}) // Doesn't print anything

// Week20
timmySavings.value = 100
jennySavings.value = 200
isConsoleAttainable.subscribe(onNext: {print($0)}) // 300 is enough for the gaming console!
```
이것은 함수형 리엑티브 프로그래밍으로 무엇을 할 수 있는지에 대한 아주 피상적인 부분에 불과하다. 당신이 이것과 익숙해진다면 전혀 다른 세상이 열릴 것이다. 그러면 이제 사람들이 많이 사용하는 MVC패턴을 넘어 다른 아키텍처를 활용할 수 있는 기회도 생긴다. 바로 MVVM이다!
스위프트 함수형 리엑티브 프로그래밍 세계의 가장 큰 경쟁자 두 개는 다음과 같다.

[RxSwift](https://github.com/ReactiveX/RxSwift)
[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)


## 5. 의존성 관리자(Dependency Manager)
`CocoaPods`과 `Carthage`는 iOS개발에서 가장 많이 활용되는 의존성 관리자이다. 이것들은 라이브러리를 사용하고, 최신으로 유지하는 것을 간편하게 해주는 역할을 한다.

**CocoaPods**는 루비로 구현되어 있고 다양한 라이브러리를 가지고 있다. 아래 명령어를 이용하면 쉽게 설치할 수 있다.
```Shell
$ sudo gem install cocoapods
```
설치 후에 프로젝트에 Podfile을 만들고 싶다면 아래 명령만 입력해주면 된다.
```Shell
$ pod install
```
혹은 아래와 같은 구조의  Podfile을 만들어주면 된다
```Shell
platform :ios, '8.0'
use_frameworks!

target 'MyApp' do
 pod 'SwiftyJSON','~> 2.3'
end
```
파일을 만들었다면, 이번에는 라이브러리를 설치하면 된다.
```Shell
$ pod install
```

이제 프로젝트 안에 `.xcworkspace`를 열고 라이브러리를 가져오면 된다.

**Carthage**는 CocoaPods와는 반대로 분산형 의존성 관리자다. 이것의 단점은 사용자가 라이브러리를 찾기가 힘들다는 점이다. 반면에 유지보수가 간편한 장점이 있다. 

[Git - Carthage](https://github.com/Carthage/Carthage)


## 6. 데이터 저장하기 
앱에 데이터를 저장하는 가장 간단한 방법부터 시작해보자. `NSUserDefaults`라고 불리는 이 녀석은 사용자의 기본적인 정보를 담을 수 있고, 앱이 처음 로딩될 때 불러올 수 있다. 이런 이유로 간단하고 사용이 편리한데, 몇가지 제약이 존재한다. 제약 중에 하나는 활용할 수 있는 오브젝트의 종류이다. 이건 마치 Property List(Plist)랑 비슷하다. 아래 6가지 종류만 저장할 수 있다.
- NSData
- NSDate
- NSNumber
- NSDictionary
- NSString
- NSArray

스위프트와 호환을 위해 NSNumber는 다음과 같은 것도 포함한다.
- UInt
- Int
- Float 
- Double
- Bool

오브젝트는 `NSUserDefaults`에 아래와 같은 방식으로 담을 수 있다. 첫 번째 예시는 상수를 만들고, 이 상수로 키를 만들어서 오브젝트를 저장한다.

```Swift
let keyConstant = "objectKey"
let defaults = NSUserDefaults.standardsUserDefaults()
defaults.setObject("Object to save", objectKey: keyConstant)
```
NSUserDefaults에서 오브젝트를 읽어내기 위해서는 아래와 같이 할 수 있다.
```Swift
if let name = defaults.stringForKey(keyConstant) {
	print(name)
}
```
`Keychain`은 비밀번호, 인증서, 개인 키와 개인적인 메모를 저장할 수 있는 비밀번호 관리 시스템이다. Keychain은 두 단계의 암호화로 이루어진다. 첫 번째 단계를 잠금 화면 비밀번호 입력에서 활용이 되고, 그 다음 단계는 생성된 키를 사용하고, 기기에 저장한다.

무슨 말이냐고? 물론 이것이(특히 잠금 화면 비밀번호를 쓰지 않는다면) 완벽하게 안전한 것은 아니다. 그리고 이것은 기기에 저장되기 때문에 두 번째 단계에서 이 키에 접근할 수 있는 방법도 존재한다.

가장 좋은 방법은 본인만의 암호화 방식을 사용하는 것이다.(키를 기기에 저장하지 마라.)

`CoreData`는 애플에서 디자인한 프레임워크이다. 이것을 통해서 오브젝트 친화적인 방식으로 데이터베이스와 통신할 수 있다. 이 방식을 이용하면 코드도 줄일 수 있고, 테스트도 줄어든다.

## 7. 컬렉션뷰와 테이블뷰
거의 모든 앱은 한 개 이상의 컬렉션뷰와 테이블뷰를 가지고 있다. 미래의 정신 건강을 위해서 상황에 따라 둘 중에 어느 것을 써야 하고, 그리고 각각이 어떻게 동작하는지 이해하는 것이 중요하다.

**테이블뷰(TableViews)** 는 하나의 세로 열로 아이템의 목록을 보여준다. 따라서 위아래로 스크롤할 수밖에 없다. 각 아이템은 `UITableViewCell`로 대표되고, 완벽하게 커스터마이즈 할 수 있다. 때때로 섹션과 열로 분류되기도 한다.

**컬렉션뷰(CollectionView)** 는 역시나 아이템의 목록을 보여준다. 하지만 여러 개의 열과 행으로 이뤄져 있다. (grid를 생각하면 좋다.)  콜렉션뷰는 위아래 혹은 좌우로 스크롤할 수 있고, 각 아이템은 `UICollectionViewCell`로 구성된다. `UITableViewCell`과 마찬가지로 자유롭게 변경이 가능하고, 때때로 섹션과 열로 분류되기도 한다.

둘 모두는 비슷한 기능을 하고, 재사용이 가능한 셀을 활용한다. 둘 중에 무엇을 선택할지는 당신이 만들고 싶은 목록의 복잡성에 달려있다. 컬렉션뷰는 개인적으로 어떠한 리스트를 만들 때도 모두 사용이 가능하다. 예를 들어서, 연락처 목록을 만든다고 상상해보자. 이때는 하나의 열만 있으면 되기 때문에 테이블뷰를 선택하면 된다. 몇 달이 지나지 않아, 디자이너가 그 연락처 목록을 그리드 형태로 바꾸기로 결정한다면, 당신은 테이블뷰로 구현해놓은 것을 컬렉션뷰로 고쳐야 한다. 따라서 당신의 목록이 간단하고 테이블뷰로 구현해도 충분하더라도, 앞으로 디자인이 변경될 가능성이 있다면 컬렉션뷰를 사용하는 것이 좋을 것이다.


## 8. 스토리보드 vs. Xibs vs 프로그래밍한 UI
각각의 방법은 모두 UI를 만들기 위해서 사용된다. 하지만 이걸 섞어 쓰지 말라는 법도 없다.

**스토리보드(Storyboards)** 는 프로젝트를 더 넓게 볼 수 있게 해준다. 이 방식을 통하면 앱의 플로우를 한눈에 볼 수 있기 때문에, 디자이너가 더 좋아하기도 한다. 단점은 많은 화면이 더해지면, 그 관계가 알아보기 힘들고, 로딩하는 시간이 길어진다는 것이다. 그리고 한 파일에 모든 UI가 있기 때문에 충돌 나는 부분을 잡는 시간도 많아진다. 이렇게 모여있으면 해결하기가 힘들다.

**Xibs**는 한 화면 혹은 화면의 특정 부분을 보여준다. 장점은 재사용하기 편하다는 것이다. 그리고 스토리보드를 사용하는 것보다 충돌이 발생할 가능성이 적고 화면에 어떻게 나타날지 쉽게 확인할 수 있다. 

**UI를 직접 프로그래밍 하는 방식**은 훨씬 더 높은 자유도를 준다. 그리고 충돌도 적고, 만일 충돌이 발생하더라도 해결하기도 쉽다. 단점은 개발 중에는 시각적으로 볼 수 없고, 프로그래밍하는데 시간이 오래 걸린다는 것이다.

앱에 UI를 만드는 방식은 여러 가지가 존재한다. 하지만 내 주관적인 관점으로는 이 세가지를 적절하게 섞어서 사용하는 것이 가장 좋다. 여러 개의 스토리보드와 여러 개의 Xibs를 사용하면서 필요할 때 프로그래밍까지 더 해주면 된다. 

## 9. 프로토콜
`프로토콜(Protocols)`은 일상생활 속에서 특정한 상황에 어떻게 반응해야 하는지를 의미한다. 예를 들어서, 당신이 소방관이고 응급 상황이 발생했다고 생각해보자. 모든 소방관은 성공적으로 대처하기 위해 특정한 규칙(protocol)을 따라야 한다. iOS 개발에서의 프로토콜 역시 마찬가지다.

프로토콜은 특정한 기능을 수행하기 위한 메서드와 구정 요소 등의 초안이다. 이건 각 요구 조건을 달성하기 위해 클래스, 구조체(structure), 열서형(enumeration)에 사용할 수 있다. 

아래는 프로토콜이 어떻게 만들어지고 사용되는지에 대한 예시다.

아래의 예에서 나는 불을 끌 수 있는 다양한 물질로 이뤄진 열거형이 필요하다.
```Swift
enum ExtinguisherType: String {
	case water, foam, sand
}
```
그리고 이제 위급한 상황에 대응하기 위한 방법들에 관한 프로토콜을 만들어보자.
```Swift
protocol RespondEmergencyProtocol {
	func putOutFire(with material: ExtinguisherType)
}
```
그리고 이제 프로토콜을 상속하는 소방관 클래스도 하나 만들어보자.
```Swift
class Fireman: RespondEmergencyProtocol {
	func putOutFire(with material: ExtinguisherType) {
		print("Fire was put out using \(material.rawValue).")
	}
}
```
그리고 이제 소방관을 동작시켜보자.
```Swift
var fireman: Fireman = Fireman()
fireman.putOutFire(with: .foam)
```

이 결과는 "Fire was put out using foam."일 것이다.

프로토콜은 또한 **Delegation 패턴** 에서도 자주 활용된다. 이건 클래스나 구조체가 특정한 함수를 다른 형태의 클래스나 구조체에서 부를 수 있도록 해준다.

예제를 보자!
```Swift
protocol FireStationDelegate {
	func handleEmergency()
}
```
소방서는 소방관에게 비상사태를 처리할 권한을 위임하고 있다.
```Swift
class FireStation {
	var delegate: FireStationDelegate?
	func emergencyCallReceived() {
		delegate?.handleEmergency()
	}
}
```
이 말은 소방관 클래스 역시 FireStationDelegate 프로토콜을 상속해야 한다는 것을 의미한다.
```Swift
class Fireman: RespondEmergencyProtocol, FireStationDelegate {
	func putOutFire(with material: ExtinguisherType) {
		print("Fire was put out using \(material.rawValue).")
	}

	func handleEmergency() {
		putOutFire(with: .water)
	}
}
```
이제 해줘야 하는 것은 소방관을 소방서의 대리인으로 설정하고, 응급 상황을 대신 처리하도록 하는 것이다. 
```Swift
let firestation: FireStation = FireStation()
firestation.delegate = fireman
firestation.emergencyCallReceived()
```
결과는 "Fire was put out using water."일 것이다.

## 10. 클로저
스위프트의 클로저에 대해서 이야기해보자. 클로저는 주로 completion block을 반환하거나 high order functions와 함께 사용된다. Completion blocks라는 건 이름에서도 알 수 있듯이 한 동작(task)이 끝날 때 실행되는 부분이다. 

스위프트의 클로저는 C나 Objective-C에서 blocks와 비슷하다.

스위프트에서 함수는 클로저의 특수한 케이스에 불과하다.

참고:[Swift closures and functions](http://fuckingswiftblocksyntax.com/)
이 링크는 스위프트 클로져 문법을 이해하기 좋다.

## 11. 스킴(Schemes)
간단히 말해서 스킴은 여러 설정을 바꾸는 쉬운 방법이다. 우선 배경에 대해서 잠깐 설명하도록 하자. 한 작업공간(workspace)은 다양한 관련된 프로젝트를 담을 수 있다. 한 프로젝트는 다시 다양한 대상(target)을 가질 수 있다. 한 대상이란 한 제품(product)이 어떻게 만들어지는지(build)를 결정한다. 프로젝트는 또한 여러개의 설정값을 가질 수 있다. Xcode의 스킴은 다양한 대상(targets)의 모음을 어떻게 만들지 결정하고, 어떤 설정 값이 활용될지, 어떤 테스트를 할지를 정한다. 

## 12. 유닛 테스트
당신의 코드를 테스트하고 있다면, 당신은 제대로 코딩을 하고 있는 것이다. 물론 이것이 모든 것을 해결해주지 않는다. 에러가 하나도 없거나, 어떤 문제도 생기지 않을 거라고 이야기하는 것은 아니다. 다만 테스트에 관해 내가 생각하는 장점과 단점은 다음과 같다.

우선 유닛 테스트의 단점을 생각해보자: 
* 개발 시간이 증가한다.
* 코드량이 증가한다.

장점은 다음과 같다: 
* 테스트하기 쉬운 모듈화 된 코드를 작성하게 된다.
* 서비스를 출시 후에 버그를 잡기가 쉽다.
* 유지보수가 편하다.

몇몇 도구를 쓴다면 당신의 앱이 버그와 갑자기 죽어버리는 현상 없는 앱을 만드는데 도움이 될 것이다. 여러 도구들이 있는데, 당신의 필요에 따라서 한 개 혹은 그 이상을 골라서 쓰면 된다. 자주 사용되는 것은 아마도 Leak Checks, Profile Timer, 그리고 Memory Allocation일 것이다. 

## 13. 위치정보
많은 앱들이 사용자 위치 정보가 필요한 기능이 필요할 것이다. 따라서 이것이 위치 정보가 iOS에서 어떤 식으로 작동하는지 알아두면 좋다.

`Core Location`이라고 불리는 프레임워크가 당신이 필요한 모든 정보에 접근할 수 있도록 도움을 준다. 

`Core Location`프레임워크는 당신이 기기를 통해 현재 위치를 판단하거나 어디로 향하고 있는지 알려준다. 이 프레임워크는 하드웨어를 통해 이것을 판별한다. iOS에서는 블루투스 비콘을 통해서 현재 위치를 확인할 수도 있다. 

아래 애플 가이드와 샘플코드를 참고하도록 하자.
[Apple Developer - About Location Services and Maps](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009497)

## 14. 다개국어 지원
모든 앱이 반드시 구현해야 하는 기능이 있다. 이건 바로 지역에 따라서 언어를 바꿔주는 일이다. 만약 당신의 앱이 하나의 언어로만 시작을 하더라도, 가까운 미래에 다른 언어를 추가하는 일이 따라올 것이다. 만약 모든 문장을 다개국어가 지원 가능한 형태로 세팅을 해놓는다면, `Localizable.strings`파일에 번역된 내용을 추가하기만 하면 된다. 

파일 창(file inspector)을 통해 하나의 언어를 추가할 수 있다. `NSLocalizedString`을 통해서 텍스트를 하나 불러오기 위해서, 당신이 해야할 것은 아래와 같다.
```Swift
NSLocalizedString(key:, comment:)
```
안타깝게도 다른 언어 하나를 이 파일에 추가하는 것은 수동으로 해야 한다. 아래는 어떤 식으로 작성하는지에 대한 예시다.
```JSON
{
	"APP_NAME" = "MyApp"
	"LOGIN_LBL" = "Login"
	...
}
```
심지어 복수형을 처리하기 위한 방법도 존재한다.

























