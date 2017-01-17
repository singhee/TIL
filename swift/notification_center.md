# NotificationCenter
앱 내부에서 시그널 패스가 연결되지 않은 곳에 메세지를 주고 받을 수 있게 해주는 것이 `NotificationCenter`의 역할이다. 그리고 여기서 주고받게 되는 메시지 내용을 감싸는 껍데기가 `NSNotification` 오브젝트이다.

## NSNotificationCenter
`NSNotificationCenter`는 이름에서 알 수 있듯이 Notification Center로, NSNotification 을 중계해 주는 역할을 한다. 일반적으로 오브젝트를 생성해서 사용하지 않고 싱글턴 인스턴스를 받아서 사용한다.s

```swift
// Swift Example: NSNotificationCenter Singleton Pattern
let notificationCenter = NSNotificationCenter.defaultCenter()
```
굳이 변수에 오브젝트 포인터를 저장해 둘 필요는 없다. 싱글톤 인스턴스이기 때문에 필요 할 때 바로 싱글턴 팩토리(defaultCenter)를 불러서 쓰면 된다. 

## Observer 등록
`Observer`란 중계되는 NSNotification 중 원하는 것을 골라서 받을 수 있게 해 주는 기능을 한다. 옵저버는 인자로 `NSNotification`을 받는 형태로 작성할 수 있다. 

```swift
// Swift Example: Notification Handler Method
func didReceiveSimpleNotification(notification: NSNotification) {
	let message: String? = notification.userInfo("message") as? String
	println("I've got the message \(message)")
}
```
위 코드는 NSNotification을 통해 문자열(message)을 전달받아 로그를 찍는 코드이다. 여기서 userInfo라는 `Dictionary`형 오브젝트를 이용할 수 있다는 점을 알아두자.

아래의 코드는 "simple-notification"이라는 이름의 Notification을 받으면 위에서 만든 `didReceiveSimpleNotification:` 을 호출하도록 옵저버를 등록하는 예제이다.
```swift
// Add Notification Observer with Selectors
let nc = NSNotificationCenter.defaultCenter()
nc.addObserver(self, 
               selector: "didReceiveSimpleNotification:", 
               name: "simple-notification", 
               object: nil)
```
여기까지 옵저버의 등록이 끝난다. 하지만 위의 두 가지 코드를 `Closure`를 이용하여 한 블록 안에서 한번에 처리할 수도 있다. 아래의 코드를 보자.

```swift
// Add Notification Observer with Closure
let nc = NSNotificationCenter.defaultCenter()
nc.addObserverForName("simple-notification", 
                      object: nil, 
                      queue: NSOperationQueue.mainQueue(), 
                      usingBlock: { (n: NSNotification!) -> () in
    let message: String? = n.userInfo["message"] as? String
    println("I've got the message \(message)")
})
```
위의 코드는 메서드를 분리해서 셀렉터를 등록하는 것에 비해 쓰기 편해 보인다. 하지만 queue 라는 것이 쓰이고 있다는 점을 주의해야 한다. 위의 예제에서는 mainQueue를 불러다 썼기 때문에 UI 업데이트에 문제가 없겠지만 만약 다른 NSOperationQueue 를 만들어서 이걸 쓰고 싶다면 필요할 때 메인스레드를 사용하도록 잘 처리해야 한다.

## Post Notification
이제 옵저버를 만들고 옵저버가 Notification 메세지를 받을 수 있도록 해 놨으니 이제는 Notification 을 쏴 볼 차례다. 아래 코드는 "simple-notification" 라는 이름의 Notification을 쏘는 예제이다. 

```swift
// Post Notification
let nc = NSNotificationCenter.defaultCenter()
let userInfo = [ "message": "Message using Notification" ]
nc.postNotificationName("simple-notification", object: nil, userInfo: userInfo)
```
"message"라는 키를 가진 NSDictionary 사전형 객체를 만들고 이를 userInfo에 넣고 Notification을 Post 한다. 

## Remove Observer
`NSNotificationCenter`는 사용 방법이 간단해서 쉽게 쓸 수 있었는데 주의해야 할 점이 있다. 옵저버는 특정 클래스 오브젝트의 것이지만 **NSNotifacationCenter는 싱글턴 인스턴스라서 여러 오브젝트에서 공유한다.** 그래서 옵저버를 등록한 오브젝트가 메모리에서 해제되면 NSNotificationCenter 에서도 옵저버를 없앴다고 알려주어야 한다.

```swift
// Remove Notification Observer from deinit
deinit {
    NSNotificationCenter.defaultCenter().removeObserver(self)
}
```
위의 코드는 NSObject형식의 모든 클래스에서 메모리가 해제될 때 호출되는 dealloc 에 옵저버를 제거하는 코드를 넣은 예제로, `removeObserver:`를 이용해 자기 자신의 오브젝트에 속한 모든 옵저버를 NSNotificationCenter에서 제거한다.

만약 제대로 제거해 주지 않으면 해당 옵저버가 소속된 오브젝트가 해제 되었을 때 Notification 이 전달되면 앱이 100% 죽게된다.

물론 특정 옵저버만 제거할 수 있는 `removeObserver:name:object` 메서드도 있다.