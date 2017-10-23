# Read/Write memory

We've put the data into a register. But what if you have multiple data? For example, If we need to create an address book or manage credit for hundreds of students, but all data can not handle it with just a few registers. So we have to write and read data in memory.

In a normal programming language, we think of a variable as a name. In C, ``int a;`` Specify a variable with the name a. But processor actually have no idea about variables. A variable is actually a name to specific location in memory. Programmers can remember the memory location because it has a name. In C, you learn the concept of pointers, which is the memory address. In assembly language, data must be available only as pointers, not name. The assembler allows you to create variable names, which is actually a gimmick. It just changes a specific memory address to a variable name.

I will tell you again. The variable is gimmick. The processor does not know the name. It does not know what "a" is. The processor can understand only the address because the address is a number. (Do you understand why the number is the first one? Everything is number !!)

Let's try an exercise. It is a little bit complicated, but it does not matter if you do not understand everything. Just think of code to represent the memory address.


```
org 100h
mov ax, 0b800h
mov ds, ax
mov cl, 'A'
mov ch, 11011111b
mov bx, 15eh
mov ds:[bx], cx
ret
```

Run Notepad, type this code, save it as var.txt, and run emu8086.exe var.txt in the terminal.

Then press the button called screen at the bottom of the emulator window. Then a blank window appears. Then run the program with the single step button. Did you see the pink letter A on the screen?

Let's read the source.

At first there is ``org 100h`` but let's ignore it for now. ``org 100h`` will just be put in the beginning of the program unconditionally. Briefly whenever you run the emulator, ip is set to 100h by ``org 100h``. So the program will be placed at 100h. It does not matter why do we need 100h, not other values for now. You can skip it because this is only for 8086 and it is not related to the processor these days.

It then stores the address 0b800h in ds. You can not immediately store a value of 0b800h directly into a segment register, such as a ds register. This is a hardware limitation of the 8086 processor. It's just that the processor is designed to prevent it from doing so. So we first store the value in a general-purpose register that can be written immediately, and then copy the value of the register to ds.

And ds appears again in ``mov ds:[bx], cx``. Here ``ds:[bx]`` is the representation of memory location. In the previous article, I mentioned that the memory address is represented by a combination of the segment address and the general purpose register. Since ds has a value of 0b800h and bx has a value of 15eh, ``ds:[bx]`` refers to the address 0b8000h + 15eh. And what is the [] for? ``mov bx, cx`` is a command to copy the value of the cx register to the bx register. If you have a memory address in bx and want to issue a command to put cx in the memory location stored in bx, wrap bx with []. So in the end it will be ``mov [bx], cx`` or ``mov ds:[bx], cx``. The ds can be omitted because ds is default segment register. When the processor calculates the memory address, it reads ds if no segment register is specified.(Note 1) Switch to ``mov [bx], cx``. The result is the same.

Then 'A' is written to cl register. This is the character to be printed on the screen. You can use either of '1' or 'b'. The single quotes refer to the ASCII code of the character. If you don't know the ASCII code, just think of 'A' as the letter A itself.

ch has a strange value, which means that the character will be white color on the pink color background.

The bx register indicates a location of the character on the screen. Do you remember the old 386AT computer? It had black and white screen or green screen and it had a CRT monitor. At that time, the screen was not the screen where the graphic is displayed like the window of the present. It was able to print only 80 characters on a line and 25 lines on screen. That's the screen you see right now on the emulator. The emu8086 emulates the screen output of the old days.

When printing one character on the old screen, you have to write 2 bytes. The screen is 160 bytes per line because a single line can output 80 characters. What is 15eh(350 in decimal)? The offsets of characters on first line is 0,2,4...158, the second line is 160,162,164...318, the third line is 320..., so 350 is displayed on the third line.

And where is it printed in the third line? 350 - 320 = 30. And 2 bytes per character. So 30 refers to the 15th position.

It is natural that you do not understand and think it is difficult. It would be comforting to know how many people do not understand the concept of C language pointers and then give up computer science and change their major in electronics / electrical engineering. It is natural if you have never used mathematics in this way. How to printing a character does not matter. If you do not understand, just turn it over. You only need to know ``ds:[bx]``.

What we need to learn here is not how many positions are on the screen, but how to specify a specific location in memory. Again, ``ds:[bx]`` is the expression that ``ds:bx`` is a memory address and not a register value.

The final ret command is the command to end the program.

---

Note1: A regsister to store the memory address. Do not you remember something? Yes, the bx register is just a pointer to the C language. Pointers are just integer values when read. However, if you append an asterisk, it recognizes the variable as a memory address and accesses the memory. The ``*`` operator in C has become [] in the assembly language. To be precise, [] is inherited from C language to ``*``. Do you know that? The C language was created to make a language that is a bit easier for the assembly language programmer, but more powerful and flexible. So after you study assembly language, C language will look different. You will become a human compiler, you will see what assembly instructions correspond to C language code. It's becoming Neo of the Matrix.
