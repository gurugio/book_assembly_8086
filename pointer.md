# get the address a of a variable: pointer

Let me introduce a new command LEA. It gets the address of a variable.

```
ORG 100h
MOV AL, VAR1 ; check value of VAR1 by moving it to AL.
LEA BX, VAR1 ; get address of VAR1 in BX.
MOV BYTE PTR [BX], 44h ; modify the contents of VAR1.
MOV AL, VAR1 ; check value of VAR1 by moving it to AL.
RET
VAR1 DB 22h
END
```

``mov al, var1`` command stores the value of var1 into al while ``lea bx, var1`` command stores the address of var1 into bx. Please note that the size of the address is 16bit, so bx is used to store the address. On 8086, only BX, SI, DI and BP registers are used to store address with []. (Currently all register can be used for addressing with [] on modern CPUs.)

It is important to get the address of a variable. We have not consider yet, but if you create a function in the future, you will pass the address of the variable to another function, which will change the value of the variable in that function. If you pass the value of a variable to a function, the function can not change the value of the variable permanentaly. I will explain why in later chapter. Both of mov and lea are used to access a memory. But there is big difference between them. Please read the example again and run emu8086. One is to change the value of a variable, and the other is to locate the variable.
