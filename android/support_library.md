# 안드로이드 Design Support Library
안드로이드 Design Support Libary를 얘기하기에 앞서, Material Design에 대해 알 필요가 있다.

## Material Design 
Material Design은 2014년에 소개된 구글의 디자인 철학이자 가이드라인이다. 모바일 뿐만 아니라 데스크탑을 아우르는 새로운 디자인 가이드라인을 제시함으로써 통일된 UI/UX를 제공하겠다는 것이 목적이다. Material Design은 플랫폼에 상관없이 매우 일관성 있는 디자인 경험을 제공할 뿐만 아니라, 가이드라인이 잘 제시되어 있어 적용하기 쉽다.

> "Material Design에서 표면과 그림자는 물리적인 구조를 형성하여, 사용자들이 화면 상의 어떤 부분을 터치할 수 있고 움직일 수 있는지 쉽게 이해할 수 있도록 돕습니다. 모든 움직임에는 의미가 있으며, 화면 요소들 간의 관계를 명확히 하고 세세한 디테일을 통해 사용자에게 이러한 관계를 알려줍니다." 
> ["구글 디벨로퍼 코리아 블로그 - 머티리얼 디자인 (Material Design) 이란?"](https://developers-kr.googleblog.com/2014/07/this-is-material-design.html)


## 안드로이드와 Material Design
Material Design이 Android 롤리팝과 함께 공개되면서 기본적으로 Material Design이 적용되었는데, 이 Material Design을 직접 안드로이드 앱 개발에 구현하는 것이 매우 어려웠다. v7-appcompat, cardview, recyclerview 등 일부 Material Design을 적용하기 위한 서Support Library가 제공되었지만, 그 기능이나 요소들이 매우 제한적이어서 별도의 Custom Library를 사용해야 했다. 

