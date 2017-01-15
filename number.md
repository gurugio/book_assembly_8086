# The principle of the computer world: number

The number was born first.

The number is older than the assembly. As Pythagoras said, the number is the principle of all things. The principle of the computer is naturally the number. The computer was invented by mathmaticians.
We will understand how the computer was related to the number in near future. First you must know the binary number and hexadecimal number. When you write assembly code you should use the binary and hexadicimal. And every number emu8086 shows is the hexadecimal.

## Decimal number

Unfortunitely we use decimal number system. We use the number from 0 to 9 when we write a number. And you sometimes write one more numbers serially to write big number. For the big number, each digit means the numerical index.

For example, let us think about 754.

``7*10^2 + 5*10^1 + 4*10^0``

7 is multiplied by square of 10 and 5 is multiplied by 10. 4 is multiplied by 10 to the power of 0. 10 to the power of 0 is 1.
If the position is changed the number would be absolutely different. 574 is not 754.

## Hexadecimal

The processor is made of materials with two states, 1 and 0. If there is an electronic signal, the state of the material become "1" or 0.(Note 1) So computer cannot count 10 numbers from 0 to 9. So the computer use binary numbers.
Binary number is a numbering system that has two numbers 0 and 1 while the decimal number has ten digits.
To distinguish between binary and decimal numbers, we usually write b after the binary number. Because the binary number is "binary" in English.
What is the binary number 10100101b in decimal number?
The decimal number 754 has a power of 10 for each digit. Likewise, a binary number can be raised to the power of two for each digit. The rightmost is 0 of 2, the left of 1 is 2, and so on.

``10100101b = 1*2^0 + 1*2^2 + 1*2^5 + 1*2^7 = 1 + 4 + 32 + 128 = 5 + 160 = 165``

Conversely, how do you express 165 as a binary number? As you learned in school, there are ways to list the rest after dividing by 2. Rather, I like the way calculating the power of 2 which is the closest to 165.
I find 165 = 128 + 37 first because 128 is 2 ^ 7. As I studied assembly language, I was able to memorize several numbers which are the square of 2.
After 165 next is 37 = 32 + 5. 32 is 2 ^ 5. So 5 is 4 + 1, so it is 2 ^ 2 + 1. The conclusion is 165 = 2 ^ 7 + 2 ^ 5 + 2 ^ 2 + 2 ^ 0. The way to convert this to binary is to add zero to the power of the square. 2 ^ 7 is 10000000b since there are seven zeros. 2 ^ 5 is 100000 and 2 ^ 2 is 100 because there are five zeros. Add everything to 10100101b.
It does not matter whether you calculate for yourself or use a calculator. Of course, the calculator is the best.
Note that in the 8086 assembly language, each digit of a binary number is called a bit. 4 bits are one nibble and 8 bits are one byte. And 2 bytes are one word and 2 words are called as double word. The rightmost bit is called as the Least Significant Bit (LSB), and the leftmost bit is called as the Most Significant Bit (MSB).

## Hexadecimal number

In the computer industry, hexadecimal numbers are common. Why? This is because binary numbers can be easily converted into hexadecimal and vise versa. When representing a 32-bit address you need 32 binary digits in binary. Since 1024 is 2 ^ 10, you should use ten zeros to write 10000000000b. It is very inconvenient. It is also very easy to make mistakes when typing code because there is a high risk of misplacing the number of zeros.
However, to write a decimal number, you have to calculate the number of digits of each digit of the binary number. Multiply by 2 and add 1 and ... it is too difficult. So hexadecimal number is picked. It is relatively simple because you can think of hexadecimal by breaking four binary digits.
If you cut four digits from 10100101b, it is 1010 and 0101. 1010b is 10 in decimal and 0101 is 5. Is there a 10 in hexadecimal? no. In hexadecimal, 10 is A. Is there more numbers bigger than 9? No. Since there are only 10 numbers, we borrow the alphabets a, b, c, d, e, and f to represent 16 numbers.

