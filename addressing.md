# memory addressing

In the previous article, we have seen how to represent the simplest memory address ``ds:[bx]``. This article, let's see what else.
The following table is a collection of all representations of memory address.

```
[BX + SI]
[BX + DI]
[BP + SI]
[BP + DI]	
[SI]
[DI]
d16 (offset of a variable)
[BX]	
[BX + SI + d8]
[BX + DI + d8]
[BP + SI + d8]
[BP + DI + d8]
[SI + d8]
[DI + d8]
[BP + d8]
[BX + d8]	
[BX + SI + d16]
[BX + DI + d16] 
[BP + SI + d16]
[BP + DI + d16]	
[SI + d16]
[DI + d16]
[BP + d16]
[BX + d16]
```

d8 refers to an 8-bit number, and d16 refers to a 16-bit number.

If you look at it carefully, you can see a sequence of the registers. The first register is bx, bp, then si, di and then the number. The full names of bx, bp are "base register" and "base pointer register", so bx, bp always indicate base address. Also, we know that the names of si and di are the "source index register" and the "destination destination index register", so we can think they have the index added to the base address. If you know C language, you might think of something similar. Yes, those are representations of the array. (Note 1)

Let's take an example code.

```
org 100h
mov ax, 0b800h
mov ds, ax
mov bx, 0
mov cl, 'A'
mov ch, 11011111b
mov ds:[bx], cx
mov [bx+2], cx
mov si, 4
mov [bx+si], cx
mov [bx+si+2], cx
ret
```

Before you run it, think about how many times you will see the letter A on the screen while watching the code. How many instructions do you write to memory? There are four because there are four commands with [].

So how many memory addresses are there?

The first command to write to memory is ``mov ds:[bx], cx``. The ds value is 0b800h and the bx value is 0. "Segment address * 10h + register value" is the address value, so 0b8000h + 0h = 0b8000h is the address value. 'A' is written to memory at the address of 0b8000h.

The next address expression is ``[bx + 2]``. The segment ds value is 0b800h and 2 is added to the register value bx, so 0b8000h + 0 + 2 = 0b8002h becomes the address value. It adds 2 to the privious memory address.

The next expression is ``bx + si``, where si valus is 4. So the address is 0b8000h + 0 + 4 = 0b8004h. The next one is ``bx + si + 2``, so it will be 0b8006h.

Run the code now. How many pink A's are there?

Are the pink A sticking together or apart?

Try making a line of A with other forms not in the example. And then move on to the next line. If you continue to add 2 to the memory address, you can fill one line. Change the color and change the letters into different letters. You can change the color with changing the value of 11011111b to any value.

If it works well, try wrong expression. Try placing a 16-bit value in d8. Write other three registers like ```[ax + bx + cx]``` instead of [bx + si] using two registers. In the table, the first register is always bx, bp, si, and di, but try another register like ax or cx. What happens?
---

Note1: If you're C language programmer, you must realize immediately that the expressions are same to pointer operations. Think about ``*(p + 1)`` or ``*(pp + 3) + 4 when p is a pointer and pp is a doube pointer. Please note that the C language is origianally designed for assembly programmer.

