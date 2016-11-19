# 도커(Docker)
클라우드와 같이 잘 짜여지고, 잘 나뉘어진 거대한 시스템에서야 그렇다 치더라도 가상 머신은 여러모로 손실이 많은 수단 중 하나이다. 가상 머신은 격리된 환경을 구축해준다는 데서 매력적이긴 하지만, 실제 배포용으로 쓰기에는 성능 면에서 매우 불리한 도구라고 할 수 있다. 운영체제 위에서 또 다른 운영체제를 통째로 돌린다는 것 자체가 리소스를 비효율적으로 활용할 수밖에 없다.

이런 가운데 가상 머신의 단점은 극복하면서 장점만을 극대화하기 위한 최적화가 계속해서 이루어지고 있다. 도커 역시 이런 단점을 극복하기 위해서 나온 가상화 어플리케이션이다. 도커는 단순한 가상 머신을 넘어서 어느 플랫폼에서나 재현가능한 어플리케이션 컨테이너를 만들어주는 걸 목표로 한다. LXC(리눅스 컨테이너)라는 독특한 개념에서 출발하는 Docker의 가상화는 기존에 운영체제를 통째로 가상화하는 것과는 접근 자체를 달리한다. 가상 머신이라고 하기에는 격리된 환경을 만들어주는 도구라고 하는 게 맞을 지도 모른다.

## Docker 설치하기 [설치](https://docs.docker.com/engine/installation/)
각 운영체제 별 Docker의 설치 방법은 공식 홈페이지에 잘 정리되어 있지만, Ununtu 환경에서의 사용을 권장한다. 
```
curl -s https://get.docker.io/ubuntu/ | sudo sh
```
위의 명령을 실행하기 위해서는 curl이 설치 되어있어야한다.


```
$ docker -v
Docker version 0.7.6, build bc3b2ec
```
```
sudo ufw allow 4243/tcp

```

기본적으로 docker의 대부분의 명령어를 실행 시 root 권한이 필요하다. 따라서 sudo를 사용해야하는 번거로움이 따라오는데, 이를 위해 현재 유저를 docker 그룹에 포함시켜 주면 된다.

```
$ sudo groupadd docker
$ sudo gpasswd -a ${USER} docker
$ sudo service docker restart

```
명령어들을 실행하고 재로그인을 하게 되면 더 이상 sudo 명령어를 앞에 붙이지 않아도 docker 명령을 사용할 수 있게된다. 

## 이미지(Image)
도커를 시작하기 위해서는 이미지의 개념에 대한 이해가 필요하다. 이미지는 `베이스 이미지`와 `도커이미지`로 구분된다.


### 베이스 이미지
`베이스 이미지`란, 리눅스 배포판의 커널 영역을 제외한 `사용자 공간/영역(유저랜드)`만을 이미지에 포함한 파일을 의미한다. 이때 `사용자공간/영역`이란, 커널 공간/영역과 관련된 것들을 제외한 부팅시 필요한 몇몇 실행파일과 라이브러리, 그리고 패키징 시스템(우분투의 apt같은)것들을 포괄해서 부르는 명칭이다. 이 `사용자공간/영역`에 Redis나 Nginx등을 설치해서 `베이스 이미지`로 배포하기도 한다.
`유저랜드`에는 각 리눅스 배포판의 특징이 고스란히 있어서 각 배포판 고유의 패키징 툴도 그대로 활용 할 수 있다. 이 베이스 이미지는 변형되거나 뭔가가 추가 설치될 수 있는데 이렇게 해서 변형된 이미지를 `도커 이미지`라 부른다.

### 도커 이미지
도커 이미지는 베이스 이미지에서 몇가지를 라이브러리, 프로그램, 소스 파일등을 추가/설치/저장한 이미지다. 도커 이미지는 베이스이미지부터 상속받듯이 만들어진다. 즉, 베이스이미지에서 추가되거나 변형되는 부분만 `도커 이미지`에 저장된다. 이렇게 만들어진 `도커 이미지`는 또 수정될 수 있는데, 이렇게 수정되면 또 그 변형된 부분만 다시 `도커 이미지`에 저장된다.
첫번째 변화후 저장된 `도커 이미지`를 A라 하고, 두번째 변화후 저장된 `도커 이미지`	를 B라 하면,
```
베이스 이미지 + A + B

```
를 하여 `도커 이미지` 실행이 시작된다. 이 때 도커는 필요한 파일을 부모이미지부터 자식이미지까지 싹 받아서 합친다. 이렇게하여 실행된 이미지를 우리는 `컨테이너`라 부른다.

