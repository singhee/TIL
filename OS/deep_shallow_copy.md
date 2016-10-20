# 깊은 복사(deep copy) vs 얕은 복사(shallow copy)

### 깊은 복사 (deep copy)
	1. 데이터 크기가 클 경우, 성능 저하가 있다.
	2. 자원해지 시점 고려가 필요 없다.

### 얕은 복사 (shallow copy)
	1. 데이터 복사 비용이 발생하지 않는다.
	2. 어느 시점에 데이터(자원) 해지를 해야할지 결정하기 힘들다.

	"해결책 : *참조계수에 의한 자원 관리"

#### *참조계수 (reference counting)
	1. 포인터(레퍼런스)가 몇개인지 카운트 한다.
	2. 더 이상 참조되지 않을 때, 파괴한다. (참조계수 = 0)

	-> Linux kernel 의 대부분 자료구조(구조체)등은 참조계수를 이용한다.
	------------------------------------------

	- 참조계수의 증감은 원자적으로 증가/감소 해야 한다.
	  : CPU 에서 제공하는 *Atomic Operations 사용 




-------

- 참고 
[*참조계수 - Reference Counting](https://github.com/singhee/TIL/blob/master/etc/reference_counting.md)
[*Atomic Operations](https://github.com/singhee/TIL/blob/master/OS/atomic_operations.md)