The decimal number 10 is A in hexadeimal, 11 is B, 15 is F. Case is not relevant.
So 10100101b becomes A5.
No matter how long the binary number is, we can only think of it as four. Splitting long 32-bit binary numbers into four can be represented as an 8-digit hexadecimal number. In other words, one nibble is a single hexadecimal digit.
However, to write a decimal number, you have to calculate the number of digits of each digit of the binary number. It must be multiplied, it must be added, and it is too difficult for bad old men who have been playing games. So I picked a hexadecimal number. It is relatively simple because you can think of hexadecimal by breaking four binary digits. Is it easy to wor
Hexadecimal is expressed by postfix h, as if the binary number is represented by b. h is for "Hexa-decimal". If the first digit is an alphabet, 0 is sometimes added as prefix. So A5 is displayed as 0A5h. In addition, C language code prefer 0xA5. "0x" means a hexadecimal number.

The following table shows binary, and hexadecimal numbers of decimal numbers 0~15.

```
decimal    binary    hexa
0    0000    0
1    0001    1
2    0010    2
3    0011    3
4    0100    4
5    0101    5
6    0110    6
7    0111    7
8    1000    8
9    1001    9
10    1010    A
11    1011    B
12    1100    C
13    1101    D
14    1110    E
15    1111    F
```

```
1234h = 4 * 16 ^ 0 + 3 * 16 ^ 1 + 2 * 16 ^ 2 + 1 * 16 ^ 3 = 4660
```
Let's change the hexadecimal value 1234h to decimal.

``1234h = 4 * 16 ^ 0 + 3 * 16 ^ 1 + 2 * 16 ^ 2 + 1 * 16 ^ 3 = 4660``

To display 4660 as a hexadecimal number, you can repeat to either divide it by 16 or find the squared number closest to the hexadecimal.

Note that it is not very common to convert hexadecimal to binary or binary to decimal. There is a lot of work to change between hexadecimal and binary numbers, but it is rare to do division several times to convert binary&hexadecimal to decimal. You can use the calculator when you need it occasionally.(note 2)


## Sign

We have not thought of negative numbers. In fact, computers do not recognize negative numbers.
Let's think about an 8-bit operation.
Is hexadecimal 0FFh 255 or -1? An 8-bit operation is operation with 8 0's and 1's. What would be the value if you subtract 1 from 0000000b in binary? Conversely, what number will be 0 if you add 1 to it?
The correct answer is 11111111b. Add 1 to it will be 00000000b. It should be 100000000b. Since 8-bit operation can store only 8-bits, the first 1 cannot be stored and disappears. So -1 is 11111111b. Then -2 is 11111110b and -3 is 11111101. Add 2 \ (00000010 \) to -2 and add 3 \ (00000011 \) to -3. Answer is always 0. Then finally we can realize that -127 is 10000001b.
One strange thing is that -128 is 10000000b. Actually binary number of 128 looks also 10000000b. Here we make one promise. If the MSB, the leftmost bit is 1, it is a negative number.
The positive numbers in the 8-bit operation are 00000000 to 01111111. 0 to 127.

Negative numbers are between 11111111 and 10000000. -1 to -128.
Continue adding 1 to 8 bits 00000000. Starting with 0, 1, 2, 3 ... 127, then -128, then -127 then -126 ... -1 and then 0 again.
(Do you remember the Matrix?)
This strange loop can exist because the scope of the operation is limited to 8 bits. The number of digits is not limited in general mathematics. The number of digits increases to infinity. However, in computers, the number of bits the processor can store is limited. A 16-bit computer 8086 can only store up to 16 bits. 
In other words, since the numbers cannot continue to increase and become 0 finally, we can take it negative when MSB is 1.

Please remember that there is no negative number in computer. 11111111b is not -1. But we take it -1 because 11111111b + 1 is 0.
Sometimes 11111111b is 255 and sometime -1. But both of 255 + 1 and -1 + 1 are always 0 in 8-bit operation.
Finally 255 is the same to -1 in 8-bit operation.

You can define a "char" type variable and a "unsigned char" type in C or any programming language and print the binary or hexadecimal value. It must be the same.

I'm from Asia. I'm familiar with Zen and meditation since I was born. If you're not, it's good time to start.
EVERYTHING IS THE SAME ;-)

---

Note1: Please refer to digital circuit class.

Note2: Both of Windows and Linux calculator have programmer mode. It can print and convert binary, hexadecimal and octaldecimal. And it also have Byte, Word, Dword and Qword mode that are 8-bit, 16-bit, 32bit and 64-bit mode respectively.
