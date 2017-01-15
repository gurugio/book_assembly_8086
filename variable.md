# Variable

The variable is just the address of the memory. But the address of the memory is a number and the number is hard for us to remember. So the assembler and compiler allows you to use variables. We wrote the data at "0b800h:0h" in the previous article. Would not it be easy to make a variable indicate this memory address?

Let's start with the simplest variable.

```
ORG 100h
 
MOV AL, var1
MOV BX, var2
 
RET    ; stops the program.
 
VAR1 DB 7
var2 DW 1234h
```

As I mentioned before, it is not case sensitive. VAR1 and va1 are the same. "DB" can be read as define byte or data byte. It declares a variable with one byte size. "DW" declares a word size variable.
If you run the program with the emulator, you can see that the AL and BX values change to 7 and 1234h, respectively.
Simple. But there is something more important here. I mentioned that the variable is the address of the memory. What are the memory addresses of var1 and var2? Where do we name the address of the memory?

The answer lies in the emulator. The values of the variables we created are 7 and 1234h. If you see 7 and 1234h somewhere in the emulator, that's the location of the variables we created.

![](/assets/2524.png)

The number corresponding to the ret command (AKA, machine code) is C3. The C3 value is stored in memory address 7107h. When the ret command is executed, the program is finished. You can see 07 and 3412 after the ret command.

And on the right side of the emulator screen, it looks like ``mov al, [00108h]``. I mentioned that the address of memory is in []. That is, the address of the variable var1 to be stored in al is ``ds:108h``. You might forget ds and think of the address as 108h. But you should always think of segment registers when calculating the address. Since the value of ds is 700h, the final address value is 7108h. Check the value of address 7108h. Is it 07?

There are the variables we declared. We have declared the variables immediately after the ret command, which is stored immediately after the ret command in the actual program.

The strange thing is that it looks like 3412 while we saved 1234h. This is called a little endian convention. It is not a little Indian ;-), but an endian. Conversely, there is a big endian. Little endian is to store the values ​​written to memory in reverse order by the program. When 16 bits 1234h is written, 34, 12 are stored in the order, and when 32 bits 12345678h are stored, 78, 56, 34, 12 are stored. Since the unit of memory is a byte, it is not stored as 87654321, but it is divided into bytes and its order is changed byte by byte. There are various reasons, but for the details, please refer to Wikipedia.

When we declare a variable between source code, will the program have that variable in place? Let's try following code.

```
ORG 100h
 
MOV AL, var1
VAR1 DB 7
MOV BX, var2
 
RET    ; stops the program.
 
var2 DW 1234h
```

Can you see the 7 next to ``mov al, var1`` on the emulator? What is the address of the var1 that is stored in al register?

![](/assets/2525.png)

Again, the computer does not know what the variable name is. It only know numbers. If you pass the number 103h to the address value, processor will read the memory at 103h and bring it into the register.

So what happens if the variable value itself is an address value? So how can we mimic a pointer variable?

```
ORG 100h
 
MOV word ptr [addr], 1234h
mov bx, addr
mov word ptr [bx], 1234h
 
RET    ; stops the program.
 
addr DW 120h
```

If you look at the source, you have the addr variable that is 120h. That is, we want to store the value 1234h in the ``ds:[120h]`` address. Run the emulator. The ``mov word ptr [addr], 1234h`` command assembled into ``mov word ptr [120h], 1234h`` as we wrote? No, that's why I asked.
Looking at the assembled code, it appears as ``mov word ptr [10fh], 1234h``. Why? As I said above, this is because the variable is a memory location. What is the memory address the addr located at? Find the value 120h. According to the little endian convention, the numbers we should find are 20, 01. It's just at 10fh. As I emphasized already, a variable is not a value but a memory location.
The assembling code for ``mov al, var1`` is not ``mov al, 7`` but ``mov al, byte ptr [var1]``. Think of variables as memory locations. ``mov al, var1`` looks like it would pass the value in var1 into al directly, but in fact it is calculating the memory address of var1 and then reading the data from that address in memory and store the data in the register.

The equality of ``mov al, var1`` and ``mov al, byte ptr [var1]`` may seem strange. However, ``mov al, var1`` is a translation that the assembler provides for human readability, and the syntax understood by the original machine is ``mov al, byte ptr [var1]``.
So how do you store a value in a memory? Save the address value in the bx register as you wrote it in the example source code and access it with the [bx] command.
When the ``mov word ptr [addr], 1234h`` instruction is executed, 1234h is stored in the addr variable. And when ``mov bx, addr`` command is executed, 1234h value is stored in bx register. Finally, we store a value of 1234h in the ``ds:[bx]`` location.

Press the aux button on the emulator to open the memory window and check the memory at address 0700:1234. A value of 34 12 will appear.

In C, writing a value into a memory addressed by a pointer variable is called indirect reference. Why is it indirect? To access the address value stored in the pointer variable, as we did in the assembly language, you must first read the address value stored in the variable into the register. And then you can access memory using the register values.

Normal variables are accessed at once because we know the address of the variable and read the memory once. But for pointer, CPU read the pointer value into a register, and then read the final memory with the address in the register. There is one more memory access so that it's called as indirect reference.

Again. C is an extension of assembly!!
 
----

Note1: ``unsigned short *p = 0xb8000;`` this is a way to store the address value in the variable.
