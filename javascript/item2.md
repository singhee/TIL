#아이템2. 자바스크립트의 부동 소수점 숫자 이해하기

1. 자바스크립트는 단 하나의 숫자형 데이터(number)를 가진다
2. 자바스크립트 내의 모든 숫자는 'double' 형태
3. 자바스크립트의 비트단위 연산자는 인자들을 암묵적으로 32비트 정수로 변환

	```
		 8 | 1  // 9 		
	(double)(double)	 -> 32비트 정수형으로 표현

	00000000000000000000000000001000	// 8
	00000000000000000000000000000001	// 1
	--------------------------------
	00000000000000000000000000001001	// 9
	```

4. 부동 소수점 숫자는 부정확하다	
	> 잘못된 산술 결과를 만들 수 있다
	> 근사 값만을 만들어 낼 수 있고 가장 가까운 표현가능한 실수로 반올림 한다	
	```
	(0.1 + 0.2) + 0.3	// 0.6000000000000001
	0.1 + (0.2 + 0.3)	// 0.6
	```						
	> 정확도를 위해선 가능한 정수 사용 





