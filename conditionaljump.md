# conditional jump: jump if necessary

While jmp instruction does jump unconditionally, there is other jump instructions that do jump only-if a certain condition meets. There are three kinds of condition: according to flag register, signed number operation and unsigned number operation.

## jump according to flag register

The following table shows the instructions that jump according to the processor flag register.
However, some commands are described as "jump if 0 or equal".
What is the relationship between 0 and equal?

A comparison of numbers means that you subtract two numbers. For example, if you want to loop 10 times, put 0 in cx and increment cx by one at each loop.
And how can we check cx value reaches 10?
We subsract 10 from cx value. If cx is 10, the zf flag bit will be 1, and you can see that the cx value is 10.

Of course, subtracting with ``sub cx, 10`` command will change cx value, so there is also cmp command to subtract without change.
So "if 0" is the same to "equal".


| instruction | desciption | condition | opposite |
| :--- | :--- | :--- | :--- |
|jz, je|	if 0 or equal	|zf=1|	jnz,jne
|jc, jb, jnae|      if carry, less, not above or equal | cf=1   | jnc, jnb, jae  |
|js           |  sign bit turned on after calculation  |  sf=1 |   jns  |
|jo |   jump if overflow bit sets   | of=1   | jno | 
|jpe, jp|    jump if Parity Even  |  pf=1  |  jpo  |
|jnz, jne|   if not 0 or equal  |  zf=0  |  jz, je | 
|jnc, jnb, jae|   no carry, not below, above or equal  |  cf=0 |   jc, jb, jnae  |
|jns|    if sign bit is 0  |  sf=0  |  js | 
|jno|    if overflow bit is 0  |  of=0  |  jo  |
|jpo, jnp|   if Parity Odd |   pf=0  |  jpe, jp  |

"not above and equal" is the euqal to "below". "not below and equal" is the equal to "above".

There are commands that do the same thing, but have different names like jz, je.
These commands have the same machine code.
So even though I have written jz in my code, it can be displayed as je on the emulator.
Jc and jb are the same commands too.

Thos instructions, which has the different names but do the same, exist for the programmer to understand the code.
But tools like emulators and disassemblers do not know programmer's intend and translate the machine code mechanically.

If you run the following code as an emulator you can see that all jump instructions are assembled to the jnb command.
The machine code of the jnb command is limited to 2 bytes, and the jnb command itself is 73h, which occupies 1 byte.
Therefore, since there is only one byte left to represent the address, the range of jump is 1 byte.
If it is 1 byte, considering the sign, it is from -128 ~ 127.
So it can jump back up to 128 bytes from the it and forward up to 127 bytes.

That is, the jump instructions described in following example are 8-bit jumps, not 16-bit jumps.


```
   jnc a  
   jnb a  
   jae a

   mov ax, 4  
a: mov ax, 5  
   ret
```

What if the program is too big to jump with 128bit range?
Run following example and see how it is assembled in the emulator.
In modern programming language, this is not necessary at all, so I will not explain it in detail.

Note that modern processors like ARM also have a small range of jumps because of the nature of RISC. So there are various solutions, but the compiler does it. Only when you need to develop operating systems or drivers, you need to care such characteristics.

```
jz a  
jb a  
jnc a  
c DB 128 DUP\(9\)  
a:  
ret
```

## jump according to compararation of signed numbers

When we compare 1byte number, which is bigger 0ffh or 10h? It depends whether we compare signed numbers or unsigned numbers.
If we compare signed numbers, 0ffh is -1 and 10h is 16 in decimal, so 10h is bigger.
If not, 0ffh is bigger because it is 255.
Therefore jump instructions are separated for signed comparing and unsigned comparing. 

Following is a list of jump instructions for signed comparing.

| instruction  |  description   | condition  |  oppsite  |
| :--- | :---   | :---  | :--- |
|JE , JZ |   Jump if Equal \(=\). Jump if Zero.  |  ZF = 1   | JNE, JNZ  |
|JNE , JNZ |   Jump if Not Equal \(&lt;&gt;\). Jump if Not Zero.  |  ZF = 0  |  JE, JZ  |
|JG , JNLE |   Jump if Greater \(&gt;\). Jump if Not Less or Equal \(not &lt;=\).   | ZF = 0 and SF = OF  |  JNG, JLE  |
|JL , JNGE |   Jump if Less \(&lt;\). Jump if Not Greater or Equal \(not &gt;=\).  |  SF &lt;&gt; OF  |  JNL, JGE  |
|JGE , JNL |   Jump if Greater or Equal \(&gt;=\). Jump if Not Less \(not &lt;\).  |  SF = OF   | JNGE, JL  |
|JLE , JNG |   Jump if Less or Equal \(&lt;=\). Jump if Not Greater \(not &gt;\).  |  ZF = 1 or SF &lt;&gt; OF   | JNLE, JG|

``<>`` means "not equal". "Not greater or equal" means "less" and "Not less or equal" means "greater".
You should notice that opposite of ``<`` is ``>=``, not ``>``.

## jump according to comparation of unsigned numbers

| instruction  |  description   | condition  |  oppsite  |
| :--- | :---   | :---  | :--- |
|JE , JZ    |Jump if Equal \(=\). Jump if Zero.  |  ZF = 1  |  JNE, JNZ  |
|JNE , JNZ   | Jump if Not Equal \(&lt;&gt;\). Jump if Not Zero.  |  ZF = 0  |  JE, JZ  |
|JA , JNBE  |  Jump if Above \(&gt;\). Jump if Not Below or Equal \(not &lt;=\).  |  CF = 0 and ZF = 0  |  JNA, JBE | 
|JB , JNAE, JC  |  Jump if Below \(&lt;\). Jump if Not Above or Equal \(not &gt;=\). Jump if Carry.   | CF = 1  |  JNB, JAE, JNC  |
|JAE , JNB, JNC  |  Jump if Above or Equal \(&gt;=\). Jump if Not Below \(not &lt;\). Jump if Not Carry.   | CF = 0  |  JNAE, JB  |
|JBE , JNA  |  Jump if Below or Equal \(&lt;=\). Jump if Not Above \(not &gt;\).  |  CF = 1 or ZF = 1  |  JNBE, JA|

You can understand without more explanation.

