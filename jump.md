# Jump and function

우리는 보통 프로그래밍을 할 때 함수를 만들어서 필요할 때마다 call이라는걸 합니다. 그런데 프로세서는 함수라는게 있다는걸 알까요? 당연히 모르니까 제가 이런 질문을 던졌겠지요.

우리는 변수를 이름으로 가르킵니다. int aaa라는 것은 int 타입이고 aaa라는 이름을 가진 저장소를 만들으라고 컴파일러에게 지시를 하는 문장입니다. 그럼 컴파일러는 int 타입이 4바이트인지 8바이트인지 판단하고 메모리 위치 어디가 적당할지 판단해서 적당한 장소에 몇 바이트정도 메모리를 예약합니다. 그리고 aaa라는 이름을 만날 때마다 그 메모리 위치에 값을 쓰거나 읽습니다.

어디가 적당한 위치인지 크기가 얼마가 적당할지는 프로세서가 32비트 모드인지 64비트 모드인지 운영체제가 뭔지 등등의 환경들을 종합해서 결정합니다. 컴파일러는 그냥 대략적인 위치만 지정하고 링커/로더가 실제 어떤 주소에 할당될지를 결정합니다. 세부적인 내용은 이런 내용이 필요한 분야에서 일하게 될 경우에는 필요하겠지만 일단 취미로 하는 단계에서는 그냥 컴파일러/링커/로더가 이런 일들을 한다고만 아시면 됩니다.

그리고 메모리 예약이라는 것도 좀 생각해볼만 합니다. 예약이라는게 뭘까요. 예약이라는 말은 내꺼라고 선언하고 남들은 못가지게 만드는 것입니다. 컴파일러는 메모리를 어떻게 예약할까요? 어딘가에 메모리 몇번지부터 몇번지까지는 누구꺼 이렇게 기록할까요? 기억이 나실지 모르겠지만 변수를 이야기할때 어셈블러는 변수를 선언한 그 위치를 그대로 변수 이름과 바꿔치기한다고 했습니다. 변수를 org 1000h 바로 다음에 선언했다면 변수 이름은 [1000h]로 치환됩니다.

We usually create a function and call it when we need it. But does the processor know there is a function? Of course, it does not know.

A variable is referenced by name. ``int aaa`` expression in code tells the compiler to allocate a memory space named aaa. The compiler decides memory size, for instance 4-bytes on 32bits machine or 8-bytes on 64bits machine, and then judges where the memory is allocated and reserves a few bytes of memory in the right place. And whenever the compiler encounters the name aaa, it writes or reads the value to that memory location.


함수 이름이라는 것도 변수 이름과 마찬가지 입니다. 이름이라는게 사실은 메모리 주소라는것입니다. 함수 이름도 메모리 주소입니다. {}같은 특별한 표시로 묶어놓은 코드 모음을 함수라고 이름짓고 사용하지만 사실 기계가 이해하는 것은 어떤 표시도 아닌 주소입니다. 단지 그 주소에 내가 하려는 일을 위한 명령들이 있는 것이지요. 사람을 이름으로 부르지않고 주민번호로 부르는것 같은 차이일까요?

고급언어에서도 함수를 어셈블리로 변환할때 프로세서나 운영체제에 따라서 어떤 함수를 불러야할지 함수가 끝나면 어떻게 되돌아가야할지, 인자는 어떻게 전달해야하는지 등등 세부적으로 결정해야할 사항들이 매우 많습니다. 마찬가지로 이런 일들을 컴파일러가 하는 것이지요.

그럼 가장 간단한 점프부터 이야기해보겠습니다.

The function name is the same as the variable name. The name is actually translated into a memory address. The function name is also a memory address. In C language, a collection of code combined with {} is put together and called as functions.
But what the processor can understand is only the address which is again, number, not function name.
So we stores some instructions at contiguous memory and make the processor read it.

In high-level languages, calling a function and returning from the function is done by compiler. But in assembly, we have to do all things for ourselves.
We have to know how to call a function, pass parameters, get return value and get back to caller.

Let's start with the simple jump.

## Unconditional jump

