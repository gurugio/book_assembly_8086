# Macro function

As I described before, macro function is not called, inserted to where it is.
It's same to preprocessor and inline function of C language.

Following is template to declare macro.

```
name    MACRO  [parameters,...] 

            

ENDM
```

MACRO keyword can have parameters like following.

```
MyMacro    MACRO  p1, p2, p3

     MOV AX, p1
     MOV BX, p2
     MOV CX, p3

ENDM

ORG 100h

MyMacro 1, 2, 3

MyMacro 4, 5, DX

RET
```

Above example also shows how to use the macro function.
We don't need call instruction. Just write the name of macro and arguments.
That is because MACRO is keyword for assembler, not instruction of 8086.

Following shows the assembled code of the macro function.

```
MOV AX, 00001h
MOV BX, 00002h
MOV CX, 00003h
MOV AX, 00004h
MOV BX, 00005h
MOV CX, DX
```

If we need to make a label inside the macro function, how we can make label?
Code of macro function is repeated, so label also will be declared again.
For that case, emu8086 has LOCAL keyword as following example.

```
MyMacro2    MACRO
    LOCAL label1, label2

    CMP  AX, 2
    JE label1
    CMP  AX, 3
    JE label2
    label1:
         INC  AX
    label2:
         ADD  AX, 2
ENDM


ORG 100h

MyMacro2

MyMacro2

RET

END
```
