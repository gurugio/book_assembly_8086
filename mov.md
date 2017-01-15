# The first instruction: mov

Let's start the first exercise. It's ok if you do not understanding anything about hexadecimal, binary and 8086 structures discribed in privious chapters. The first thing to learn is always that you know what you do not know. Now you know that "I do not know about hexadecimal, binary, 8086 structure". So it is a step forward.

In this chapter I'll describe the mov instruction. It is the basis instruction of all 8086 instructions and very simple.

Let's run the mov instruction with the emu8086. If you have the old version of emu8086, you might have the emu8086.exe file and cmdhere.cmd file. Please run the cmdhere.cmd file. A terminal window appears. Maybe some of you have seen the terminal for the first time. Do not afraid and type notepad with keyboard. And hit enter to run Notepad. In Notepad, type the following assembly instructions.
If you have another version of the emu8086 or another emulator, it must have the same feature. Please run your emulator and run the following code.

```
mov ax, 1000
mov bx, 1000h
mov cx, 0ffffh
```

And save the file. You can use any file name, for instance mov.txt. Then type emu8086.exe mov.txt on the terminal window. Then emu8086 runs and our sample source appears in a text editor of emu8086. Just run the emulate menu.

I will not capture the screen deliberately. Try it for yourself. It could be a little different for each version. On the emulator screen of the old version, register values ​​appear on the left, and memory values ​​and assembly instructions in memory appear on the right. Is there a "mov ax, 1000" we entered int the instruction list? Probably not. Is there "mov bx, 1000h"? There is. What's the difference?

In the previous article I mentioned that the processor only knows the binary number and that the assembly instructions only use hexadecimal numbers to make the binary numbers easier to see. Right now we can see that the decimal numbers we entered are assembled into hexadecimal and run by the processor. The decimal number 1000 is 3e8h in hexadecimal. So, the command screen shows "mov ax, 3e8h". Since 1000h is a hexadecimal number, "mov bx, 1000h" are displayed on the command screen. 0ffffh is the same.

Then press the single step button. Does the register value change? Please also press the step back button. And press any other button to see how it works.

Can you feel what mov instruction does? The command to write the value directly to the register is mov.

We wrote the data in the register. Please try something else. Please find how we can move the value of a register to another register.

Also, please write the values ​​to special registers such as CS, IP, SS, SP other than general purpose registers AX, BX, CX and DX. Is it possible to move values ​​between special registers and general registers?

