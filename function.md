
#함수 만들기

함수를 어떻게 만드는지 설명하겠습니다. 어셈블리 언어에서는 함수를 프로시저라고 부릅니다. 파스칼같은 옛날 언어들은 함수를 프로시저라고 부르던데 사실 무슨 차이인지는 모르겠습니다. 어셈블리에서는 proc 이라는 키워드를 사용하므로 프로시저라고 말하는게 더 맞는 것이긴 하지만 요즘 시대에 맞지 않으므로 저는 함수라고 부르겠습니다.

##함수 만들기

함수는 이렇게 만듭니다.

 
```
함수이름 PROC

; 함수 내용

ret

함수이름 ENDP
```
 

PROC과 ENDP는 키워드입니다. 실제로 실행되는 명령어가 아니라 어셈블러에게 여기부터 여기까지가 함수의 내용이라고 알려주는 역할을 합니다.

함수이름은 PROC와 ENDP에서 두번 나타납니다. 이름을 다르게쓰면 안됩니다. 이름에 대한 규칙이 있습니다만 어셈블러마다 다르므로 굳이 복잡하게생각하지 않고 영문과 숫자와 _ 기호 정도만 사용하도록 하겠습니다.

ret 명령이 나왔는데 지금까지 역할이 뭔지 모르고 있었지만 지금에서 말씀드리면 함수를 호출한 위치로 되돌아가는 명령입니다. 어떻게 되돌아가는지 세부적인 내용은 스택의 개념을 알아야 하므로 또 다시 완벽한 설명을 뒤로 미룰 수밖에 없습니다. 지금은 함수의 마지막에는 항상 ret을 써야한다는 걸로 넘어가겠습니다.

함수를 호출하는 명령은 call 입니다. 예제를 한번 보시겠습니다.

 
```
ORG    100h
 
CALL   m1
 
MOV    AX, 2
 
RET                   ; return to operating system.
 
m1     PROC
MOV    BX, 5
RET                   ; return to caller.
m1     ENDP
 
END
```
 
m1이라는 함수를 만듭니다. call m1 명령이 함수를 호출하는 명령입니다. m1함수는 bx 에 5를 저장하고 되돌아갑니다. 함수가 끝나면 call 명령의 다음으로 이동하므로 mov ax, 2 명령이 실행됩니다. 그러면 ax에 2가 써지고 ret이 실행됩니다.
주석에는 운영체제로 복귀한다고 써있는데 무슨 뜻이냐면 운영체제가 이 프로그램을 함수 호출처럼 호출했다는 뜻입니다. 운영체제에서 call 명령으로 프로그램을 실행한다는 것입니다. 우리 프로그램의 이름을 어떻게할고 함수처럼 호출했을까요? 또 또 이름이 주소라는 설명을 해야됩니다. 이제 좀 지겹네요. m1이라는 함수 이름도 mov bx, 5 라는 명령이 있는 메모리 주소로 변환됩니다. 우리 프로그램에서는 아마 107h일 것입니다. 그럼 운영체제가 우리 프로그램의 이름을 알 필요가 없다는걸 알 수 있습니다. 대신 call 700h:100h 명령을 실행하면 우리 프로그램이 실행됩니다. 심심하신 분은 예제 프로그램에서 call m1을 call 700h:100h로 바꿔보세요. 무한히 프로그램이 실행되는 것을 볼 수 있습니다. 700h는 cs의 값이고 100h의 ip의 값을 지정하는 것입니다. cs, ip 레지스터의 값은 직접 바꿀 수 없다고 설명했었습니다. 대신에 call 명령으로 바꿀 수 있고 바로 프로세서가 실행해야할 위치를 지정하는 것입니다. 실행 위치또한 메모리의 주소입니다.

마지막은 ret 명령으로 운영체제로 복귀하는 것입니다. 운영체제가 하던 일을 계속하는 것입니다. 요즘 운영체제라면 다른 프로그램을 실행할 것이고 도스같은 운영체제라면 아무것도 안하고 사용자가 다른 프로그램을 실행시키길 기다릴 것입니다. 그건 운영체제가 알아서 할 일이니까 우리가 할 일은 우리 프로그램이 끝났다는 표시만 하면 되는 것입니다.
 
##레지스터로 함수 인자 전달하기

함수에서 빠지면 안되는게 함수 인자입니다. 인자가 없다면 함수가 별로 필요없겠지요.

그런데 함수 인자를 전달하는 방법은 매우 다양합니다. 레지스터에 저장해서 전달하는 방법이 가장 간단합니다. 그런데 인자가 5개면 어떻게 해야할까요? si나 di 레지스터를 써야할까요? 또 함수 호출 전에 각 레지스터에 있던 값들은 어떻게 해야할까요? 인자가 4개라면 ax, bx, cx, dx 레지스터를 모두 써야되는데 기존에 있던 값들이 지워지지 않게 해야되는데 어떻게 해야할까요? 이런 사항들을 고려해서 함수 호출에 대한 규칙을 정한게 Calling convention 호출 규약이라는 것입니다. 언어마다 운영체제마다 다른 호출 규약을 씁니다. 

지금은 간단하게 레지스터에 인자를 저장하고 함수를 호출하겠습니다. 다음 예제는 al, bl 레지스터에 값을 저장하고 함수를 호출하면 함수는 곱셈을 하고 결과 값을 ax 레지스터에 저장해주는 예제입니다.

 
```
ORG    100h
 
MOV    AL, 1
MOV    BL, 2
 
CALL   m2
CALL   m2
CALL   m2
CALL   m2
 
RET                   ; return to operating system.
 
m2     PROC
MUL    BL             ; AX = AL * BL.
RET                   ; return to caller.
m2     ENDP
 
END
``` 

bl 레지스터의 값은 바뀌지 않겠지요. al 레지스터의 값은 2의 제곱, 세제곱, 네제곱이 되서 ax 레지스터에는 2^4의 값이 저장됩니다.
 
##쬐끔 고급 예제



그냥 눈으로만 보셔도 좋은 예제입니다. 화면에 Hello World! 라는 메세지를 출력하는 프로그램입니다. lea, call, cmp, byte ptr, jmp 명령어를 쓰고 함수도 만들고 함수 인자로 si를 사용하고 있는 종합선물세트입니다. int 10h 명령어만 대강 화면에 al 레지스터에 있는 문자를 출력한다고만 생각하시고 읽어보시기 바랍니다.

 
```
ORG    100h
 
LEA    SI, msg        ; load address of msg to SI.
 
CALL   print_me
 
RET                   ; return to operating 
 
system.
 
;==========================================================
; this procedure prints a string, the string should be null
; terminated (have zero in the end),
; the string address should be in SI register:
print_me     PROC
 
next_char:
    CMP  byte ptr [SI], 0    ; check for zero to stop
    JE   stop         ;
 
    MOV  AL, [SI]     ; next get ASCII char.
 
    MOV  AH, 0Eh      ; teletype function number.
    INT  10h          ; using interrupt to print a char in AL.
 
    ADD  SI, 1        ; advance index of string array.
 
    JMP  next_char    ; go back, and type another char.
 
stop:
RET                   ; return to caller.
print_me     ENDP
; ==========================================================
 
msg    DB  'Hello World!', 0   ; null terminated string.
 
END
```