자, 그럼 실제로 image에 대해 더 깊게 파헤쳐보도록 하자.
먼저 `docker images`명령어로 시스템에 어떤 이미지가 있는지 확인해 보자.
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
```
docker images 명령어는 현재 시스템에서 사용가능한 이미지 일람을 보여준다. 이미지는 크게 세 가지 방법을 통해 추가할 수 있다.

하나는 `docker pull <이미지 이름>`을 통해 가져오는 방법이다. 이 명령어를 사용하면 docker.io의 공식 저장소에서 이미지를 다운로드 받아온다. 리눅스에서 `apt-get`이나 `yum`혹은 `gem`이나 `pip`,`cpan`,`npm`같은 명령어를 사용해본 개발자라면 바로 이해할 수 있을 것이다. 이런 유틸리티를 사용해본 적이 없다고 하더라도 마찬가지 개념으로 docker이미지 파일들을 관리하는 중앙 저장소가 있다고 이해해도 된다. 독특한 점은 `install`이 아닌 `pull`명령어를 사용한다는 점이다. 

(두 번째 방법은 `commit`을 하는 방법이다. 마지막 방법으로는 Dockerfile을 통해서 기술된 과정을 거쳐 생성하는 방법이다.)

여기서는 편의상 ubuntu이미지를 다운로드 받아오도록 하겠다. 이 이미지에 대한 정보는 웹을 통해 확인할 수 있다. 공식 저장소에 있는 이미지 정보들은 [https://index.docker.io](https://index.docker.io)에서 확인할 수 있으며 우분투 이미지에 대한 정보는 [여기](https://index.docker.io/_/ubuntu/)에서 확인할 수 있다.

```
$ docker pull ubuntu
Pulling repository ubuntu
04180f9bd8a6: Download complete
1e548c932d40: Download complete
5e94ff221e91: Download complete
b750fe79269d: Download complete
3e47bae8d07a: Download complete
43461fe97ba1: Download complete
8dbd9e392a96: Download complete
511136ea3c5a: Download complete
27cf78414709: Download complete
46e4dee27895: Download complete
86f6383454b4: Download complete
7a4f87241845: Download complete
1957a8106a4c: Download complete
b74728ce6435: Download complete
```
도커가 무언가를 열심히 다운받는다. 사실 이것들 하나하나가 이미지라는 것을 이해할 필요가 있다. 우분투 하나를 설치했을 뿐인데, 무려 14개의 이미지를 다운로드 받았다. 다운로드가 끝나면 이제 다시 이미지들을 확인해본다.

```
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              saucy               43461fe97ba1        3 days ago          144.6 MB
ubuntu              raring              5e94ff221e91        3 days ago          133.6 MB
ubuntu              quantal             3e47bae8d07a        3 days ago          127.6 MB
ubuntu              lucid               04180f9bd8a6        3 days ago          139.6 MB
ubuntu              precise             1e548c932d40        3 days ago          125.9 MB
ubuntu              12.04               8dbd9e392a96        9 months ago        128 MB
ubuntu              latest              8dbd9e392a96        9 months ago        128 MB
ubuntu              12.10               b750fe79269d        10 months ago
```
위에서 다운로드 받은 것들 하나하나가 이미지라고 이야기 했는데, 이상하게도 다운로드 받은 이미지 수와 실제로 출력되는 이미지 수가 맞지 않다. 이를 확인하기 위해서는 `-a`플래그를 통해 모든 이미지를 출력해 볼 수 있다.

```
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              saucy               43461fe97ba1        3 days ago          144.6 MB
<none>              <none>              86f6383454b4        3 days ago          0 B
ubuntu              raring              5e94ff221e91        3 days ago          133.6 MB
ubuntu              quantal             3e47bae8d07a        3 days ago          127.6 MB
<none>              <none>              1957a8106a4c        3 days ago          0 B
ubuntu              lucid               04180f9bd8a6        3 days ago          139.6 MB
<none>              <none>              7a4f87241845        3 days ago          0 B
ubuntu              precise             1e548c932d40        3 days ago          125.9 MB
<none>              <none>              46e4dee27895        5 weeks ago         0 B
<none>              <none>              b74728ce6435        5 weeks ago         0 B
<none>              <none>              511136ea3c5a        7 months ago        0 B
ubuntu              12.04               8dbd9e392a96        9 months ago        128 MB
ubuntu              latest              8dbd9e392a96        9 months ago        128 MB
ubuntu              12.10               b750fe79269d        10 months ago       175.3 MB
<none>              <none>              27cf78414709        10 months ago
```
`-a(-a=false: show all images)`플래그는 이미지를 빌드하는 과정에서 생성되는 모든 이미지를 보여준다. 자세히 보면 저장소(Repository)가 `<none>`인 이미지들이 존재하는 것을 알 수 있다. `-a`플래그의 설명에서 알 수 있듯이 이 이미지들은 최종적인 이미지를 생성하는 과정에서 생성되는 중간 이미지들이다. 나머지 다수의 우분투 이미지들은 TAG에서 볼 수 있듯이 다양한 버전의 이미지가 등록된 것을 알 수 있다.





