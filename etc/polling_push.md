# Polling / Push
	하나의 이벤트를 감지하는 방법 (영화감독 <=> 배우)
	
1. `Polling` : 이벤트 발생에 대비해 상태를 주기적으로 확인하는 방식
=> CPU 사용량 많지만, 반응 속도가 빠르다.

2. `Push` : 데이터 변경되었을 때(이벤트가 발생했을 때), 알려주는 방식
=> CPU 사용량이 적다.

## 락(Lock)
	   동시에 두개 이상의 스레드가 접근하지 못하는 것
	   상호배제 (Mutual Exclusion)
	   임계영역 (Critical Section)

  	-> 스핀락(SpinLock) : Polling, CPU 낭비 up, 속도 up  

	-> Mutex : Push, 반응속도down, 성능 저하 

	-> Reader-Writer Lock : 기본적으로 세 가지 상태가 존재한다.(read-lock, write-lock, unlock) 

		- read-lock 은 누군가 자원을 Read 하고 있을 때 걸리는 RW lock 상태이다. 하나의 스레드라도 자원을 읽는다면 자원의 RW lock 은 read-lock 상태가 된다. 이 상태에서 Read 요청이 들어오면 통과시키고, Write 요청이 들어오면 block 시킨다. 일반적으로 write작업이 기아상태에 빠지지 않기 위해, 대기하는 write요청이 있다면 다른 read 요청을 block 시킨다.

		- write-lock은 누군가 자원을 Write 하고 있을 때 걸리는 RW lock 상태이다. write-lock 상태에 있는 RW lock 은 들어오는 모든 자원 요청에 대해 block 시킨다. Write작업은 완료될때 까지의 자원의 일관성을 보장할 수 없기 때문이다. 