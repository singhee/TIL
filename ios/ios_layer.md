# iOS Layer
iOS는 다음과 같이 4개의 층으로 구성된다. 위에 있을수록 유저와 가깝고, 아래 있을수록 하드웨어와 가깝다.

![Figure_1](../images/iOS_Layer.jpg)

* Cocoa Touch
* Media
* Core Services
* Core OS

## Cocoa Touch
화면이나 버튼 등 유저 인터페이스(UI)와 관련된 Layer이다. View hierarchy, Map Kit, Web View, Camera, Alerts, CoreMotion, Localization, Multi touch, Image Picker등 UI와 연관된 것들이 모두 여기에 포함된다.

## Media
음악, 영상, 애니메이션, 사진 등과 관련된 층이다. Core Audio, Audio Mixing, Audio Recording, Video Playback, PNG, JPEG, OpenAL, PDF, Quartz(2D), Core Animation, OpenGL등이 관련된다.

## Core Services
Foundation이 포함되는 층으로서 네트웡킹, 데이터베이스, 파일 관리와 관련되는 층이다. import Foundation을 하여 가져오는 메서드들이 여기와 관련된다. UI와는 무관하다. Collection, Core Location, Address Book, Net Services, Networking, Threading, File Access, Preferences, SQLite, URL, Utilities등이 포함된다.

## Core OS
C언어로 짜여진 유닉스 OS층이다. 일반 앱 개발자로서 거의 직접 접근하지 않는 층이다. OSX Kernel, Power Management, Mach 3.0, Keychain Access, BSD, Certificates, Sockes, File System, Security, Bonjour 등이 이 영역에 포함된다. 