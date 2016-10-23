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