그냥 어딘가로 점프하는 것입니다. 당연히 그 어딘가는 주소 값이겠지요. 프로세서는 숫자만 인식하고 주소가 숫자니까요. 그런데 문제가 있습니다. 수십바이트부터 수백 수천 바이트 그 이상일때가 많은 프로그램에서 어떤 지점으로 점프하고 싶은데 그 주소를 알려면 어떻게 해야할까요? 명령어 갯수를 세야할지도 모릅니다. 이런 불편함을 어셈블러가 해소해줍니다. 어셈블러가 단순히 영여로된 명령어를 숫자로 바꾸는 일만 하는게 아닌 것이지요.

여기서 label이라는 개념이 나옵니다. 메모리의 특정 지점에 이름을 붙이는 것입니다.

That is just jumping to somewhere. Of course, it's somewhere specified by address. Again, the processor recognizes numbers only and address is number. But there is a problem. If there are many instructions and we want to jump to 100th instruction, how can we get the address of the instruction?
That job is just summation of size of 1st~99th instructions. We don't need to do such a simple repeating job.
Assembler does check the address of each instruction.
What we should do is just placing label.

Here comes the concept of label. The label gives a name to a specific point in memory.
 
```
org 100h
mov ax, 0
mov bx, 0
start:
jmp loop
inc ax
loop:
inc bx
jmp start
ret
``` 

jmp 명령의 사용법과 label을 어떻게 사용하는지 아시겠지요? 이름: 이렇게 쓰면 어셈블러가 해당 위치의 주소를 기억했다가 jmp 등에서 이름을 주소로 바꿔줍니다.

C언어에서 goto 명령이 jmp 명령을 그대로 표현한 것입니다. 예전에는 goto 명령을 절대 쓰면 안된다고 가르쳤었는데 요즘 트렌드는 에러 처리를 위해 goto 명령을 쓰라고 합니다. 프로세서가 발전해서 예전에는 goto 명령이 성능을 떨어지게했지만 요즘에는 goto 명령을 많이 써도 성능 저하가 없어서 그렇습니다. goto를 잘쓰면 코드 이해하기도 쉽고 에러 처리를 한곳에 모아놓아서 좀더 에러에 잘 대처할 수 있게됩니다.

예제를 하나 더 보겠습니다.

Can you understand how to use jmp instruction and label? Just place ``label-name:`` before target instruction you want to jump. The assembler will remember the address of the label, and change the name in jmp instruction into the address.

In C, the goto command is compiled to jmp instuction. 
In the past, the goto command caused performance degradation, but nowadays there is no performance penalty even with a lot of goto commands. If you write goto well, you can make the code better readable and put error handling in one place so that you can deal with errors better.

Let me give you another example.


```
org    100h

mov    ax, 5          ; set ax to 5. 
mov    bx, 2          ; set bx to 2. 

jmp    calc           ; go to 'calc'. 

back:  jmp stop       ; go to 'stop'. 

calc:
add    ax, bx         ; add bx to ax. 
jmp    back           ; go 'back'. 

stop:

ret                   ; return to operating system. 
``` 

8086에서는 점프할 수 있는 범위에 제한이 있습니다. 65,535바이트 이내로만 점프할 수 있습니다.

왜 그러냐면 점프하는 주소가 CS:[label] 이기 때문입니다. 변수에서 ds, es를 이용해서 주소를 20비트로 확장하는걸 배웠습니다. 그런데 점프는 16비트 65535까지만 됩니다. 왜냐면 CS의 값을 바꿀 수 없기 때문입니다. 그래서 그냥 레지스터 값의 범위인 16비트 범위로만 점프가 가능합니다.

CS값을 한번 바꿔보세요. 되나요? 어떤 에러가 나나요? 요즘 환경에서는 필요없는 개념이라 더는 설명하지 않겠습니다. 

The 8086 has a limited range to jump. You can jump only within 65,535 bytes.

Why? That's because the jump address is specified by ``CS:[label]``.
We learned that the address of value is 20bits.
However, the jump is limited to 16 bits, 65535.
This is because you can not change the value of CS.
So, you can jump only in the range of 16 bits, which is the range of 16bits register values.

Try to change the CS value with mov instruction.
CS value will not be changed directly.
