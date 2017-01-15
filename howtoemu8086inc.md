# emu8086.inc

emu8086 provides some libraries that have macro functions for IO, mathmatic operaion and so on.
emu8086.inc is the most common file.
You can use the library if you write ``include "emu8086.inc`` at the first line of source file.

## macro function

Let me introduce some macro functions.

* PUTC char - print one character
* GOTOXY col, row - set cursor position
* PRINT string - print a string 
* CURSOROFF - turn off text cursor 
* CURSORON - turn on text curosr 

Following is an example.

```
include "emu8086.inc"

ORG    100h

PRINT 'Hello World!'

GOTOXY 10, 5

PUTC 65           ; 65 - is an ASCII code for 'A'
PUTC 'B'

RET               ; return to operating system.
END               ; directive to stop the compiler.
```
 

## library function

Some library functions.

* PRINT_STRING - print a string that addressed by DS:SI register
 * usage: write DEFINE_PRINT_STRING before END keyword
* PTHIS - get a address from stack and print a string
 * how to call: CALL PTHIS db 'Hello World', 0
 * usage: write DEFINE_PTHIS before END keyword
* CLEAR_SCREEN - clear entire screen
 * usage: write DEFINE_CLEAR_SCREEN before END keyword
* SCAN_NUM - get number from keybord and store the number in cx
 * usage : DEFINE_SCAN_NUM before END keyword
* PRINT_NUM - print a decimal number in AX
 * usage : write DEFINE_PRINT_NUM and DEFINE_PRINT_NUM_UNS before END keyword
* PRINT_NUM_UNS - print a signed decimal number in AX
 * usage : DEFINE_PRINT_NUM_UNS before END keyword

Following shows how to use the functions.
They are functions, so you should call them with call instruction.
```
include 'emu8086.inc'

ORG    100h

LEA    SI, msg1       ; ask for the number
CALL   print_string   ;
CALL   scan_num       ; get number in CX.

MOV    AX, CX         ; copy the number to AX.

; print the following string:
CALL   pthis
DB  13, 10, 'You have entered: ', 0

CALL   print_num      ; print number in AX.

RET                   ; return to operating system.

msg1   DB  'Enter the number: ', 0

DEFINE_SCAN_NUM
DEFINE_PRINT_STRING
DEFINE_PRINT_NUM
DEFINE_PRINT_NUM_UNS  ; required for print_num.
DEFINE_PTHIS

END                   ; directive to stop the compiler.
```
 
