# 옵셔널 체이닝(Optional Chaining)
옵셔널 체이닝은 옵셔널을 좀 더 편리하게 사용할 수 있는 문법으로, nil일지도 모르는 옵셔널에 속해 있는 프로퍼티, 메소드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정이다. 옵셔널을 반복 사용하여 옵셔널이 자전거 체인처럼 서로 꼬리를 물고있는 모야이기 때문에 옵셔널 체이닝이라고 부른다. 중첩된 옵셔널 중 하나라도 값이 존재하지 않는다면 결과적으로 nil을 반환한다.

옵셔널 체이닝은 프로퍼티나 메소드 또는 서브스크립트를 호출하고 싶은 옵셔널 변수나 상수 뒤에 물음표`?`를 붙여 표현한다. 옵셔널이 nil이 아니라면 정상적으로 호출될 것이고, nil이라면 결과값으로 nil을 반환할 것이다. 결과적으로 nil이 반환될 가능성이 있으므로 옵셔널 체이닝의 반환된 값은 항상 옵셔널이다. 

옵셔널 체이닝에 대해 알아보기 위해 기본 클래스를 설계 하겠다.
```swift
class Room {
	var number: Int		// 호실 번호

	init(number: Int) {
		self.number = number
	}
}

class Building {		// 건물
	var name: String	// 건물 이름
	var room: Room?		// 호실 정보

	init(name: String) {
		self.name = name
	}
}

struct Address {				// 주소
	var province: String		// 광역시/도 
	var city: String			// 시/군/구
	var street: String			// 도로명
	var building: Building?		// 건물
	var detailAddress: String?	// 건물 외 주소
}

class Person {					// 사람
	var name: String			// 이름
	var address: Address?		// 주소

	init(name: String) {
		self.name = name
	}
}

```

먼저, sanghee라는 Person인스턴스를 생성한다.

```swift
let sanghee: Person = Person(name: "sanghee")
```
sanghee가 사는 호실 번호를 알고 싶다. 옵셔널 체이닝과 강제 추출을 사용하여 프로퍼티에 접근해보면 다음과 같은 결과를 볼 수 있다.

```swift
let sangheeRoomViaOptionalChaining: Int? = sanghee.address?.building?.room?.number		// nil
let sangheeRoomViaOptionalUnwrapping: Int = sanghee.address!.building!.room!.number 	// 오류 발생
```
sanghee에는 아직 주소 정보와 건물 정보, 호실 정보가 없다. 그러기 때문에 sangheeRoomViaOptionalChaining 상수에는 address 프로퍼티가 `nil`이므로 옵셔널 체이닝 도중 nil이 반환된다. 그러나 sangheeRoomViaOptionalUnwrapping 상수에는 강제 추출을 시도했기 때문에 `nil`인 address 프로퍼티에 접근하려 할 때 런타임 오류가 발생한다. 

## 옵셔널 바인딩과 옵셔널 체이닝 

### 옵셔널 체이닝을 통한 값 가져오기 
옵셔널 바인딩을 사용하여 sanghee가 사는 호실 정보를 가져오는 코드를 표현하면 아래와 같다.
```swift
let sanghee: Person = Person(name: "sanghee")
var roomNumber: Int? = nil

if let sangheeAddress: Address = sanghee.address {
	if let sangheeBuilding: Building = sangheeAddress.building {
		if let sangheeRoom: Room = sangheeBuilding.room {
			roomNumber = sangheeRoom.number
		}
	}
}

if let number: Int = roomNumber {
	print(number)
} else {
	print("Can not find room number")
}
```
위의 코드를 옵셔널 체이닝으로 표현해보면 훨씬 간단해진다.
```swift
let sanghee: Person = Person(name: sanghee)

if let roomNumber: Int = sanghee.address?.building?.room?.number {	// address가 nil이기 때문에 building체크하지 않고 nil반환
	print(roomNumber)
} else {
	print("Can not find room number")
}
```

위의 코드는 옵셔널 체이닝과 옵셔널 바인딩이 결합한 형태이다. 옵셔널 체이닝의 결과값은 옵셔널 값이기 때문에 옵셔널 바인딩과 결합할 수 있는 것이다. 옵셔널 바인딩을 통해 `sanghee.address?.building?.room?.number`의 결과값이 `nil`이 아님을 확인하는 동시에 roomNumber라는 상수로 받아올 수 있다.

### 옵셔널 체이닝을 통한 값 할당
옵셔널 체이닝을 통해 값을 받아오기만 하는 것이 아니라 반대로 값을 할당해줄 수도 있다. 
```swift
sanghee.address?.building?.room?.number = 505
print(sanghee.address?.building?.room?.number)	// nil
```
위의 코드에서 nil이 반환되는 이유는 address프로퍼티가 없기 때문에 하위의 building, room 프로퍼티도 없는 것이다. 그렇기 때문에 옵셔널 체이닝은 도중에 중지된다. number 프로퍼티는 존재조차 하지 않으므로 505가 할당되지 않는다. 만약, 아래와 같이 옵셔널 체인에 존재하는 프로퍼티를 실질적으로 할당해주면, 옵셔널 체이닝을 통해 값을 정상적으로 할당할 수 있게된다. 

```swift
sanghee.address = Address(province: "경기도", city: "수원시 영통구", street: "법조로", building: nil, detailAddress: nil)
sanghee.address?.building = Building(name: "빌딩")
sanghee.address?.building?.room = Room(number: 0)
sanghee.address?.building?.room?.number = 505

print(sanghee.address?.building?.room?.number)	// Optional(505)
```

### 옵셔널 체이닝을 통한 메소드 호출
옵셔널 체이닝을 통해 메소드와 서브스크립트 호출도 가능하다. 먼저, 옵셔널 체이닝을 통한 메소드 호출 방법은 프로퍼티 호출과 동일하다. 만약 메소드의 반환 타입이 옵셔널이라면 이 또한 옵셔널 체인에서 사용 가능하다. 

### 옵셔널 체이닝을 통한 서브스크립트 호출
서브스크립트는 인덱스를 통해 값을 넣고 빼올 수 있는 기능이다. 우리가 서브스크립트를 가장 많이 사용하고 있는 곳은 Array와 Dictionary이다. 옵셔널의 서브스크립트를 사용하고자 할 때는 대괄호(`[]`)보다 앞에 물음표(`?`)를 표기해주어야 한다. 이는 서브스크립트 외에도 언제나 옵셔널 체이닝을 사용할 때의 규칙이다. 옵셔널 체이닝을 통한 서브스크립트 호출은 아래와 같이 한다.
```
let optionalArray: [Int]? = [1, 2, 3]
optionalArray?[1]	// 2

var optionalDictionary: [String: [Int]]? = [String: [Int]]()
optionalDictionary?["numberArray"] = optionalArray
optionalDictionary?["numberArray"]?[2]	// 3
```







