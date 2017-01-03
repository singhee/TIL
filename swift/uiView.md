# Swift UIView

## UIView 생성방법
1. Code
2. Interface Builder 

### Code를 통해 UiView 생성하기 
```swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil) -> Bool {
        self.window = UIWindow(frame: UIScreen.main.bounds)
        
        let button = UIButton(type: .infoDark)
        button.frame.origin = CGPoint(x: 100, y: 200)
        
        let view = UIView(frame: CGRect(x: 100, y: 100, width: 50, height: 50))
        view.backgroundColor = UIColor.blue
        
        if let window = self.window{
            window.rootViewController = UIViewController()
            window.addSubview(button)
            window.addSubview(view)
            window.backgroundColor = UIColor.green
            window.makeKeyAndVisible()
        }
        
        return true
    }
```
위처럼 Code를 통해 UIView를 생성하는 것은 번거롭다. -> Interface Builder

### Interface Builder를 통한 UIView생성
```swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil) -> Bool {
    
        self.window = UIWindow(frame: UIScreen.main.bounds)
        // 여기까진 Code로 생성하는 방법과 동일하다. 

        // xib를 통해 view를 생성한다. : Bundle이용    
        let bundle = Bundle.main

        if let arr = bundle.loadNibNamed("FirstController", owner: nil, options: nil){
     // if let view = bundle.loadNibNamed("FirstController", owner: nil, options: nil)?[0] as? UIView 와 동일
            if let view = arr[0] as? UIView{
                window?.addSubview(view)
            }
        }
        
        if let window = self.window{
            window.rootViewController = UIViewController()
            window.makeKeyAndVisible()
        }
        return true
    }
```

UIView를 Code나 Interface Builder로 생성하는 것은 이벤트 처리가 되지 않는다.
-> window.rootViewController = UIViewController() : UIViewController 는 
부모의 타입이고 이벤트 처리가 되지 않은 이유는 UIViewController로 View를 생성한 것이 아니라 View를 별도로 window에 올렸기 때문에 이벤트 처리가 되지 않았던 것이다.
-> 그렇다면 Controller를 지정해 View를 띄어준다!!! (2가지 방법)

#### 첫 번째 방법
1-1) ViewController에서 loadView() Override해서 MyView xib를 load해서 View 지정.
1-2) window.rootViewController = ViewController(nibName: "MyView", bundle: nil) 
```
   func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil) -> Bool {
       self.window = UIWindow(frame: UIScreen.main.bounds)
       
       if let window = self.window{
           window.rootViewController = ViewController(nibName: "MyView", bundle: nil)
           window.makeKeyAndVisible()
       }
       return true
   }
```

#### 두 번째 방법
 2-1) window.rootViewController = FirstController()
 2-2) Outlet 지정 : 단, xib 와 Controller 이름이 같아야 한다.
 2-3) File's Owner 지정은  Custom Class를 지정한다. 그리고 난 뒤 View를 설정해준다.
```swift
   func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil) -> Bool {
       self.window = UIWindow(frame: UIScreen.main.bounds)
       
       if let window = self.window{
           window.rootViewController = FirstController()
           window.makeKeyAndVisible()
       }
       return true
   }

```