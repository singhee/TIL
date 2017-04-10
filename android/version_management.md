# Versioning Android
소프트웨어 개발자라면 버전 관리(Version Management)가 왜 중요한지 알고 있을 것이다. 하지만 어디에도 이렇게 관리해야만 한다는 규정(De facto Standard)은 없고, 경험상으로만 어떻게 하면 좋다는 이야기들만 돌아다닐 뿐이다. 그럼에도 소프트웨어 프로젝트는 출시 이후에도 계속 유지 보수를 해야하며, 이러한 프로젝트는 버전 관리가 매우 중요하다는 것이 사실이다. 

여기에서는 안드로이드 프로젝트에서 버전 관리에 대한 좋은 방법을 설명한다. 

## Android 의 버전 체계
Android에서는 `versionCode`와 `versionName`이라는 2가지 버전 체계가 존재한다. `versionCode`는 계속 증가하는 값으로서 Google Play Store에서 APK의 버전이 변경 되었다는 것을 추적하기 위해서 사용되며 `versionName`은 앱을 개발하는 개발자가 이해하기 쉽도록 문자열로 버전을 기재할수 있도록 되어 있는 값이다. 이 2가지 값은 Android Studio의 경우 프로젝트 레벨 폴더의 `build.gradle` 파일에서 확인이 가능하다.

```android
android {
    ...
defaultConfig {
        ...
        minSdkVersion 15
        targetSdkVersion 23
        versionCode 15
        versionName "1.4.6"
    }
}
```

여기에서 주의할 점은 `versionCode`는 계속 증가하는 값이어야 하며, 한 번 APK를 올리면 그때 사용한 `versionCode`는 **재사용할 수가 없다**. 이 경우 무조건 더 높은 `versionCode`를 사용해야 하며, 과거의 APK로 Rollback하여 돌아갈수 없다는 뜻이기도 하다. Google이 정책적으로 Rollback을 허용하지 않는 이유는, 이를 허용하면 **DevOps 차원에서 몇 가지 관리상의 문제점이 생기기 때문**인데 상세한 내용에 대해서는 Play Store와 관련된 Documentation에 나와 있으니 한 번쯤 찾아보면 좋다. (꼭 앱에만 해당되는 내용은 아니니 서비스를 개발/유지/보수하는 개발자라면 읽어볼만한 내용이다.)

그렇다면 `versionName`은 어떤 영향이 있는가? 이 값은 순전히 개발자 편의를 위해 아무 문자열이나 넣어도 되도록 만들어져 있다. 이렇게 한 의도는 **개발팀마다 버전 관리 체계가 제각각이기 때문이다.** 어느 팀에서는 날짜와 시간을 쓰기도 하고 어느 팀은 코드네임을 붙이기도 하며 어느팀은 약속된 숫자를 쓰기도 한다. 그러나 보통 semver(유의적 버전) 체계를 많이 사용한다. 

이들 값은 build.gradle 파일을 통해 APK가 빌드될 때 Build Configuration Code에 자동으로 포함되어 빌드되기 때문에 앱의 Java Code에서 다음과 같이 불러올수 있다. 이 방법을 사용하여 최신 버전 업데이트 또는 강제 업데이트 등의 기능을 구현할수 있다. 

```
BuildConfig.VERSION_CODE // versionCode를 가져올수 있다.
BuildConfig.VERSION_NAME // versionName를 가져올수 있다.
```

## GIT을 통한 APK 버전관리
이런 요소들을 통해서 App Version관리를 할 수 있음에도 불구하고 관리가 잘 안되는 이유는 버전을 손으로 써주는 일이 생각보다 귀찮고, 기껏 수작업으로 매겨놓은 버전과 소스 컨트롤 시스템에서 관리되는 버전을 서로 잘 연관짓기가 힘들다는 것이다. 따라서 버전도 자동으로 써주면서 소스 컨트롤과 잘 매칭되도록 기록을 남겨주는 방법이 필요했다. 그것이 바로 GIT과 연동하는 방법이다.

GIT에서 소스의 변경점을 추적하는데 도움이 되는 기본 정보가 무엇이냐 묻는다면 다음의 3가지를 꼽을수 있겠다.

* 브랜치 (Branch)
* 해시 값 (GIT Hash)
* 리비전 카운트 (Revision Count)

이들 3가지 정보를 APK와 연관 시키는 것이 본 아이디어의 핵심이다. (팀이 버전관리를 하는 최우선 목적이 빌드의 결과물이 어느 소스에서 나왔는지를 쉽게 추적 가능케해야 한다는 전제를 바탕으로 할 경우에) 이클립스의 Ant 빌드 체계에서는 이를 위한 설정 등이 쉽지가 않았는데 Android Studio의 Gradle 빌드 체계에선 몇가지 코드만을 추가하면 간단하게 이루어진다. 단, **빌드되는 환경이 GIT을 실행 가능해야 하며, 이미 안드로이드 앱 코드 자체가 GIT으로 관리되고 있어야만 한다.**

