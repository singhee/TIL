# Atomic Operations
	- 원자 단위(하나의 CPU 명령어, 인스트럭)의 operation
	- 시스템에서 사용할 수 있는 가장 작은 단위의 동기화 기법이다.
	- 한 프로세스의 일련의 모든 연산이 끝날 때까지 다른 프로세스는 그 연산에 대한 어떠한 변화도 할 수 없다.
	- 전체 연산 중 어느 하나라도 실패할 경우, 시스템은 전체 연산이 시작하기 전의 상태로 복구된다.

	참고 : http://jake.dothome.co.kr/atomic/

#### [ ++i ] "원자적이지 않다"
	1. 메모리로 부터 레지스터에 데이터를 읽어온다.
	2. 레지스터 값을 증가한다.
	3. 다시 메모리에 반영한다.

	: 대부분 CPU 는 해당 명령(정수연산)을 원자적으로 처리해주는 기능을 가진다.
 	  => mutex, critical section 등 운영체제 차원의 동기화는 매우 느리다.