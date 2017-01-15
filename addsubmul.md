# ADD, SUB, MUL

There are arithmetic operation: add, sub, mul, imul, div, idiv, inc, dec, neg.

```
org 100h
 
mov ax, 3
mov bx, 5
 
add ax, 1
sub bx, 1
add ax, bx
sub bx, ax
 
ret
```

No need to explain ADD and SUB.
Next are for MUL and IMUL.

``` 
org 100h
 
mov al, 90h
mov bl, 90h
mul bl
 
mov ax, 1000h
mov bx, 1000h
mul bx
 
mov ax, 20
mov bx, -20
imul bx
 
mov ax, 0ffffh
mov bx, 0ffffh
imul bx
 
mov ax, 0ffffh
mov bx, 0ffffh
mul bx
 
 
ret
```

First things first. The size of one register is 16 bits. How many bits will be the result when the we multiply two values in two registers?

16 bits value is multiplied by 16 bits, that will be 32 bits. 2^16 * 2^16 = 2^(16+16) = 2^32. 
What I mean is that when you want to multiply 16bits values, you need 32 bits to store the result. 
Where can we store the result? Where is 32bits register?

The multiply operation uses an additional register that are not visible to the instruction.

First, let's take a look at the command "mul bl".
The bl register is 8bits, so the result should be stored in 16bits register.
The numbers to be multiplied are in al and bl registers.
Yes, the al register is not specified in the command.
But mul instruction uses al for multiplication explicitely.
Where will the result value be stored?
Since 16 bits are required, the entire 16-bit register ax is used to store the result value.
Multiplying 90h and 90h is 5100h in hex.
Check the values in registers after run "mul bl" instruction.
51 and 00 are stored in ah and al, respectively.

In the previous article, ax register was introduced as the accumulate register and used in the calculation mainly,
because ax register is used by mul and div instruction.

Next, the ``mul bx`` command multiplies ax and bx. If you multiply ax and bx, the result will be 32 bits while ax is 16 bits. Where will the other 16 bits be stored? 1000h * 1000h is 1000000h. The upper 16 bits are 100h and the lower 16 bits are 0000h. The upper 16bits value, 100h, is stored in dx register. And lower 16bits value, 0000h, is stored in ax.

The conclusion is, for 8-bit operations, the ax register is used implicitly, and for 16-bit, dx and ax registers are used.

Now let's look at imul. The imul instruction does a multiplication of signed values while the mul performs an multiplication without considering the sign. But imul considers the sign. Again, if the leftmost bit is 1, it is negative. If 0, it is positive.

Let's multiply 20 and -20. There is -20 in the code, but the computer does not know the - sign. Look at how the -20 appears in the emulator. The emulator shows 0ffech. 20 is 14h. 0ffech + 14h is 0, so 0ffech is -20, right?
The result of ``imul bx`` must be negative. And the result must be stored in dx and ax. So what is the dx value? It is 0ffffh. And ax?
It is 0fe70h. So the 32bits result is 0fffffe70h.
The sign bit is 1. So it is a negative number. 400 is 190h in hexadecimal. Add 190h to 0fffffe70h. It's 100000000h.
However, due to the constraint of 32 bits, the most left value should be ignored. So the final result is 0fffffe70h that is -400 in decimal.

Next code is the signed multiplication of 0ffffh and 0ffffh. 0ffffh is -1. If -1 is multiplied by -1, it is 1. Please try it. If the result is 0fffe0001h, it means that you did an unsigned multiplication. If the result is 1, you did a signed multiplication. Please check whether dx value is 0 or 0fffeh.

The final code is the unsigned multiplication of 0ffffh and 0ffffh. Is is (-1 * -1) or (0ffffh * 0ffffh)?
