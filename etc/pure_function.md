# 순수 함수(Pure Function)
	부수효과(side effect)가 없는 함수.
	주어진 입력으로 계산하는 것 이외에 프로그램의 실행에 영향을 미치지 않아야 한다. (다른 기능은 하지 않아야 한다) 

"참조에 투명하다" - 입력 값이 같으면 결과 값도 같다. 
---> 참조 투명성 (referential transparey, RT)

## 참조 투명성 (referential transparency, RT)
	모든 프로그램에 대해 어떤 표현식(expression) 을 모두 그 표현식의 결과로 치환해도 프로그램에 아무 영향이 없다면 그 표현식은 참조에 투명하다(referentially transparent) 라고 한다.
	
	함수가 참조에 투명하다면, 그 함수는 순수함수이다.
