# compare values

수를 비교하는 cmp 명령을 설명하겠습니다. 너무너무너무 많이 사용되는 명령이라 아무리 강조해도 지나치지 않은 명령입니다. 그리고 다시한번 말씀드리지만 동작하는 방식은 빼기입니다. 단지 뺄셈을 해서 결과를 저장하는게 아니라 플래그 비트만 바꾼다는게 차이입니다. 

5-2 연산을 하면 zf가 0이 되겠지요. 따라서 cmp 5, 2를 하면 zf가 0이 됩니다. cmp 7, 7을 하면 zf가 1이 되는 것입니다.

좀 복잡한 예제를 실행해보겠씁니다. emu8086.inc라는 파일을 include한다는게 뭔가 C 코드같은 느낌이 납니다. 별거는 아니고 putc라는 매크로 함수를 미리 코딩해놓은 파일입니다. putc외에도 다양한 매크로 함수들이 있습니다만 지금은 볼 필요가 없습니다. 지금은 putc가 문자를 화면에 출력하는 일을 한다는 것만 알면 됩니다. 에물레이터를 실행한 다음 screen 버튼을 눌러서 어떤 문자가 출력되는지 확인하시기 바랍니다.

The cmp instruction do comparation of values.
The internal operation of cmp is subtraction.
You need to subtract one value from another value to compare the values.
I don't know any other way to compare the values.

You should use sub instruction to store the result.
But if you want to just compare the values without saving the result, you should use cmp instruction.
It does not change any register except flag register.

If you do "5-2", zf flag will be 0.
So ``cmp 5, 2`` command clears zf flag while ``cmp 7, 7`` sets zf.

Let's try following example.
There is ``include "emu8086.inc"`` that looks like ``#include <header.h>`` in C language.
Yes, it's the same. It includes emu8086.inc file for us to use putc macro-function.
There are more macro-functions. You can open the file and check.

The putc prints one character on screen.
So you need to click screen button to check what character is printed.

```
include "emu8086.inc"
 
org    100h
 
mov    al, 25     ; set al to 25. 
mov    bl, 10     ; set bl to 10. 
 
cmp    al, bl     ; compare al - bl. 
 
je     equal      ; jump if al = bl (zf = 1). 
 
putc   'n'        ; if it gets here, then al <> bl, 
jmp    stop       ; so print 'n', and jump to stop. 
 
equal:            ; if gets here, 
putc   'y'        ; then al = bl, so print 'y'. 
 
stop:
 
ret               ; gets here no matter what.
```
 
첫 cmp 명령은 25와 10을 비교합니다. 당연히 zf는 0이고 je 명령을 만나도 점프를 하지 않겠지요. 그리고 putc 'n' 매크로가 실행되서 화면에 n이 출력됩니다. 에물레이터에서 single step으로 실행하면서 어셈블된 명령어들을 보면 push, pop, int 같은 아직 모르는 명령어들이 나오는데 그냥 넘어갑시다. putc 매크로 함수가 어셈블된 것입니다.
화면에 n이 출력된 후 stop으로 점프합니다. 그리고 프로그램이 끝납니다. 만약 25를 10으로 바꾸면 y가 출력되겠지요.
참고로 매크로 함수와 그냥 함수의 차이를 말씀드리면 매크로라는 것은 실행 코드가 호출된 위치에 들어간다는 것입니다. putc를 호출한 곳에 putc의 코드가 들어갑니다. putc를 호출하는게 아닙니다. 변수나 label처럼 이름을 만나면 해당되는 코드로 바꿔치기를 하는 것입니다. 일반 함수는 함수를 호출하는 코드가 들어갑니다. 그리고 다시 호출 명령으로 복귀합니다. 매크로가 많아지면 코드가 길어지겠지요. 함수를 아무리 많이 호출해도 코드가 길어지지 않습니다. 왔다갔다만 열심히 하니까요. 매크로는 왔다갔다를 하지 않습니다. 왔다갔다할 시간을 아낄 수 있지요. 그래서 좀더 빠릅니다. 코드의 길이와 실행 속도의 trade off가 있습니다. 보통 짧은 코드는 매크로로 만들고 긴 코드는 함수로 만듭니다. 어느 언어나 마찬가지입니다.
 
 The first cmp command compares 25 and 10.
 Of course, zf is 0 and ``je equal`` do nothing and ``putc 'n'`` prints 'n' on screen.
 If you run the code one by one with "single step" button, you could see push, pop and int instruction that are not described yet. Let's just ignore them.
They are just code of putc macro-function.

After printing 'n' on screen, it jumps to stop. Then program is terminated.
If you change 25 into 10, it prints 'y' on screen.

Note that the difference between a macro function and a normal function is that a macro is placed in the location where the macro-function is called.
The code of putc will be added into the place it's called.
So putc is not actually called, it just exists there.

When the normal function is called, the processor jumps to the function and returns to next instruction of call instruction.
We will use call instruction soon.
The more macros you have, the longer your code will be.
But no matter how many functions you call, the code will not be long.
Macros do not go back and forth. I can save time to go back and forth. So it's faster. But it's bigger.
Function calling makes program slow but keeps size.

There is a trade off between code length and execution speed. Usually short code is made as a macro and long code as a function. It is the same in any language.

Recent processors have less overhead for call instruction.
So it'd better use the normal function than macro because small program can use CPU-cache efficiently.
