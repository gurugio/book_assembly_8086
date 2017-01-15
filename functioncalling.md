# Calling function via stack and local variables 함수 호출 규약과 지역변수

이전에는 함수 호출을 위한 call,ret 명령에 대해서만 알아보기위해 개략적인 내용만 설명했습니다. 이제는 함수가 어떻게 호출되는지 제대로 알아보겠습니다. 함수를 호출할 때는 스택을 활용합니다. 스택이 어떻게 사용되는지를 설명하겠습니다.

Last chapter described two instructions, call and ret, to just call a function.
Now let's dig in the function in detail.

## call a function with arguments in stack

함수를 배울 때 실습했던 예제가 있었습니다. 레지스터에 함수 인자를 저장하고 함수를 호출해서 함수내에서 곱셈을 실행하고 결과값을 ax 레지스터로 반환하는 예제입니다. 이 예제를 다음처럼 스택을 이용해서 인자를 전달하도록 바꿔보겠습니다. 사실은 이 예제처럼 스택에 인자를 저장하고 함수 내부에서는 스택 메모리에 접근해서 인자를 읽는 방식이 C의 표준적인 함수 호출 규약중 하나입니다. 이 규약을 잘 지키면 어셈블리 코드에서 libc의 라이브러리 함수를 호출하는 것도 가능합니다.

일단 에물레이터로 실행해보시기 바랍니다.

In last chapter, we tried a simple example to just call a function that get arguments from reigsters and does multiplication, and then return the result in ax.

Let's improve that function. We will pass arguments via stack.
This is a mimic version of calling convention of C language on x86 architecture that specifies what argument is passed which register and stack, and how to return a result.
Following example shows the basic principle of that convention.
So following example helps you to understand the standard calling convention.

```
ORG    100h

MOV    dx, 1
push dx
MOV    dx, 2
push dx
CALL   m2
add sp, 4

push ax
mov dx, 2
push dx
call m2
add sp, 4

RET                   ; return to operating system.
 
m2     PROC

mov bp, sp
mov bx, [bp+2]
mov ax, [bp+4]

MUL    Bx             ; AX = AL * BL.
RET                   ; return to caller.
m2     ENDP
 
END
```
좀 복잡하지만 에물레이터로 한줄한줄 실행해보면 어렵지 않습니다.

가장 먼저 dx에 1을 저장한 후 스택에 저장합니다. 꼭 dx 레지스터를 사용할 필요는 없습니다. 그냥 스택에 1을 저장하기 위해 아무 레지스터나 사용한 것입니다. 그리고 스택에 2를 저장합니다. 이제 함수에 전달할 인자 1과 2가 스택에 저장되었습니다. 인자를 준비했으니 함수 m2를 호출합니다.

에물레이터에서 stack 버튼을 눌러서 스택의 상황을 보고 계신가요? 그럼 call 명령이 호출된 후에 스택에 내가 저장하지 않은 이상한 값이 들어간 것을 볼 수 있습니다. 스택 메모리 0fffch에 1이 저장되고 0fffah에 2가 저장되고 그리고 0fff8h에 10bh 값이 저장되었을 것입니다. 10bh값이 뭔지는 잠시 후에 말씀드리기로 하고 지금은 함수 인자가 스택에 있다는 것만 기억하시기 바랍니다.

이제 함수에서 함수 인자를 읽어야 합니다. 그런데 pop 명령을 사용하면 스택에 있는 10bh 값이 읽혀집니다. 뭔가 역할이 있는 값일테니 pop 명령을 써서 날려버리면 안되겠지요. 그래서 스택을 건드리지않고 메모리에 있는 값만 읽도록 해야합니다. 이럴 때는 주로 bp 레지스터를 씁니다. bp 레지스터는 그냥 변수 주소를 넣을 때도 쓸 수 있지만 사실은 이렇게 스택에 있는 함수의 인자를 읽기 위해서 스택의 포인터의 복사본을 저장하는데 주로 사용됩니다. [bp]로 메모리의 값을 읽으면 10bh 값이 읽어질거니까 그건 건너뜁니다. 그래서 함수의 인자는 [bp+2] 와 [bp+4]가 됩니다.

