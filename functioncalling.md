# Calling function via stack and local variables

Last chapter described two instructions, call and ret, to just call a function.
Now let's dig in the function in detail.

## call a function with arguments in stack

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

After the function is finished, there is one more thing to do. It is restoring the stack pointer.
We stored arguments in stack.
If we do not restore the stack pointer, it will only grow and go the out of stack memory.
We stored two arguments, so we add 4 to sp.
Now sp value get back to the initial address of stack.

Let's call m2 again.
The return value of the first m1 function is still stored in ax.
So the value of ax and 2 are stored in stack.
The second m2 function will calculate 2*2 and stores 4 in ax register.

We understand when stack is used and where ss, sp and bp registers are used.
If you want, try to make a infinite resursive function and check the status of stack memory.

## local variable of function

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

But above does not work well because ax and bx is changed by the first swap function.
what will happend to the registers if other registers also has data?
The data can be changed by the function.

We know what registers swap function uses.
But we don't know what registers other APIs or library functions use.
Therefore each function should save register values it will change.
Let's make swap function again.
Following swap function save register values it will use at the beginning.
At the end it will restore the saved values such like sp register.
So after function finishes, everything is the same before the function call.

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
