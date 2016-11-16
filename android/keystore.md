# Android KeyStore
제작한 Android 앱을 구글 플레이 스토어와 같은 스토어에 올려서 배포 하기 위해서는 반드시 개발자의 `KeyStore` 파일을 이용한 서명(signing)을 해야한다.(릴리즈 빌드된 APK 파일을 만들어야 한다.) 서명은 앱의 개발자임을 증명하는 아주 중요한 수단이다. 때문에 한 번 서명한 KeyStore 파일은 계속 백업 후 유지해야 한다. 같은 패키지명을 사용한다 하더라도 서명이 다르면 그 앱은 업데이트 되지 않는다. 기본적으로 Google PlayStore에 앱 업데이트로 게시되지 않는다. 그래서 서명 파일을 생성 시 그 유효기간을 앱의 예상 사용시간보다 조금 길게 유지하는 것이 좋다. 구글에서는 25년 이상을 권장한다.


## Debug vs Release
제작한 앱을 구글 플레이 스토어와 같은 스토어에 올려서 배포하기 위해서는 릴리즈 빌드된 APK 파일을 만들어야 한다. 우리가 일반적으로 개발할 때 사용하는 빌드 설정은 디버그 빌드이다.
디버그 빌드와 릴리즈 빌드의 차이점은 릴리즈 빌드는 개발자의 고유 인증서로 서명한다는 것과(디버그 인증 서명은 개발툴에서 알아서 해준다.) Zipalign 툴로 최적화 되어 있다는 점이 다르다. 필요에 따라 ProGuard 와 같은 난독화 툴을 사용하여 난독화 하기도 한다.(자바는 구조상 소스코드 노출에 취약하기 때문에 코드 보안을 위해 난돗화를 해야 하는 경우가 많다.)

릴리즈 빌드에 사용하는 개발자의 고유 인증서는 생성하고 사용한 후 절대로 잃어 버리면 안된다. iOS와는 달리 고유 인증서를 잃어 버리면, 다시는 해당 앱의 업데이트 버전을 만들 수 없게 된다.(새로 인증서를 만들게 되면 완전히 다른 앱으로 취급된다.)

## KeyStore 생성 방법
1. 기본 Command
```
$ keytool -genkey -v keystore [keystore 파일명] -alias [alias 이름] -keyalg [암호화방식] -keysize [key 크기] -validity [유효기간]

```

2. 기본 Command 실행 후 출력 화면 및 프로세스
```
$ keytool -genkey -v keystore test -alias testest -keyalg RSA -keysize 2048 -validity 18250
keystore 암호를 입력하십시오:  
새 암호를 다시 입력하십시오: 
이름과 성을 입력하십시오.
  [Unknown]:  sanghee Yoon
조직 단위 이름을 입력하십시오.
  [Unknown]:  
조직 이름을 입력하십시오.
  [Unknown]:  
구/군/시 이름을 입력하십시오?
  [Unknown]:  
시/도 이름을 입력하십시오.
  [Unknown]:  
이 조직의 두 자리 국가 코드를 입력하십시오.
  [Unknown]:  
CN=Noh, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown이(가) 맞습니까?
  [아니오]:  y

다음에 대해 유효 기간이 18,250일인 2,048비트 RSA 키 쌍 및 자체 서명된 인증서(SHA1withRSA) 생성 중
	: CN=Noh, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown
<testest>에 대한 키 암호를 입력하십시오.
	(keystore 암호와 같은 경우 Enter를 누르십시오):  
새 암호를 다시 입력하십시오: 
[test 저장 중]


```


3. Keytool Option

Keytool Option|Description
--------------|-------------
`-genkey` | Generate a key pair(public and private keys)
`-v` | Enable verbose output.
`-alias <alias_name>` (별칭) | An alias for the key. Only the first 8 characters of the alias are used.
`-keyalg <alg>` (키 알고리즘) | The encryption algorithm to use when generating the key. Both DSA and RSA are supported.
`-keysize <size>` (키 사이즈) | The size of each generated key (bits). If not supplied, Keytool uses a default key size of 1024 bits. In general, we recommend using a key size of 2048 bits or higher.
`-dname <name>` (대상 이름) | A Distinguished Name that describes who created the key. The value is used as the issuer and subject fields in the self-signed certificate. Note that you do not need to specify this option in the command line. If not supplied, Jarsigner prompts you to enter each of the Distinguished Name fields (CN, OU, and so on).
`-keypass <password>` | The password for the key. As a security precaution, do not include this option in your command line. If not supplied, Keytool prompts you to enter the password. In this way, your password is not stored in your shell history.
`-validity <valdays>` (유효기간) | The validity period for the key, in days. Note: A value of 10000 or greater is recommended.
`-keystore <keystore-name>.keystore` (키 저장소) | A name for the keystore containing the private key.
`-storepass <password>` | A password for the keystore. As a security precaution, do not include this option in your command line. If not supplied, Keytool prompts you to enter the password. In this way, your password is not stored in your shell history.










