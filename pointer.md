#변수의 주소값 알아내기(포인터) get the address a of a variable

LEA라는 새로운 명령어를 말씀드리겠습니다. 특정 변수의 주소를 알아내는 명령입니다.

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
var1의 값을 그대로 읽는게 아니라 var1이라는 변수가 위치한 메모리 주소를 알아내는 것입니다. 주소를 저장하는 레지스터(BX, SI, DI, BP)는 메모리 포인터같이 대괄호안에서만 사용될 수 있다는 것을 기억하시기 바랍니다.

``mov al, var1`` command stores the value of var1 into al while ``lea bx, var1`` command stores the address of var1 into bx. Please note that the size of the address is 16bit, so bx is used to store the address. On 8086, only BX, SI, DI and BP registers are used to store address with []. (Currently all register can be used for addressing with [] on modern CPUs.)

변수의 주소를 알아내는 것은 중요한 일입니다. 지금은 생각하지 않지만 앞으로 함수를 만들게되면 변수의 주소를 다른 함수로 넘겨서 그 함수에서 변수의 값을 바꾸게 할 것입니다. 변수의 값을 함수에게 전달하면 그 함수는 변수의 값을 바꿀 수가 없습니다. 그 이유는 나중에 함수를 설명할 때 말씀드리겠습니다. mov로 주소 값을 다루는 동작을 설명하면서 lea도 유사한 동작을 하므로 같이 설명했습니다. mov와 lea가 비슷하다고 생각되시면 큰일납니다. 다시한번 예제를 잘 읽어보시고 emu8086으로 실행해보세요. 하나는 변수의 값을 바꾸는 것이고 하나는 변수의 위치를 알아내는 것입니다.

It is important to get the address of a variable. We have not consider yet, but if you create a function in the future, you will pass the address of the variable to another function, which will change the value of the variable in that function. If you pass the value of a variable to a function, the function can not change the value of the variable permanentaly. I will explain why in later chapter. Both of mov and lea are used to access a memory. But there is big difference between them. Please read the example again and run emu8086. One is to change the value of a variable, and the other is to locate the variable.