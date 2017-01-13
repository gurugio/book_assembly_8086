# loop

혹시 여기까지 읽고계신 분들이 계시다면 재미가 있으신지 궁금합니다. 혹시 재미가 있으시다면 잠깐 한번 C 코드나 자바 코드를 생각해보세요. 아무 코드나 한번 보셔도 좋습니다. 변수를 만들거나 함수를 호출하거나 루프를 만들거나 하는 일들이 어떻게 보이시나요? 왠지 그 속에 있는 어셈블리 코드가 보이는 것 같은 느낌이 들지 않으시나요? 분명히 말씀드릴 수 있는 것은 어셈블리를 조금만 공부해보시면 언어에 상관없이 더 좋은 코드를 만드실 수 있다는 것입니다. 여기서 좋다는건 사람에게 좋은 읽기 좋은 코드가 아니라 기계에게 좋은 빠른 코드를 말합니다. 읽기 좋은 코드는 그 언어를 잘 알아야 만들 수 있는것 같습니다. 어려운 문제이지요. 빠른 코드는 보다 명확합니다. 어셈블리 코드로 길이가 짧으면 빠릅니다. 루프가 적으면 빠르지요. 루프의 횟수는 내가 정하는게 아니라 필요에 따라 정해지는 것이므로 내가 잘 할 수 있는게 아닙니다. 결국 코드가 어떻게 어셈블될지 안다는게 좀더 짧은 어셈블리 코드로 컴파일되는 코드를 만드는데 도움이 되고 프로그램의 성능을 높일 수 있게 됩니다. 재미로 호기심으로 시작한 어셈블리 프로그래밍이지만 한가지 효과는 얻었네요.

I am wondering if there are any of you who have fun so far.
If so, think about C code or Java code for a while. You can look at any code.
How do you see things like creating a variable, calling a function, or creating a loop?
Do you feel like you're seeing some assembly code inside of it?
I can tell you that if you study a little bit of assembly, you can make better code regardless of type of language.
"better code" I mention here is not readable codes good for people, but fast codes good for machines.
You'd better study the language itself to write more readable and elegance code.
But learning assembly language helps you to write more fast code.

Now let's look into repeating.

반복을 위해서 loop라는 명령어가 있습니다. 사실 점프와 다를게 없습니다. 비교하고 반복하는 것입니다. 단지 cmp, jmp 명령을 하나로 만든 명령입니다. 반복이라는 것은 몇번 반복하겠다는 조건이 깔려있는 것입니다. 이 몇번을 확인해주는 기능이 들어있는게 loop 명령입니다. cmp 를 생략해도 된다는 것입니다. cx 레지스터에 반복할 횟수를 저장해놓으면 자동으로 cx 레지스터의 값을 1씩 줄이다가 0이 되면 루프가 끝나는 것입니다.

There is a loop instruction to repeat something.
Actually it's the combination of cmp and jump instructions.
It needs counter to check how many times it has repeated.
The loop instruction check the counter automatically, so we can skip comparing the counter.
If we stores the number to repeat in cx register, loop instruction decrease cx at each loop and terminates repeating when cx is zero.

``` 
include "emu8086.inc"
 
org    100h
 
mov al, 0
mov bl, 10
 
x_loop:
cmp    al, bl
je     equal
putc 'x'
inc al
jmp x_loop
 
equal:
mov cx, 10
y_loop:
putc   'y'
loop y_loop
 
ret
```

루프를 2가지 방식으로 만들어봤습니다. cmp와 jmp를 이용하는 방식과 loop를 이용하는 방식의 차이를 보면 왜 loop라는 명령을 따로 만들었는지 알 수 있습니다. cmp와 jmp를 가지고도 할 수 있는 일을 왜 따로 명령을 만들었을 까요? 2가지 이유가 있습니다.

Above example shows two kinds of implementations of looping.
First looping uses cmp and jmp instruction, second uses loop instruction.
You might realize soon what the difference is and why loop instruction is necessary.

첫번째는 프로그램의 길이입니다. loop를 쓰면 프로그램의 길이가 줄어듭니다. 프로그램이 짧다는 것은 빠르다는 것 뿐만 아니라 프로그램을 저장할 저장소 즉 디스크나 메모리의 사용이 줄어든다는 것입니다. 지금이야 프로그램을 짧게 만다는게 특수한 분야에서만 신경쓰는 일이되었지만 현업에서 어셈블리로 프로그래밍하던 까마득한 옛날에는 메모리가 엄청난 돈이었습니다. 8086에 얼마의 메모리를 달 수 있다고 말씀드렸나요? 1MB입니다. 컴퓨터의 메모리가 1MB가 한계이고 실제로 1MB의 메모리를 달려면 수백만원을 써야하던 시절입니다. 도스 시절까지만 해도 몇백 바이트의 메모리가 없어서 프로그램이 실행되지 못하는게 부지기수였습니다. 따라서 프로그램이 짧다는게 얼마나 중요한 기술이었을지 알 수 있습니다. 그때는 플로피디스크를 쓰던 시대였으므로 프로그램이 크면 디스크를 읽는 시간도 길어지므로 실행이 된다해도 더 오래 기다려야한다는 문제도 있습니다. 아마 모르시는 분들이 더 많으시겠지만 디스크를 더 오래 읽는 다는 것은 디스크의 수명이 줄어든다는 것입니다. 플로피디스크라는 것이 얼마나 말도 안되게 보관도 어렵고 수명이 짧았는지 상상도 못하실 겁니다. 까마득한 옛날에는 프로그램이 짧은게 좋았다는 것입니다.

First is the length of program. The loop instruction can decrease the length of program.
Long time ago, many machines had only 1MB memory and no hardware cache.
Shorter program was better.
Shorter program is also efficient recently because hardware cache is always not enough.

두번째 이유는 레지스터의 사용을 줄이는 것입니다. cmp와 jmp를 쓰려면 몇번 실행했는지를 저장하는 레지스터와 몇번까지 실행해야할지를 저장하는 레지스터 2개가 필요합니다. 하지만 loop를 쓰면 cx 레지스터 하나만 쓰면 됩니다. 하나 차이가 얼마나 하길래라고 생각하시는 분들이 계실겁니다. 하지만 8086에 몇개의 레지스터가 있는지 생각해보세요. 우리가 막 쓸 수 있는 레지스터는 ax, bx, cx, dx, si, di 정도이므로 6개입니다. 빠르게 접근할 수 있는 레지스터가 6개인데 그중에 하나라면 얼마나 중요할까요. 그때나 지금이나 메모리 속도와 레지스터 속도는 수백만배 차이가 납니다. 지금도 최대한 많은 레지스터를 사용해서 메모리에 접근하지 않는게 컴파일러의 중요한 기술중에 하나입니다. 조금이라도 더 레지스터를 늘리기 위해서 인텔과 ARM이 얼마나 많은 노력을 하는지 모릅니다. 레지스터의 사용을 줄이는 것도 중요하나는 말이지요. 레지스터 접근이 빠르므로 성능과도 연관이 있습니다.

Next is less usage of register. For cmp and jmp instruction we need two registers to store 0 and 10 in above example.
But we need only one register if we use loop instruction.
You can wonder why it is so important to save one register.
But you should consider the 8086 has only 6 general registers: ax, bx, cx, dx, si, di.
Even the latest x86 architecture has only 10~20 registers.

Accessing register is faster than memory several millions times.
If a processor has one more register, it can save so many memory accessings and it directly increase program performance.
