# Array

Arrays can be viewed as contiguous variables. Continuous means that all variables are stored in contiguous memory. If you have an 8-bit variables at location 1000h, 1001h and 1002h, you can combine these three variables into a single array. An array is defined as a set of variables. For example, a string is a array of byte size variables. Each byte represents one ASCII code value of the character. It is a set of single-byte variables.

Here is an example of declaring an array.

```
a DB 48h, 65h, 6Ch, 6Ch, 6Fh, 00h
b DB 'Hello', 0 
```

Array b is exactly the same as an array a. The compiler changes the string in quotes to a set of bytes. The picture below shows memory when this array is declared. Note that the end of the string is 0, not a character. A value of 0 indicates that the string is finished. If you do not have a zero, you do not know where the string ends.

![](/assets/array.gif)
 
The elements of array can be accessed by []. For example:
 
```
MOV AL, a[3] 
```

Above code stores 6ch in al.
Or memory index registers, BX, SI, DI and bp, can also be used.

``` 
MOV SI, 3
MOV AL, a[SI]
```
Big array can be declared with DUP like following.

``` 
Number DUP ( value(s) )
Number: The number of copy (should be constant)
value: data to copy
```

For example,

``` 
c DB 5 DUP(9)
```
above is the same to following.

``` 
c DB 9, 9, 9, 9, 9
``` 

For one more example

``` 
d DB 5 DUP(1, 2)
``` 

above is the same to following.

``` 
d DB 1, 2, 1, 2, 1, 2, 1, 2, 1, 2
``` 

Of course, if you need a value that is larger than 255 or less than -128, you can use DW rather than DB.
DW cannot declare character string.
Now you've learned how to use a string and memory addressing.
Please make a program to print "hello, world" on screen.
