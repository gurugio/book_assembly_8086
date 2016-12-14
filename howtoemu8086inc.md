#emu8086.inc 사용법

emu8086 프로그램이 자체적으로 제공하는 라이브러리가 있습니다. emu8086.inc 입니다. include "emu8086.inc"라는 키워드만 사용하면 라이브러리를 사용할 수 있습니다.

##매크로

제공되는 매크로 몇가지를 소개합니다.

* PUTC char - 한개의 파라미터를 가진 매크로, 현재위치의 아스키 문자를 출력함 
* GOTOXY col, row - 두개의 파라미터를 가진 매크로, 커서위치를 정함 
* PRINT string - 한개의 파라미터를 가진 매크로, 문자열을 출력함 
* PRINT string - 한개의 파라미터를 가진 매크로, 문자열을 출력함, PRINT와 같지만 문자열 끝에 '캐리지 리턴'을 자동으로 추가해줌 
* CURSOROFF - 텍스트 커서 해제 
* CURSORON - 텍스트 커서 시작 

이렇게 사용하면 됩니다.
```
include "emu8086.inc"

ORG    100h

PRINT 'Hello World!'

GOTOXY 10, 5

PUTC 65           ; 65 - is an ASCII code for 'A'
PUTC 'B'

RET               ; return to operating system.
END               ; directive to stop the compiler.
```
 

##함수

제공되는 함수들도 있는데 몇가지를 소개합니다.


* PRINT_STRING - 현재 커서부터 NULL까지 출력하고 DS:SI레지스터에서 문자열의 주소를 받는 프로시져.
 * 사용법 : END 지시어전에 DEFINE_PRINT_STRING 선언
* PTHIS - 현재 커서부터 NULL까지 출력하지만(PRINT_STRING처럼) 스택에서 문자열의 주소를 받는 프로시져. CALL 명령어 바로다음에 ZERO TERMINATED를 정의해야한다. 
 * ex) CALL PTHIS db 'Hello World', 0
 * 사용법 : END지시어전에 DEFINE_PTHIS 선언
* GET_STRING - 유저로부터 문자열을 받고, DX의 버퍼크기만한 DS:DI버퍼로 작성된 문자열은 받는 프로시져. 'Enter'가 입력이 되면 멈춘다.
 * 사용법 : END 지시어전에 DEFINE_GET_STRING 선언
* CLEAR_SCREEN - 화면을 깨끗이 정리하고(스크롤되는 전체화면), 커서를 위에 위치시키는 프로시져.
 * 사용법 : END 지시어 전에 DEFINE_CLEAR_SCREEN 선언
* SCAN_NUM - 키보드로부터 정수를 입력받고, CX레지스터에 결과값을 저장하는 프로시져.
 * 사용법 : END 지시어 전에 DEFINE_SCAN_NUM 선언
* PRINT_NUM - AX레지스터에 있는 정수를 출력하는 프로시져.
 * 사용법 : END 지시어 전에 DEFINE_PRINT_NUM and DEFINE_PRINT_NUM_UNS 선언
* PRINT_NUM_UNS - AX레지스터에 있는 부호없는 정수를 출력하는 프로시져.
 * 사용법 : END지시어 전에 DEFINE_PRINT_NUM_UNS 선언

 

예제를 보시면 쉽게 사용법을 아실 수 있습니다. 함수이므로 call 명령으로 호출해야합니다. 매크로와는 다릅니다.
```
include 'emu8086.inc'

ORG    100h

LEA    SI, msg1       ; ask for the number
CALL   print_string   ;
CALL   scan_num       ; get number in CX.

MOV    AX, CX         ; copy the number to AX.

; print the following string:
CALL   pthis
DB  13, 10, 'You have entered: ', 0

CALL   print_num      ; print number in AX.

RET                   ; return to operating system.

msg1   DB  'Enter the number: ', 0

DEFINE_SCAN_NUM
DEFINE_PRINT_STRING
DEFINE_PRINT_NUM
DEFINE_PRINT_NUM_UNS  ; required for print_num.
DEFINE_PTHIS

END                   ; directive to stop the compiler.
```
 