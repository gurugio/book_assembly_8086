# stack: store temporary data

Stack is memory to temporary data. Yes, only temporary data, not long-time data.
The long-time data should be stored in variable or global memory.

## push/pop instructions

The instructions for stack implementation are push and pop.
The push instruction stores 16bits data into stack, pop read data from stack.
The operand of push can be a register, a memory address or a constant value.

```
push ax

push ds

push [bx]
```

The pop instruction read value in stack memory and store in a register or memory.

```
pop ax

pop ds

pop [bx]
```

How to use push/pop instruction is simple.
It's more important to know what stack is and why it is necessary.

## stack pointer

The stack is memory space. But where is it? Where the temporary data will be stored?
The stack is allocated by OS. When a program is executed by loader, OS/loader decides the size and location of stack memory.

The address of stack is specified by ss (stack segment) and sp (stack pointer) registers.
The address of variable is specified by segment and general register like ``ds:[si]`` or ``ds:[bx]``.
The address of stack is also specified by ``ss:[sp]``.
The ss and sp registers are dedicated for stack memory.
Just before the program starts, OS initialized ss and sp, and then program can access the stack memory.

The push instruction does:
1. subtract 2 from sp register (because data is stored by 16bit)
1. store data at ``ss:[sp]``

The pop does reversely:
1. read data from ``ss:[sp]``
1. add 2 to sp register

We usually store data from low memory. We store data 1000h, 1002h and so on.
On the contrary, whenever you stores data into stack, sp register valus is decreased.
So the stack is growing backward.
For instance, the initial value of sp is 0fffeh, one push command decrease sp into 0fffch and store data into [0fffch].
You can check the stack memory value with stack button of the emulator.

Stack has one more characteristic. The last pushed data is the first poped data.
If you store 1,2,3,4,5, poped data will be 5,4,3,2,1.
You can imagine a stack of dishes.
The lowest dish is the first stacked dish and the last poped dish.
The last pushed dish is the first poped dish.

Let's run following example.

```
ORG    100h
 
 
mov ax, 1234h
push ax
push ax
pop bx
pop dx
 
ret
```

Following is the captur of the emulator.

 ![](/assets/2537.png)

Please run the emulator and press stack. You can see memory values at 700h:0fffeh.
The initial values are 0.
After ``push ax``, 1234h is stored at 0fffch address and 1234h is stored at 0fffah.
The initial value of sp is 0fffeh but no data is stored at 0fffeh.

``pop bx`` command changes sp value from 0fffah into 0fffch and stores 1234h in bx.
``pop dx`` stores value into dx.

The memory data is copied into register and the memory values are remained.
So there was some cracking techniques to use old stack data.