우선 브랜치와 해시값 그리고 리비전 카운트를 GIT에서 얻어오는 코드를 소개한다. 이들 값을 gradle-git 플러그인으로 불러올 수도 있지만 굳이 플러그인을 깔고 싶지는 않아서 subprocess로 GIT를 실행하여 정보를 얻어올 수 있도록 하였다. 이 3가지 함수를 앱 폴더의 build.gradle 최상단에 포함한다.

```
def gitBranch() {
    return 'git rev-parse --abbrev-ref HEAD'.execute().text.trim()
}

def gitSha() {
    return 'git rev-parse --short HEAD'.execute().text.trim()
}

def getRevisionCount() {
    return Integer.parseInt('git rev-list --count --first-parent HEAD'.execute().text.trim());
}
```

여기서 리비전 카운트만 정수값으로 파싱하도록 되어 있는데 이유는 `versionCode` 값으로 사용하기 위함이다. Gradle은 소스만 봐서는 잘 감이 안오지만 나름 Type Checking을 하는데 `versionCode` 값은 정수값만 받을수 있도록 되어 있다. 이렇게 파싱한 값을 `versionCode`에 활용하도록 한다.

```
android {
    compileSdkVersion 23
    buildToolsVersion "23.0.2"

    def currentGitBranch = gitBranch()
    def currentGitSha = gitSha()
    def currentGitRevisionCount = getRevisionCount()

    defaultConfig {
        ...
        minSdkVersion 15
        targetSdkVersion 23
        versionCode currentGitRevisionCount
        ...
    }
}
```

리비전 값은 소스를 commit할 때마다 자동으로 계속 증가하는 값이므로 Google이 요구한 `versionCode` 값에 알맞는 코드값이라 할수 있다. 이로서 `versionCode` 관리의 불편함은 해소되었다.
이제 브랜치 값과 해시값은 어디에 사용하면 적합할까? 그것은 바로 APK의 이름을 지정해 주는데 사용한다. 왜냐면, 안드로이드 프로젝트의 특성상 APK 파일 자체가 개발팀 내, 특히 사내에 돌아다닐 가능성이 높으며 따라서 돌아다니는 파일을 쉽게 구분하려면 APK의 파일명을 바꿔 주는게 효율적이다. 하지만 Android Build Tools에서는 파일 이름을 쉽게 바꿀수 있는 방법을 제공해주지 않기 때문에 직접 파일명을 바꿔주는 다음과 같은 스크립트를 짜야 한다. 

```
android {
     ...
     release {
        ...         
        applicationVariants.all { variant ->
           variant.outputs.each { output ->
              def changedFileName =  output.outputFile.name.replace(".apk",
                "-${variant.versionName}-${currentGitRevisionCount}-${currentGitBranch}-${currentGitSha}.apk")
              output.outputFile = new File(output.outputFile.parent, changedFileName)
              println "Output File Path: " + getString(output.outputFile.absolutePath)
           }
           println()
        }
    }
}
```

위 코드를 보면 GIT에서 얻어온 정보를 최대한 APK 파일 이름에 기재하고 있다. 이렇게 함으로서 파일명만 보아도 이 APK가 어느 브랜치와 리비전에서 나온 코드를 기반으로 빌드가 되어 있는지를 확인할 수 있게 된다. 이로서 이 APK가 어떤 버그가 수정이 되었고 문제점을 지니고 있는지 GIT로그를 통해서 추정할수 있게 된다.

그렇다면 `versionName`은 어떻게 관리하면 되는가? 이에 대한 가장 좋은 방법은 수동으로 해주는 방법이 최고였다. 만일 빌드 날짜와 시간을 기준으로 버전을 표기하는 팀이라면 Gradle로 쉽게 해결할수 있겠으나 semver를 사용하는 우리팀은 자동으로 버전을 매기는 것이 어려웠다. 주번호, 수번호, 부번호를 바꾸는 일은 어느정도 정책적인? 결정(어느 번호를 1 올릴 것인지 등등)을 요구하는데 이걸 확고한 기준으로 잘라서 기계적으로 구현하는 것이 어려웠기 때문이다. 차라리 개발자가 좀 근면하게 신경써서 `versionName`을 관리하는 것도 개발문화 육성을 위해 어느정도 필요할거란 계산도 있었다.


[출처](https://blog.cabbit.io/versioning-android-apk-with-gradle-1b656333c625)
