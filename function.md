# function


Let's make a function.

Following is a template to define a function.
 
```
<function-name> PROC

; function body

ret

<function-name> ENDP
```

PROC and ENDP is keyword for assembler that is not instruction.
They just indicate assembler a starting point and a last point of the function.
The name of function is specified twice at PROC and ENDP. We can use normal alphabet and under-tar(_) for the name.

The ret is instruction that is already used many times before but not described yet.
Now let me describe what it does.
It should be used at the end of a function.
It make the CPU return where the function is called.

The instruction to call function is call.
So ret instruction returns the processor to the next instruction of call instruction.

Let's try an example.

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

Above example creates a function: m1.
The m1 function is called with ``call m1`` command.
The m1 function stores 5 in bx register and returns.
The ret instruction returns to a instruction next of call, so ``mov ax, 2`` command is called.
And then ret is called and the program is terminated.
The comment of ret instruction says that it returns to OS by ret instruction.
That is because OS execute the program.

I commented that we cannot change cs and ip registers.
But there is a exception. The call instruction can change cs and ip registers.

Try to replace ``call m1`` with ``call 700h:100h``.
700h is for cs and 100h is for ip.
What happens?
Try other values and check where it jumps.

 
## function arguments

The call instruction does just jump to function code.
There is no instruction to pass arguments.

How can we pass arguments?
There could be many ways.
I think storing arguments in registers is the easiest way.
We can store arguments in registers ax, bx, cx, dx.
But what if the function has 5 arguments? what if 6 arguments?

And there is more critical problem.
Before we store arguments in registers, there should be valid data in registers.
How can we save the data in registers?

Therefore there should be a standard to pass arguments that is called as Calling Convention.
There are various calling conventions for each language, compiler, processor architecture and so on.
You can find so many calling conventions here: https://en.wikipedia.org/wiki/Calling_convention
We will try one of the standard calling conventions next chapter.
 
Below example shows a non-standard calling convention that passes arguments in al and bl registers.

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

## advanced example

Following example is a little bit complicated example to print "Hello World!" on screen.
It uses lea, call, cmp, jmp instructions and "byte ptr" directive.
``int 10h`` command is just printing a character in al register.
Take your time and read code carefully.
 
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
