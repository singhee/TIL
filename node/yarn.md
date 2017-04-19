# Yarn 
`yarn`이란 Facebook에서 만든 새로운 자바스크립트 패키지 매니저로, npm을 완전히 대체하려는 시도가 아니라, npm registry에서 모듈을 가져 오는 새로운 CLI클라이언트이다. 

## Facebook은 yarn 을 왜 만들었나?
페이스북은 코드베이스가 커지면서 npm을 사용할 때 일관성, 보안, 성능에 문제를 겪게 되었고 Google, Exponent, Tilde의 개발자들과 함께 npm client를 대체할 새로운 패키지 매니저 Yarn을 만들었다. 이미 널리 쓰이고 있는 npm과 비교했을 때, yarn은 더 빠르고 안정적이라는 장점이 있다. 

## Npm의 한계
* 성능 이슈
	* 의존하는 라이브러리가 많아짐에 따라 CI(Continuous Integration)속도가 느려질 만큼 무거워짐. 라이브러리 설치도 병렬적이지 않고 의존성 트리에 따라 하나씩 받아와 많은 시간이 걸린다. 
* 비결정적 설치
	* 의존성 버전이 설치하는 시점, 순서에 결정됨에 따라 저마다 다른 파일 구조를 가지게 된다. 
		- ex) local 에서 개발할 당시 1.0.0 이었지만 CI(Continuous Integration)에서 배포할 당시는 1.0.3으로 업데이트 된 상황
		- ex) npm v3 에서 모듈 설치 순서에 따라 디렉토리 구조가 달라질 수 있음.
