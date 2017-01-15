# divide

Division has both of the signed operation and unsigned operation, as like multiplication. And also the ax and dx registers are used implicitly.

The div instruction does an unsigned division. If the operand is a byte, the AX value is divided. As a result, al stores the remainder and the result value in ah. If the operand is a word size, you should a value to be divided in DX:AX. The result is stored in AX and the rest in DX.

If a value of "dx:ax" is "0:33" and the value of cx is 3, ``div cx`` instruction results ax = 11 and dx = 0.

The idiv instruction does a division with sign.

The detail about div and idiv is the same with mul and imul.
One notable thing is division using sal and sar instructions rather than div command.
This shift instructions do not only bit operation but also multiplication and division operations.
So many operations used in sorting, searching and encryption algorithms are divide-by-two and multiply-by-two operation that can be performed by shift-right(sar) and shift-left(sal) operations. 

The shift operation should be explained in detail again.
