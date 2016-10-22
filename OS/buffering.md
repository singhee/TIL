# 버퍼링 정책
	"버퍼를 언제 비울 것이냐"
	버퍼링의 목적 : read()와 write() 호출의 최소화

1. Line Buffered
	: stdin(키보드), stdout(모니터)
	: `\n` 을 만났을 때 버퍼를 비운다.
	: 입력의 요구될때(`scanf()`...) 마다 버퍼를 비운다.

2. Full Buffered
	: File
	: 버퍼의 공간이 다 채워졌을 때 버퍼를 비운다.
	: `fflush()` 호출 시 버퍼를 비운다.

3. Non Buffered
	: stderr
	: 표준에러로 문자열 출력될 시 버퍼링 되지 않고 바로 출력된다.
