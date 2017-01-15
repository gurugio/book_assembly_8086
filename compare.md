# compare values

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
