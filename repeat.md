# loop

I am wondering if there are any of you who have fun so far.
If so, think about C code or Java code for a while. You can look at any code.
How do you see things like creating a variable, calling a function, or creating a loop?
Do you feel like you're seeing some assembly code inside of it?
I can tell you that if you study a little bit of assembly, you can make better code regardless of type of language.
"better code" I mention here is not readable codes good for people, but fast codes good for machines.
You'd better study the language itself to write more readable and elegance code.
But learning assembly language helps you to write more fast code.

Now let's look into repeating.

There is a loop instruction to repeat something.
Actually it's the combination of cmp and jump instructions.
It needs counter to check how many times it has repeated.
The loop instruction check the counter automatically, so we can skip comparing the counter.
If we stores the number to repeat in cx register, loop instruction decrease cx at each loop and terminates repeating when cx is zero.

``` 
include "emu8086.inc"
 
org    100h
 
mov al, 0
mov bl, 10
 
x_loop:
cmp    al, bl
je     equal
putc 'x'
inc al
jmp x_loop
 
equal:
mov cx, 10
y_loop:
putc   'y'
loop y_loop
 
ret
```

Above example shows two kinds of implementations of looping.
First looping uses cmp and jmp instruction, second uses loop instruction.
You might realize soon what the difference is and why loop instruction is necessary.

First is the length of program. The loop instruction can decrease the length of program.
Long time ago, many machines had only 1MB memory and no hardware cache.
Shorter program was better.
Shorter program is also efficient recently because hardware cache is always not enough.

Next is less usage of register. For cmp and jmp instruction we need two registers to store 0 and 10 in above example.
But we need only one register if we use loop instruction.
You can wonder why it is so important to save one register.
But you should consider the 8086 has only 6 general registers: ax, bx, cx, dx, si, di.
Even the latest x86 architecture has only 10~20 registers.

Accessing register is faster than memory several millions times.
If a processor has one more register, it can save so many memory accessings and it directly increase program performance.
