# iOS 외부 키보드 연결 여부 파악하는 방법
iOS 개발을 진행하다가 iPad Pro에 블루투스 키보드를 연결했을 때와, 연결하지 않고 내부 키보드를 사용할 때를 구분해야 할 필요가 생겼었다. 원래 iOS9 이전 버전에서는 키보드의 연결 여부를 확인할 수 있는 가장 신뢰적인 방법이 `UIKeyboardWillShowNotification`을 수신하는 방법이었다. 이는 내부 가상 키보드를 사용할 때만 발생하며, 외부 키보드를 사용할 때에는 실행되지 않았기 때문이다. 

그러나 이 동작은 iOS9 에서 부터 변경되었다. `UIKeyboardWillShowNotification`는 이제 내부 가상 키보드 뿐만 아니라, 외부 키보드 연결에도 키보드 도구 모음이 표시되면서 이벤트가 발생한다. 

그렇다면, iOS9 에서 사용할 수 있는 보다 안정적인 외부 키보드 연결 여부를 파악하는 방법은 무엇일까? 그것은 내부 가상 키보드 frame이 offscreen인지 확인함으로써 확인할 수 있다. 실제로 일반 가상 키보드가 표시되는 경우에는 키보드 프레임이 화면 크기 내에 있는 것을 확인할 수 있을 것이다. 그러나 외부 키보드가 연결되면서 키보드 도구 모음이 표시되면, 가상 키보드 프레임이 화면 밖에 위치하게 된다. 이 때의 키보드 프레임이 offscreen 인지 아닌지를 확인하여 키보드 툴바가 표시되었는지를 확인하여 외부 키보드가 연결되었는지를 파악할 수 있는 것이다. 

```swift
func keyboardWillShow(_ notification: Notification) {
         let keyboardFrame = (notification.userInfo?[UIKeyboardFrameEndUserInfoKey] as? NSValue)?.cgRectValue
         let keyboard = self.view.convert(keyboardFrame!, from: self.view.window)
         let height: CGFloat = self.view.frame.size.height
         if (keyboard.origin.y + keyboard.size.height) > height {
             return true
         }
         return false
     }
```


