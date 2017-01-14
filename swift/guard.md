# guard
`guard`는 **빠른종료(Early Exit)**의 핵심 키워드로, ++`if`구문과 유사하게 `Bool`타입의 값으로 동작하는 기능이다.++ guard 키워드 뒤에 따라오는 코드의 실행 결과가 `true`일 경우 코드가 계속 실행된다. if 구문과는 다르게 guard 구문은 항상 `else`구문이 뒤에 와야 한다. 만약 guard 뒤에 따라오는 코드의 실행 결과값이 `false`라면 else 블록 내부 코드를 실행하게 되는데, 이때 else 구문의 블록 내부에는 꼭 자신보다 상위의 코드 블록을 종료하는 코드가 들어가게 된다. 그래서 특정 조건에 부합하지 않다는 판단이 되면 재빠르게 코드 블록의 실행을 종료할 수 있다. 이렇게 현재의 코드 블록을 종료할 시에는 `return`, `break`, `continue`, `throw` 등의 제어문 전환 명령을 사용한다. 또는 `fatalError()`와 같은 [비반환 함수](https://github.com/singhee/TIL/blob/master/swift/function.md#종료되지-않는-함수)나 메소드를 호출할 수도 있다. 
```swift
guard Bool 타입 값 else {
	예외사항 실행문
	제어문 전환 명령어
}
```

guard 구문을 사용하면 if 코드를 훨씬 간결하고 읽기 좋게 구성할 수 있다. if 구문을 쓰면 예외사항을 else 블록으로 처리해야 하지만 예외사항만을 처리하고 싶다면 guard 구문을 쓰는 것이 훨씬 간편하다. 아래는 if 구문과 guard 구문의 비교를 설명하는 구문이다.
```swift
// if 구문을 사용한 코드
for i in...3 {
	if i == 2 {
		print(i)
	} else {
		continue
	}
}


// guard 구문을 사용한 코드
for i in 0...3 {
	guard i == 2 else {
		continue
	}		
	print(i)
}
```

Bool 타입의 값으로 guard 구문을 동작할 수 있지만 옵셔널 바인딩의 역할도 수행할 수 있다. guard 뒤에 따라오는 옵셔널 바인딩 표현에서 옵셔널의 값이 있는 상태라면 guard 구문에서 옵셔널 바인딩된 상수를 guard 구문이 실행된 아래 코드부터 함수 내부의 지역상수처럼 사용할 수 있다. 아래는 옵셔널 바인딩을 활용한 guard 구문이다. 
```swift
func greet(_ person: [String: String]) {
	guard let name: String = person["name"] else {
		return 
	}

	print("Hello \(name)!")

	guard let location: String = person["location"] else {
		print("I hope the weather is nice near you")
		return
	}

	print("I hope the weather is nice in \(location)")
}

var personInfo: [String: String] = [String: String]()
personInfo["name"] = "sanghee"

greet(personInfo) 	// Hello sanghee! 
				  	// I hope the weather is nice near you

personInfo["location"] = "Korea"

greet(personInfo) 	// Hello sanghee!
					// I hope the weather is nice in Korea
```

