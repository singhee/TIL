# ADB (Android Debug Bridge)

## 자주쓰는 adb 명령어
1. Restart Android device 
: 안드로이드 장비를 재시작한다.
```shell
adb reboot
```

2. Check android device connectivity
: 사용자 PC에 연결된 안드로이드 장비 혹은 애뮬레이터를 확인한다.
```shell
adb devices
```

3. connect a certain one device
: 연결된 기기가 2개 이상일 때는 특정기기로만 명령어로 보내야 한다
```shell
adb shell -s [Serial Number] 명령어
```

4. install android application
: 앱 파일명을 이용해 안드로이드 apk를 설치한다.
```shell
adb install [FILENAME].apk
adb install -r [FILENAME].apk  // 설치된 어플을 재설치(단 데이터 삭제는 불가)
adb install -s [FILENAME].apk  // 메모리카드에 설치
```

5. Kill package name
: 패키지 명을 이용하여 구동중인 안드로이드 어플리케이션을 강제 종료 시킨다.
```shell
adb shell am force-stop [packagename] 
```

6. remove app
: 안드로이드 앱을 언인스톨하여 삭제한다.
```shell
adb uninstall pakagename
```

7. uninstall package
: 안드로이드 앱을 언인스톨하여 삭제한다.
```shell
adb uninstall pakagename
adb shell am force-stop packagename
```

8. list package app
: 안드로이드 장비에 설치된 모든 앱의 패키지명을 가져온다.
```shell
adb shell pm list packages -f
```

9. adb ls 
: 안드로이드 장비 중 해당 폴더의 리스트를 불러온다.
```shell
adb shell ls mnt/sdcard/document
adb shell (폴더위치)
```

http://developer.android.com/intl/ko/tools/help/adb.html
