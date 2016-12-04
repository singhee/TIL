# Manifest
AndroidManifest.xml 은 안드로이드 어플리케이션에 대한 각종 정보를 기술한 어플리케이션 명세서라 할 수 있다.

## 기술 내용

1. android application 을 위한 Java Package 정의한다.
2. Application 을 구성하는 컴포넌트들 (Activity, Service, Broadcast Receiver, Content Provider) 을 기술한다.
3. Application 을 구성하는 Component 들에 대한 Class name 정의, 처리할 수 있는 기능은 Intent-filter 를 정의한다.
4. Application 이 안드로이드 플랫폼의 제한된 API 에 대한 접근과 다른 Application 의 제한된 Component 를 사용하기 위해 필요한 권한 설정 한다.
5. 외부에서의 자신의 Component 에 대한 사용 권한을 설정할 수 있다.
6. Application 구동을 위한 최소한의 SDK 버전을 정의한다.
7. Application 이 사용하는 다른 추가적인 라이브러리를 정의한다.	


## AndroidManifest.xml
기본적으로 AndroidManifest.xml은 루트 요소가 `<manifest>`이고, 그 자식으로 `<application>` 요소가 하나 존재한다.
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="cc.foxtail.androidsample">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:theme="@style/AppTheme.NoActionBar">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```

1. <manifest>
안드로이드 어플리케이션의 패키지명과 버전정보를 정의한다. 

2. <application>
어플리케이션의 제목과 아이콘을 정의하고, <manifest> 밑에는 하나의 <application>만 정의된다.

3. <activity>
안드로이드 어플리케이션을 구성하는 네가지 Component 중에 Activity를 정의하는 요소 Activity 클래스명과 Activity의 제목을 정의할 수 있다.

4. <intent-filter>
해당 Component의 intent-filter를 가리키는 것으로, 해당 Component(Activity, Service, Broadcast, Receiver 등)가 어떤 암시적 Intent를 처리할 수 있는지 정의한다. <intent-filter>는 그 밑으로 <action>요소를 정의하여 어떤 작업을 처리할 수 있는지 정의할 수 있다. <category>요소를 정의하여 Component의 유형이 무엇인지 정의할 수 있다. 

5. <uses-permission>
안드로이드 어플리케이션의 리소스접근 및 기능 사용 권한을 정의한다.

