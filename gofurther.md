# how to study modern assembly

Some people asked me how to study modern assembly.
They tried too old books that describes how to contol mouse, keyboard and even floppy disk.
Those books were written when MS-DOS was common platform for PC.
At that time, PC devices were controlled directly but it's impossible on the modern OSes.

Let me introduce several steps:
1. On the Linux, write some 64-bit assembly code and system call with nasm. Gas has too different grammar, but nasm is handy because it has similar grammar to emu8086.
1. Create an 80x86 boot loader on an x86 virtual machine such as virtualbox or qemu.
1. Create image processing such as shrinking or enlarging images with multimedia assemblies such as mmx and sse. It is also a popular method for compression and encryption algorithms.
1. There are several books on creating an operating system.

references:
* http://www.drpaulcarter.com/pcasm/ : easy guide to make bootloader with NASM
* http://programminggroundup.blogspot.kr/ : system programming with assembly on Linux
* Greate Code book: guide to understand inside of computer
 