* versioning issue
	* [Semver(Semantic Versioning)](https://github.com/singhee/TIL/blob/master/etc/semver.md)에 따른 versioning을 할 때 버그가 발생하지 않았던 개발자에게 의존 시스템이다.
* offline 또는 외부 방화벽이 막혀있는 서버에서 의존성 모듈들을 받아오는 문제 
	* 페이스북은 node_modules의 모든 소스들을 repository에 커밋하는 방법도 사용했지만, babel의 작은 업데이트도 엄청난 양의 커밋을 만들어냈다. 그래서 두번째로 node_modules 폴더를 zip파일로 압축하여 CDN(Content Distribution Network)에 올리고 이것을 다운로드하는 식으로 변경 했으나 결국엔 인터넷이 연결되어야만 가능했다. 

## Yarn은 얼마나 빨라졌나
* 사용하고 있는 repository에서 모듈들의 install 시간을 비교해 보았더니 다음과 같이 나타났다. 

**npm install** - 1회 3분 48초 -> 2회(caching) 1분 7초
**yarn install** - 1회 3분 38초 -> 2회(caching) 13초
[Compare Yarn Performance](https://yarnpkg.com/en/compare)

## Yarn 프로세스 
1. Resolution 
* Yarn은 레지스트리에 요청을 만들고 재귀적으로 살펴서 각 의존성을 파악한다. 
2. Fetching
* 글로벌 캐쉬에 해당 모듈과 맞는 버전이 있는지 확인 한 후, 있으면 사용하고 없으면 받아와서 글로벌 캐쉬에 압축하여 저장한다. 한번 캐싱되면 유저폴더의 루트경로에 있는 `yarn_cache`안에 저장된다. 따라서 **offline 상태에서도 모듈을 재설치 할 수 있다.**
3. Linking 
* 모든 의존성이 준비되면 node_modules에 카피한다. 

Yarn은 이러한 프로세스로 병렬적인 작업이 가능해 상당한 속도를 증가시킬 수 있었다. 병렬처리를 위해 mutex를 사용하여 여러 CLI인스턴스가 충돌하지 않게끔 해준다. 


## Yarn.lock 이란
* Yarn.lock(npm의 npm-shrinkwrap.json)은 실제로 설치된 node_modules 모듈들의 버전을 저장해 어디에서나 같은 버전 같은 구조의 의존성을 갖게끔 해주는 기능이다. npm과 다른점은 yarn에서는 자동으로 매 install마다 yarn.lock이 생성 된다는 점이다. 자동으로 생성되기 때문에 실수로 누락될 가능성이 감소된다. 
* 사실, npm의 package.json에 정확한 버전을 명시하는 방법도 있지만, 항상 가능하거나 이상적이지는 않다. 해당 모듈의 owner가 내가 아니고 가져다 사용만 하는 입장이거나, 모듈의 owner이더라도 때로는 버그들이 수정될 수 있게 자동으로 minor버전의 업데이트는 허용하게 끔 하고 싶을때가 있기 때문이다.
* 따라서 npm에서는 shrinkwrap, yarn에서는 yarn.lock이라는 기능을 만든것이다. 
* yarn install 명령 후 자동생성 된 yarn.lock 파일을 보면 다음과 같다. 

```
/*
* yarn 파일은 직접 수정하면 안된다.
* 다른 시스템에서도 같은 의존성들이 설치되기 원하면 yarn.lock도 repository에 커밋해야 한다.
*/
 
babel-runtime@^6.0.0, babel-runtime@^6.11.6, babel-runtime@^6.9.0, babel-runtime@^6.9.1:
// 6.0.0, 6.11.6, 6.9.0, 6.9.1 의 버전들이 필요하지만 
// npm ^ 규칙에 따라 6.11.6(이중 가장최신) 으로 대체할 수 있으므로 6.11.6만 설치
version "6.11.6"
resolved "https://registry.yarnpkg.com/babel-runtime/-/babel-runtime-6.11.6.tgz#6db707fef2d49c49bfa3cb64efdb436b518b8222"
dependencies:
  core-js "^2.4.0"
  regenerator-runtime "^0.9.5"
 
babel-template@^6.14.0, babel-template@^6.15.0, babel-template@^6.16.0, babel-template@^6.8.0:
version "6.16.0"
resolved "https://registry.yarnpkg.com/babel-template/-/babel-template-6.16.0.tgz#e149dd1a9f03a35f817ddbc4d0481988e7ebc8ca"
dependencies:
  babel-runtime "^6.9.0"
  babel-traverse "^6.16.0"
  babel-types "^6.16.0"
  babylon "^6.11.0"
  lodash "^4.2.0"
 
babel-traverse@^6.14.0, babel-traverse@^6.15.0, babel-traverse@^6.16.0, babel-traverse@^6.8.0:
version "6.16.0"
resolved "https://registry.yarnpkg.com/babel-traverse/-/babel-traverse-6.16.0.tgz#fba85ae1fd4d107de9ce003149cc57f53bef0c4f"
dependencies:
  babel-code-frame "^6.16.0"
  babel-messages "^6.8.0"
  babel-runtime "^6.9.0"
  babel-types "^6.16.0"
  babylon "^6.11.0"
  debug "^2.2.0"
  globals "^8.3.0"
  invariant "^2.2.0"
  lodash "^4.2.0"
```

## Yarn 사용법
* yarn은 기존의 npm을 사용하고 있는 사람들이 어려움 없이 이전할 수 있도록 상당히 비슷한 명령어와 시스템을 갖추고 있다.
* (node 6v 버전을 설치해야 오류가 나지 않는다.)
* 초기화
	* `yarn init` : package.json을 생성하는 대화형 프로세스가 진행된다. 이미 pacakge.json이 있을 경우 재사용한다.
* 모듈 설치
	* `yarn install`
	* `yarn install –flat` : 한 모듈당 한 버전만 설치하는 install (propmt를 통해 의존성분석에서 발견된 버전 중 어느버전을 설치할 지 선택)
* 의존성 추가
	* `yarn add {모듈명}` : production
	* `yarn add –dev {모듈명}` : development
* 의존성 삭제
	* `yarn remove` {모듈명}
* 의존성 업그레이드
	* `yarn upgrade`
