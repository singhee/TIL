# CocoaPods
`CocoaPods`는 자바프로그래밍의 'Gradle'과 같이 오픈소스 라이브러리를 관리해 주는 `Objective-C` 라이브러리 매니저(또는 Dependancy management tool)이다. iOS 플랫폼의 제품을 개발할 때 대부분의 개발자들은 Open 라이브러리를 사용할 것이다. 그런데 본인의 프로젝트에 적용하기 위해서는 라이브러리 파일을 자신의 폴더에 하드 복사를 해주던지, 소프트 복사를 해주어야 할 것이다. 하지만 라이브러리가 업데이트 되거나 하게되면 다시 다운로드를 받고 다시 복사를 해야하는 불편함이 있다. `CocoaPods`를 사용하면 주요 `Objective-C` 라이브러리의 이름과 버전을 파일에 기록해 두면 최신 소스 받아오기, 업데이트, 프로젝트에 추가하는 작업을 자동화 할 수 있다. 
 


## 설치 및 사용 순서
다운로드 및 설치 방법은 다음과 같다. Mac 에서는 Ruby 가 이미 설치되어 있기 때문에 터미널에서 해당 프로젝트 위치로 이동하여 `sudo gem install cocoapods` 만 입력하면 된다.
```terminal
$ sudo gem install cocoapods
$ pod setup
```
설치가 끝났으니, 다음은 `CocoaPods`를 적용할 `Xcode project` 파일이 있는 폴더로 이동한 후`$ touch podfile`명령으로 `Podfile`을 생성하고, `$ open -e podfile`명령으로 파일을 연다. `Podfile`은 `CocoaPods`의 설정파일이다. 아래코드에서 가장 첫 번째 줄은 사용되는 플랫폼을 명시해주는 부분이다. 두 번째 줄 부터는 내가 사용할 라이브러리의 이름과 버전을 기록하는 부분이다. 이 부부은 `'`와 `~>` 등의 기호를 양식에 잘 맞춰서 입력해야 한다. 

```
$ touch podfile
$ open -e podfile
```

```
platform :ios
pod 'JSONKit', '~> 1.4'
pod 'Reachability', '~> 3.0.0'
```

아래에 명시된 명령어를 입력하면 `Podfile`을 참조하여 `CocoaPods`가 웹에 있는 해당 라이브러리를 다운로드 하고, 이 라이브러리들만 모아서 컴파일해주는 Xcode project를 추가로 포함한 Workspace 파일(`.xcworkspace`)을 생성해 준다. 여기서 주의할 점은 새롭게 생성된 Workspace 파일을 열어야 이후 제대로 빌드할 수 있다.
```
$ pod install
```

`$ pod install` 후 포함된 라이브러리들은 `#import " "` 가 아닌 `#import < >` 를 이용해서 마치 시스템 라이브러리를 참조하듯이 사용 할 수 있다. 

이후에 내가 사용하는 라이브러리들이 업데이트 된다면 `Podfile`을 열어서 버전을 수정한 후 다음 명령어를 입력하면 된다. 
```
$ pod update
```

## Search
`CocoaPods`에서 사용 가능한 라이브러리를 찾는 방법은 크게 두 가지가 있다.
	
	1. search
	2. list

```
$ pod search [QUERY]	
```
 search 를 위한 Options 들은 다음과 같다.

`-full` : 이름, 요약, 설명등을 통한 검색
`-stats` : 추가적인 sats로 보여주기
`ios` : iOS 라이브러리 한에서만 찾기
`osx` : OSX 라이브러리 한에서만 찾기


