# Semver (Semantic Versioning)
`Semver(Semantic Versioning)`은 Github 창업자인 톰 프레스턴-워너(Tom Preston-Werner)가 작성한 버전 관리 방법으로, 버전 형식에 의미를 부여하여 좀 더 체계적인 버전 관리를 위한 제안이다. 배포 정책이나 시기에 따라서 버전이 매겨지거나, 의미 없이 버전이 올라가는 것을 지양하며 versioning에 대한 명확한 의미를 부여한다. 현재 많은 오픈소스들이 버전관리를 `SemVer`로 하고 있다.(node.js 의 npm, Angular 2 등)

`SemVer`로 인한 장점으로는 개발자가 versioning 대한 명확한 기준을 통하여 version만으로도 외부 라이브러리들의 도입이 가능하며 versioning에 대한 고민을 덜어준다는 장점이 있다.

`SemVer`의 핵심은 다음과 같다.
* 버전의 형식은 [Major].[Minor].[Patch] 형식으로 한다. 
* 이전 버전과 호환되지 않는 API변경은 Major 버전 증가
* 이전 버전과 호환되면서 기능의 변경, 추가된 경우는 Minor 버전 증가
* 버그 수정은 Patch 버전 증가

`Semver`는 버전 형식을 3단계로 구분하면서 버전 숫자의 변동을 보고 이전 버전과 어느 정도 차이가 잉ㅆ는지 짐작할 수 있는 체계를 만들고 있다. 예를 들어 외부 라이브러리의 1.3.5버전을 쓰고 있다가 1.3.6 버전이 나왔다면 버그 수정을 위한 패치일테니 부담 없이 라이브러리를 업데이트 할 수 있을 것이고, 1.4.0이 나왔다면 조금 더 많은 내용이 변경되었지만 호환은 되는 범위이니 큰 부담은 없을 것이다. 2.0.0버전이 나왔다면 이전 버전과 호환이 되지 않는 큰 변화가 나온 것으로 파악하고 깊이 있는 검토가 필요할 것이다. 
이렇게 버전의 변화를 보고 변동량을 짐작할 수 있는 체계가 `Semver`의 핵심이다. 

[Semver 공식 문서](http://semver.org/)에서의 내용 중 요점만을 살펴보면 다음과 같다. 

```
1. Software using Semantic Versioning MUST declare a public API. This API could be declared in the code itself or exist strictly in documentation. However it is done, it should be precise and comprehensive.

SemVer를 사용하는 소프트웨어는 반드시 공개 API를 선언해야 한다. 이 API는 코드 자체로 선언하거나 문서로 명시해야 한다.

2. A normal version number MUST take the form X.Y.Z where X, Y, and Z are non-negative integers, and MUST NOT contain leading zeroes. X is the major version, Y is the minor version, and Z is the patch version. Each element MUST increase numerically. For instance: 1.9.0 -> 1.10.0 -> 1.11.0.

Major, Minor, Patch 각각의 버전은 자연수로 증가하고, 0이 앞에 붙어서는 안된다. 1.9.0 -> 1.10.0 -> 1.11.0 과 같은 형식도 가능

3. Major version X (X.y.z | X > 0) MUST be incremented if any backwards incompatible changes are introduced to the public API. It MAY include minor and patch level changes. Patch and minor version MUST be reset to 0 when major version is incremented.

API가 이전 버전과 호환되지 않게 변경된 경우에는 반드시 Major 버전을 올린다. 이 때 Minor 버전과 Patch 버전은 0으로 초기화한다.

4. A pre-release version MAY be denoted by appending a hyphen and a series of dot separated identifiers immediately following the patch version. Identifiers MUST comprise only ASCII alphanumerics and hyphen [0-9A-Za-z-]. Identifiers MUST NOT be empty. Numeric identifiers MUST NOT include leading zeroes. Pre-release versions have a lower precedence than the associated normal version. A pre-release version indicates that the version is unstable and might not satisfy the intended compatibility requirements as denoted by its associated normal version. Examples: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92.

Patch 버전 뒤에 하이픈(-)을 붙이고 마침표(.)로 구별된 식별자를 붙여 배포 전의 버전을 명명할 수 있다. 1.0.0-alpha, 1.0.0-alpha.1 과 같은 형식으로. 이 때, 정식배포 버전과 비교하면 정식배포 버전의 우선순위가 더 높다. 1.0.0-alpha < 1.0.0
```

## Semver를 써야 하는 이유 
`Sementic Versioning` 방식은 새롭거나 혁신적인 아이디어가 아니다. 이미 비슷하게 적용하고 있을 수도 있다. 하지만, "비슷한" 방식으로는 충분하지 않다. 어떠한 형태로 정식 명세를 정해서 따르지 않는다면, version number는 의존성 관리에서 무의미 하기 때문이다. 그래서 SemVer에서 지향는 것은, '비슷한' 방식이 아닌 '동일한' 체계의 versioning 이다. 

version number에 이름을 정하고 명시적인 정의를 내림으로써, 소프트웨어 사용자에게 제작자의 의도를 전달하기 쉬워진다. 의도가 명확해야만, 융통성 있는 의존성 명세를 만들 수 있다. 