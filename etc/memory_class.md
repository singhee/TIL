## Strage Class
1. `.data` : 초기화 된 전역 변수, 정적 변수
	프로그램 시작부터 끝까지 존재하는 메모리
2. `.bss` : 초기화 되지 않은 전역 변수, 정적 변수
3. `.text` : 코드(기계어) 영역
4. `.rodata` : 문자열, 읽기 전용 데이터 (read only)
5. `.stack` : 지역 변수
	함수의 시작과 종료에 존재하는 메모리
6. `.heap` : 동적 메모리 
			직접 할당과 해제에 대한 코드 , 메모리 누수의 문제가 발생할 수 있다.
			프로그래머가 자유롭게 메모리 생성 해제를 할 수 있게 한다.

			malloc, free : c
			new	,	delete : c++ 



```c
void foo()
{
	int a;				// .stack
	static int b;		// .bss
	// auto -> static
	//  : 기억 부류 지정자(storage class)

}

int g;					// .bss
int n = 5;				// .data

static int c;
// extern -> static
//  : external linkage -> internal linkage 
//	: 통용 범위 지정자

int main()				// .text
{
}

```

--------

#### 변수의 메모리 할당과 해제
 - 전역 변수 : 프로그램 시작과 끝
 - 내부 정적 변수 : 함수 처음 호출시 초기화, 더 이상 호출되지 않을 때 끝
 - 지역 변수 : 함수의 시작과 끝
 - 힙 변수 : `malloc()` - `free()`


