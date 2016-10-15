# manpage 
	manpage(man) - 함수에 대한 signature나 사용법을 볼 수 있다.
	ex) man strcpy

`q` : 나가기
`space` : 페이지 단위 이동
방향키 안되는 경우 : `j` 내려감 , `k` 올라감
검색 :  `/` + `'searchItem'`

#### manpage 번호의 의미
	1번 - 리눅스 쉘
	2번 - 리눅스 시스템 콜 (Windows: Win32/64 API)
	3번 - 준 라이브러리 함수리

	man strcpy -> strcpy(3)
	man close -> close(2)
	man mv -> mv(1)		


> ##### 라이브러리 : 함수의 집합 - 클래스의 집합 ( 그래픽 라이브러리, 네트워크 라이브러리 )
> ##### 엔진 : 라이브러리의 집합
> ##### 프레임워크 : 라이브러리의 집합 + 정해진 흐름 ( 내부적으로 동작하는 흐름에 맞는 시점에 알맞은 코드를 작성 ), 용어에 대한 명확한 정의 필요