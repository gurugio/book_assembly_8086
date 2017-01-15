# how to use mov

Let's summarize the usage of the mov command.
Followings are the common case when reading or writing values to memory or registers.

```
MOV REG, [mem-address]
MOV [mem-address], REG
MOV REG, REG
MOV [mem-address], constant
MOV REG, constant
```

"Reg" is a register, and the "constant" is a number, such as 0b800h.
``Mov dx, [bx]`` reads the 16-bit value from the memory and stores it in the dx register.
But there is one thing to watch out. Can you guess what ``mov [bx], 0ffh`` will do?
Do you want to write 16 bits value of 00ffh at the memory address of bx? Or do you want to write an 8-bit value of 0ffh? Let's run an example to find out.

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
mov [bx+si], cl
mov [bx+si+2], cx
mov [bx], 0ffh
ret
```

Finally, we added ``mov [bx], 0ffh`` to the source we tested in the previous chapter. The address to write 0ffh is 0b8000h. To test this source, run the emulator and then press the aux button at the bottom of the emulator screen. There will be another menu. Select memory from the menu. Then a window called "Random Access Memory" appears as shown below.

![](/assets/2522.png)

Do you see the address value on the left side of the update button? It will be marked as 0700:0100. Click here to clear the address value, enter b800:0000 and press the update button. What happends? It shows what values are stored in the memory location where we want to write the data. Now click "single step". You can see that the values change in memory. And when the ``mov [bx], 0ffh`` command is executed, does the 16 bit value change? Or does the 8-bit value change?

![](/assets/2523.png)

The original value of 41 changed to FF. Only one byte of the value has changed.

In the emulator window, you can see ``mov b.[bx], 0ffh`` in the space where assembly instructions are shown. This means that     ``mov byte ptr [bx], 0ffh`` is abbreviated. The "byte ptr" indicates that only one byte will be written to this address. If you want to write 16 bits, you can use ``word ptr [bx]``.

Then try changing it to ``mov byte ptr [bx], 0ffh``, and convert it to ``mov word ptr [bx], 0ffh``. One byte value will change and two byte values ​​will change.
