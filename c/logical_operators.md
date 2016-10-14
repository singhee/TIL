# 논리 연산자 (Logical Operator)

## `&` , `|`   - 비트 논리 연산자 
	 1. &&, || 는 우선순위가 다르다
	 2. 우선순위는 계산의 순서를 의미하는 것이 아니다.
	 3. 우선순위는 결합의 순서를 의미하는 것이다.
	 4. 논리 연산자에 부작용 있는 연산을 섞으면 좋지 않다.

	 &&, ||  - 논리 연산자       -> 1(Non-zero), 0 
	 단회로 특성 - short circuit

	 && - 첫 번째 평가가 거짓이면, 두 번째는 실행 하지 않는다.
	 || - 첫 번째 평가가 참이면, 두 번째는 실행하지 않는다.

``` c
#include <stdio.h>
int main()
{
	int a = 1, b = 0;
	int r = --a && ++b;
	printf("%d %d %d\n", a, b, r); // 0 1 0
}
```
```c

int main(){
	int a = 0,b = 0,c = 0,r;
	r = a || b++ && ++c;
	printf("%d %d %d %d\n", a,b,c,r);// 0 1 0 0 

	a = b = c = d = 0;
	r = a && b++ || ++c && ++d; // 0 0 1 1 1 
	printf("%d %d %d %d %d\n", a,b,c,d,r);// 0 1 0 0 

	return 0;
}

// 비트 연산자
// &(AND), |(OR), ^(XOR), ~(NOT) - 비트 논리 연산자
// >>, << - 비트 시프트 연산자
```


```c
int main(){
	char n1 = 2;
	//       0000 0010 
	char n2 = 48; // 32 + 16 (2^5 + 2^4)
	//  	 0001 1000

	printf("%d\n", n1 & n2); // 

	//  0000 0010
	//  0001 1000
	-------------
	//  0000 0000 (AND) - 0
	//  0001 1010 (OR)  - 32 + 16 + 2 = 50
	//  0001 1010 (XOR) - 32 + 16 + 2 = 50
}

```
## Shift (>> , <<)

 `<<` 연산
 - 뒤에 0을 채우면 된다

 `>>` 연산 (JAVA - `>>`: arithmetic shift, `>>>` : logical shift)
 - 타입(unsigned, signed)에 따라서 다르게 동작한다
   `signed >>` 연산 - arithmetic shift
   : 부호비트가 채워진다.
 `unsigned >>` 연산 - logical shift
   : 0이 채워진다.

```
 64 + 32 + 4
 100 - 0110 0100
		 1000 0000 &   -> 1 << 7
		  100 0000 &   -> 1 << 6
 		   10 0000 &   -> 1 << 5

``` 


### 1 byte 에 대해 bit를 출력하는 함수

``` c
void print3(char value){
	
}

void print2(char value){
	unsigned char x = 1 << 7; //********
	while(x){
		if(value & x)
			putchar('1');
		else
			putchar('0');
		x >>= 1;
	}
	puchar('\n');
}

void print(char value)
{
	 int i;
	 char x = 1 ;
	 for(i = 7; i>= 0; --i){
	 	if(value & (x << i))
	 		putchar('1');
	 	else
	 		putchar('0');
	 }
	 printf("\n");
}

int main(){
	print(100);
}

```

--------


``` c
int main()
{
	char n = 1; // 0000 0001
			 //*2
	n = n << 1; // 0000 0010  // 2를 곱한 효과

	n = n << 2; // 0000 1000

	for(int i =0; i < 10; i *= 2)
	for(int i =0; i < 10; i <<= 1)

	n= 100; 	// 64 + 32 + 4  
				// 2^6 + 2^5 + 2^2
				// 0110 0100
	n = n >> 1;	// 0011 0010 : 32 + 16 + 2 = 50 // 2로 나눈 효과

	n = -1;  	// 1111 1111
	n = n >> 1; // 1111 1111
}
```




