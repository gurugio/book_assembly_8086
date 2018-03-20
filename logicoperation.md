# Logical operations used to distinguish between true and false

Often we have to judge with computer. Whether or not the user clicks the mouse, whether the keyboard is pressed or not, whether the sum of cells in Excel is over 100 or not, and we cannot guess how many decisions are made in one program.

Judgment with computer is processed by logic operation. It is called as logic operation, and it is an operation that judges which operation result is the same as the result I expect. As an example, you want to print 'A' on the screen 10 times repeatedly in a loop, you need to remember how many times you have printed it, and then determine whether it prints 10 times or not. In this case, there is a assembly command to judge whether the number is less than 10. That is the logical operation.

## Flag register

Before logical operation, we should know the flag register first. Flag register is 16bit register to show the result of operation.

When you run the emulator, there is a button called flags on the most right size of the buttons at the bottom. Press this button to show the current flag register status.

![](/assets/cpu.gif)

Do you remember this picture? There are 16 small squares on the right side with Overflow, Direction and so on. This is a flag register.

This register is 16 bits, which has a meaning for each bit. I will explain each bit. Of course, each bit has 1 or 0 value. 1 means that the corresponding state is satisfied and 0 means that it is not.

Carry Flag \(CF\) - The flag value is set to 1 if the unsigned integer operation results overflow. In other words, if you store 255 to al and add 1, the resulting value will be out of the range of 8 bits \(0~255\), and this state is called overflowed. If it is not overflowed, the flag value remains 0.

Zero Flag \(ZF\) - Set to 1 when the result value of a operation is 0. Subtracting registers with the same value, if they are not in the same register, will be 0. If the result is not 0, the flag value is 0.

Sign Flag \(SF\) - 1 if the result is negative. When it is positive, it becomes zero.

Overflow Flag \(OF\) - It becomes 1 when it overflows in operation between signed integer. For example, 100 + 50 is the case \(since the result range is not -128 ... 127\), it is different from Carry Flag.

Parity Flag \(PF\) - Set to 1 if the bit value is an even number, and set to 0 if it is an odd number.

Auxiliary Flag \(AF\) - Set to 1 if the unsigned integer nibble \(4 bits\) is overflow.

Interrupt enable Flag \(IF\) - Set to 1 when the CPU responds to an interrupt by an external device.

Direction Flag \(DF\) - This flag shows the direction of data processing instruction. It is set to 0 when processing proceeds from the memory low address to the high address, and to 1 when the processing proceeds from the memory high address to the low address.

```
org 100h
mov al, 100
mov bl, 50
add al, bl

mov al, 255
mov bl, 1
add al, bl
ret
```

I added 100 and 50. By the way, 100 is in the al register and 50 is in the bl register. Each is an 8-bit register. So it is the addition of 8 bits values and the result is 150. Can you guess which flag will be set, overflow or carry? Since the register is not out of range, there is no carry and there is only overflow. A carry is result of an out of range operation.

As you run the example line by line, check which flag bits change to 1.

## Instruction for logical operation

Let's try the instructions for logical operation: CMP, AND, TEST, OR, XOR, NOT.
The name of the instructions show what it does.
Please run following example.

```
org 100h

mov ax, 1
mov bx, 0
and ax, bx

mov ax, 1
mov bx, 0
or ax, bx

mov ax, 1
mov bx, 0
xor ax, bx

mov ax, 1
mov bx, 0
not ax
not bx

mov ax, 1
test ax, ax
test ax, 1

ret
```

Above example does the operations of "1 and 0", "1 or 0", "1 xor 0", "not 1", "not 0" to show what each operation do.

What is the result of "1 and 0"? The AND operation must be true if the both of operand are true, or false. So 1 and 0 are 0. What you need to look at during the experiment is how the register values and flag ​​change. When ax is 1 and bx is 0, where is the resulting value of "ax AND bx" stored? AX. The value of ax has been changed from 1 to 0. And the zero flag has been changed to 1 because the result of the operation is zero. In addition, overflow and carry do not change. Try replacing and ax, bx with and bx, ax. Where will the results be stored? And what happens to flag register?

The test command is a somewhat unusual command. Same as AND, but the it does not change the value of the register. Only the status register is changed. So TEST does only test. It is to test whether the values ​​are equal or not zero. For example, "TEST ax, ax" tests if AX is zero. If you run "TEST ax, 1" it checks whether the value of ax register is 1 or 0. If the "TEST ax, 1" command is executed and zf is 1, it means the resut of "ax AND 1" is 0 and then ax must be 0.

I will not explain any other commands for interest. Try it yourself.