It looks a little bit complicated but you can understand it as you run one line by one line with the emulator.

First 1 is stored in dx and dx value is stored in stack. You don't need to only dx but any register. What we want to do is just storing 1 in stack.
Next is storing 2 in stack.
Now every argument is stored in stack. It's ready to call m2 function.

Please check the values of stack memory with the emulator.
There are not only 1 and 2 but also a strange value in stack.
The first argument 1 is stored in 0fffch and 2 0fffah.
And 10bh is stored in 0fff8h.
I will explain what it is soon. 
Just remember now that arguments are in stack.

Now the function is being executed.
How can we get the arguments that caller stores in stack?
Yes, we can use pop instruction.
But wait. If we call the pop instruction now, we would get a strange value 10bh.
That is not what we want.
We don't know what 10bh is but it must be used somewhere, so we must keep it in stack.

So we need to read stack memory without changing anything.
The register that we can use to access stack memory is bp register.
The bp register has usually copy of sp register and is used to access function arguments.

At the beginning of m2 function, ``mov bp, sp`` command copies sp into bp.
And [bp+2] is always the last argument and [bp+4] is the second one.
Function itself knows how many argument it has.
So function can access its arguments with [bp+2], [bp+4], ... [bp+(2*#argument-number)].


혹시 굳이 bp를 사용하지 않고 [sp+2], [sp+4]로 읽어도 된다고 생각하지 않으시나요? bp를 사용하는 이유는 지역변수를 이야기할 때 말씀드리겠습니다. 무조건 bp를 사용해서 함수 인자를 읽는다고 생각하셔야 합니다.

함수의 첫번째 인자는 [bp+4]이고 두번째 인자는 [bp+2]입니다. 처음 스택에 넣은 데이터가 좀더 큰 주소에 있습니다. 두개의 인자를 적당히 읽어서 함수가 해야하는 처리를 합니다. 예제에서는 곱셈입니다. 그리고 곱셈의 결과가 ax에 저장됩니다.

이제 함수가 끝납니다. ret 명령이 함수의 끝에 반드시 있어야 된다고 말씀드렸고 ret 명령이 실행되면 call 명령의 다음 명령으로 점프한다고 말씀드렸습니다. 에물레이터에서 ret 명령을 single step으로 실행하고 sp 레지스터의 값을 확인해보겠습니다. 0fff8h였던 sp의 값이 0fffah가 되었습니다. 즉 ret 명령은 스택에서 10bh 값을 꺼내는 일을 합니다. 그리고 ip 값을 보시면 10bh 가 되어있습니다. 10bh는 어디인가요? call m2 다음 명령인 add sp, 4 명령이 있는 위치입니다. 즉 call 명령은 자기 다음의 명령의 주소를 스택에 저장하고 ret 명령은 스택에서 복귀 주소를 꺼내서 실행하는 것입니다. 이렇게 call 명령과 ret 명령이 함수를 호출하고 복귀하는 것입니다. 별로 어렵지 않은 것을 좀 어렵게 설명한 기분이지만 기분탓입니다.

Yes, somebody might think we can use [sp+2] and [sp+4] instead.
When function creates the local variable, sp will be changed and [sp+2] also will be changed.
So bp register is always used to access arguments.

Now function is terminated by ret instruction.
As I said, the last command of the function should be ret instruction.
The ret instruction returns back to caller as the name is.
Let's run the ret instruction with single step button and check sp register.
The sp register was 0fff8h before but now 0ffah.
The ret instruction pop two bytes value 10bh.
And please check the ip register.
Its value is 10bh.
The next command of ``call m2`` is ``add sp, 4`` and its address is 10bh.
Yes, now we understand one more thing about call and ret.
The call stores the address of the next command in stack and the ret pop the address and jump.

그리고 함수가 끝나고 해야할 일은 스택에 있던 인자들을 지워주는 것입니다. 인자들을 지우지 않으면 함수를 호출 할 때마다 스택이 점점 작아지겠지요. pop 을 두번해서 스택을 되돌리는 방법도 있고 예제처럼 sp 레지스터에 4를 더해서 sp의 값을 예전 값으로 되돌리는 방법도 있습니다. 이왕이면 2개보다는 1개 명령이 실행되는게 좋겠지요.

After the function is finished, there is one more thing to do. It is restoring the stack pointer.
We stored arguments in stack.
If we do not restore the stack pointer, it will only grow and go the out of stack memory.
We stored two arguments, so we add 4 to sp.
Now sp value get back to the initial address of stack.


또다시 m2를 호출하겠습니다. 이번에는 이전의 결과값이 ax에 있으므로 ax를 스택에 넣습니다. 그리고 다시 2를 넣습니다. 그리고 m2를 호출하면 ax에는 2*2=4가 저장되겠네요. 마지막으로 다시 스택을 복구시킵니다.

스택은 이렇게 함수 호출에 이용됩니다. 심심하신 분들은 C로 무한 재귀 함수를 만들어보시기 바랍니다. 스택 복구하는 코드가 실행되지 않고 계속 스택을 사용하기만 하므로 스택 영역을 모두 사용해서 세그먼트 폴트가 발생합니다. 스택을 몰랐다면 함수가 무한히 재귀적으로 실행되도 무한히 실행되고 문제가 없을것 같은데 그게 아니라메모리 문제가 발생한다는 것을 알 수 있습니다.

Let's call m2 again.
The return value of the first m1 function is still stored in ax.
So the value of ax and 2 are stored in stack.
The second m2 function will calculate 2*2 and stores 4 in ax register.

We understand when stack is used and where ss, sp and bp registers are used.
If you want, try to make a infinite resursive function and check the status of stack memory.

## local variable of function 함수의 지역 변수

이제 함수 내부에서 인자에 접근하기 위해 왜 sp가 아닌 bp를 사용하는지 말씀드리겠습니다. 바로 함수의 지역 변수를 스택에 만들기 때문입니다.

다음 예제는 인자로 받은 값을 swap해주는 함수입니다. swap함수는 무조건 지역 변수가 하나 있어야 된다는거 아시지요?

I said bp is used to access argument because local variables changes sp register.
Following example shows how to make the local variables.

```
ORG    100h

lea ax, var1 ; push &var1
push ax
lea bx, var2 ; push &var2
push bx
CALL   swap
add sp, 4

ret

var1 dw 11h
var2 dw 22h

 
swap     PROC

mov bp, sp
mov si, [bp+2] ;si = &var2
mov di, [bp+4] ;di = &var1

mov ax, 0 ; temp=[bp-4]
push ax

mov ax, word ptr [si] ; ax = var1
mov bx, word ptr [di] ; bx = var2
mov word ptr [bp-2], ax ; temp = var1
mov word ptr [si], bx ; *&var1 = var2
mov dx, word ptr [bp-2] ; dx = temp
mov word ptr [di], dx ; *&var2=temp

mov sp, bp

RET                   ; return to caller.
swap     ENDP
```

There are two variables, var1 and var2 those are 11h and 22h, respectively.
The swap function does swapping of two variable, so they will be 22h and 11h.
What should we pass to the swap function? Values of variables or pointers of variables?
Of course, we should pass pointers.

Let's look into the swap function.
The sp is copied into bp.
The arguments are the addresses of variables, so [bp+2] and [bp+4] are the addresses of var2 and var1, respectively.
Let's store them into si and di.

Then it stores 0 in stack.
Why?
Actually the value to be stored does not matter.
It just makes a memory space for a local variable.
Have you ever study any low level programming language like C or C++ language?
If so, you might heard that the local variable is stored in stack.
Yes, that example shows how the local variable is created in other languages.

The local variable is only used inside of function.
Therefore we don't need to call memory allocation API, for instance malloc(), to create the local variable.
If we just declare the local variable, compiler adds some code to make space in stack like above example.
We don't need to call memory free API, for instance free(), to free the local variable, because stack is restored after function is finished.
Now you can understand why the local variable has such characteristics.
That is because it is stored in stack.

The value of sp is changed whenever a local variable is created.
So sp should be copied into bp before a local variable is created.
And restoring sp should be done at the last point of the function.
At the last point of the function, stack should have only the return address for ret instruction to read the return address.
That is why ``mov bp, sp`` is the first command of the function and ``mov sp, bp`` is the last.

The value of bp is fixed during the function, so function arguments and local variables are accessible via bp register.
[bp+X] is for argument and [bp-X] is for local variable.
And [bp] is for return address.

The address of var1 variable is in si, so you can read the value of var1 with ``mov ax, word ptr [si]`` command.
The var2 is referenced with di.
There is not any name for the local variable. Why?
Because processor does not need any name, but only address.
The address of the local variable is accessed by [bp-2].

While you read above example, you should be carefule what value each register has.
Sometimes a register has a value, sometimes a register has a address.
Please check comment that I left at each line.

The last command of the function is restoring sp with bp.
Do you remember? The sp is copied into bp at the beginning of the function.

I have one more thing to tell.
If you know C language, you might heard about de-referencing of pointer.
Above example shows what de-referencing of pointer is.

## restoring registers

swap 프로그램에서 만약에 swap을 2번 호출한다면 어떻게 해야할까요?

What will happend if the swap function is called twice like following example?

```
ORG    100h

lea ax, var1 ; push &var1
push ax
lea bx, var2 ; push &var2
push bx
CALL   swap
add sp, 4

lea ax, var1 ; push &var1
push ax
lea bx, var2 ; push &var2
push bx
CALL   swap
add sp, 4

ret
```
이렇게 똑같은 호출을 2번 하면 될까요? 그래도 되긴 합니다. 하지만 ax, bx 레지스터에 값을 설정하는 부분이 중복됩니다. 따라서 중복 부분을 없애면 더 좋겠지요.

It works. But there is duplicated code to setup ax and bx.
Following example removed the duplicated code.
```
ORG    100h

lea bx, var2 ; push &var2
lea ax, var1 ; push &var1

push ax
push bx
CALL   swap
add sp, 4

push ax
push bx
CALL   swap
add sp, 4

ret
```
ax, bx를 설정하고 필요할 때마다 함수 호출을 반복하면 더 좋을것 같습니다. 그런데 막상 실행해보면 안됩니다. 왜냐면 첫번째 swap이 ax, bx 의 값이 바꾸기 때문입니다. 함수들은 항상 레지스터를 쓰기 마련인데 그렇게 되면 함수 호출 이전에 있는 레지스터의 값들이 날아가 버립니다. 따라서 라이브러리 함수일 경우 호출된 다음에 레지스터가 어떻게 바뀌었는지 모르게 될 수도 있고, 내가 만든 함수라도 일일이 함수 호출 전에 레지스터의 값들을 스택에 넣어줘야하는 불편도 있습니다. 

그래서 이런 불편을 없애기 위해 모든 함수는 자기가 사용할 레지스터들을 스택에 보존했다가 함수 종료 직전에 복구하는 규칙이 있습니다.

swap을 다시 만들어보겠습니다. swap에서는 bp, si, di, ax, bx, dx를 사용합니다. 따라서 함수 초기에 이런 레지스터들을 스택에 보존하는 일을 해야합니다.

But above does not work well because ax and bx is changed by the first swap function.
And if other registers has data, what will happend to the registers after function call?


```
swap     PROC

mov bp, sp
mov ax, 0 ; temp=[bp-4]
push ax
push bp
push si
push di
push ax
push bx
push dx

mov si, [bp+2] ;si = &var2
mov di, [bp+4] ;di = &var1
mov ax, word ptr [si] ; ax = var1
mov bx, word ptr [di] ; bx = var2
mov word ptr [bp-2], ax ; temp = var1
mov word ptr [si], bx ; *&var1 = var2
mov dx, word ptr [bp-2] ; dx = temp
mov word ptr [di], dx ; *&var2=temp

pop dx
pop bx
pop ax
pop di
pop si
pop bp
pop ax
mov sp, bp
RET                   ; return to caller.
swap     ENDP
```

이렇게 각 함수들이 자기들이 사용할 레지스터의 값들을 복구시켜주면 함수를 호출하는 입장에서는 함수가 무슨 레지스터를 사용할지 신경쓸 필요가 없으므로 편리합니다.

이렇게 함수 호출할때 인자 전달 방법, 지역 변수 생성과 레지스터의 복구까지가 함수 호출 규약에 정해져있는 것들입니다.
