# Jump and function

We usually create a function and call it when we need it. But does the processor know there is a function? Of course, it does not know.

A variable is referenced by name. ``int aaa`` expression in code tells the compiler to allocate a memory space named aaa. The compiler decides memory size, for instance 4-bytes on 32bits machine or 8-bytes on 64bits machine, and then judges where the memory is allocated and reserves a few bytes of memory in the right place. And whenever the compiler encounters the name aaa, it writes or reads the value to that memory location.

The function name is the same as the variable name. The name is actually translated into a memory address. The function name is also a memory address. In C language, a collection of code combined with {} is put together and called as functions.
But what the processor can understand is only the address which is again, number, not function name.
So we stores some instructions at contiguous memory and make the processor read it.

In high-level languages, calling a function and returning from the function is done by compiler. But in assembly, we have to do all things for ourselves.
We have to know how to call a function, pass parameters, get return value and get back to caller.

Let's start with the simple jump.

## Unconditional jump

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

The 8086 has a limited range to jump. You can jump only within 65,535 bytes.

Why? That's because the jump address is specified by ``CS:[label]``.
We learned that the address of value is 20bits.
However, the jump is limited to 16 bits, 65535.
This is because you can not change the value of CS.
So, you can jump only in the range of 16 bits, which is the range of 16bits register values.

Try to change the CS value with mov instruction.
CS value will not be changed directly.
