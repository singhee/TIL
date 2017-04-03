# CocoaPods Subspecs
Xcode에서 CocoaPods를 이용하면 Sub Module을 만들 수 있다. 이렇게 모듈을 만들면서 여러 가지 기능을 하나의 모귤에 추가하다 보면 다른 프로젝트에서는 불필요한 코드들이 추가되는 상황이 발생하게 된다. 이런 경우 [Subspecs](https://guides.cocoapods.org/syntax/podspec.html#group_subspecs)를 활용하면 **선택적 의존성**을 설정할 수 있다. 

[Optional CocoaPod dependencies using Subspecs](http://www.dbotha.com/2014/12/04/optional-cocoapod-dependencies/)에 제시된 예시를 보게 되면

```cocoapod
// Kite-Print-SDK.podspec
Pod::Spec.new do |spec|
  spec.name     = 'Kite-Print-SDK'
  spec.version  = '1.0.2'
  spec.license  =  'MIT'
  spec.homepage = 'https://github.com/OceanLabs/iOS-Print-SDK'
  spec.authors  = {'Deon Botha' => 'deon@oceanlabs.co'}
  spec.summary  = '...'
  spec.source   = {
    :git => 'https://github.com/OceanLabs/iOS-Print-SDK.git',
    :tag => '1.0.2'}
  spec.source_files =
    ['PSPrintSDK/OL*.{h,m}', 'PSPrintSDK/CardIO*.h']
  spec.resources =
    ['PSPrintSDK/KitePrintSDK.xcassets', '*.lproj']
  spec.dependency   'SDWebImage'
  spec.dependency   'SVProgressHUD'
  spec.dependency   'AFNetworking', '2.5.0'
  spec.dependency   'UICKeyChainStore', '~> 1.0.4'
  spec.requires_arc = true
  spec.platform     = :ios, '7.0'
  spec.social_media_url = 'https://twitter.com/dbotha'
  spec.default_subspec = 'Lite'

  spec.subspec 'Lite' do |lite|
  # subspec for users who don't want the third party PayPal
  # & Stripe bloat
  end

  spec.subspec 'PayPal' do |paypal|
    paypal.xcconfig =  
        { 'OTHER_CFLAGS' => '$(inherited) -DKITE_OFFER_PAYPAL' }
    paypal.dependency   'PayPal-iOS-SDK', '~> 2.4.2'
  end

  spec.subspec 'ApplePay' do |apple|
    apple.xcconfig =   
        { 'OTHER_CFLAGS' => '$(inherited) -DKITE_OFFER_APPLE_PAY' }
    apple.dependency      'Stripe', '2.2.0'
    apple.dependency      'Stripe/ApplePay'
  end
end
```

위의 코드에서 살펴봐야 할 부분은 [subspec](https://guides.cocoapods.org/syntax/podspec.html#subspec) 과 [default_subspecs](https://guides.cocoapods.org/syntax/podspec.html#default_subspecs) 이다.
이 pod의 의존성을 부모 프로젝트에서 아래와 같이 지정 했다고 가정하자.

```cocoapod
pod "Kite-Print-SDK", "~> 1.0"
```

부모 프로젝트에는 'PayPal'과 'ApplePay' 부분을 제외한 의존성이 설정되었다. 단, default_subspecs에 의해 'Lite' 모듈은 포함된다.

'PayPal'과 'ApplePay'에 관련된 의존성 추가는 필요에 따라 아래와 같이 지정할 수 있다.

```cocoapod
pod "Kite-Print-SDK", "~> 1.0"
pod "Kite-Print-SDK/PayPal", "~> 1.0"
pod "Kite-Print-SDK/ApplePay", "~> 1.0"
```

여기까지 선택적 의존성을 설정하는 것에 대한 모든 부분을 설명하였다.


추가적으로 알아야할 내용은 다음과 같다.
* 모든 subspec은 암시적으로 이를 둘러싸는 Root Spec을 상속 한다.
* default_subspecs는 다음과 같이 여러 개를 동시에 지정할 수 있다.
	* spec.default_subspec = ['Lite', 'PayPal']
* default_subspecs를 지정하지 않으면 모든 subspec이 포함된다.
* 'OTHER_CFLAGS'를 이용해서 소스 레벨에서 의존성 지정 여부를 확인 할 수 있다.
