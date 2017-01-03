#변수 Variable

변수는 그냥 메모리의 위치입니다. 그런데 메모리의 위치는 숫자이고 숫자는 사람이 기억하기에 어렵지요. 그래서 어셈블러에서도 변수를 만들어서 쓸 수 있도록 해줍니다. 가만 생각해보면 이전 글에서 0b800h:0h 위치에 데이터를 썼는데요 이 메모리 번지를 변수로 만들어 놓으면 간편해지지 않을까요?(주1)

일단 간단한 변수를 만들어보겠습니다. 백문이 불여일코라 백번 들어도 한번 코딩해보는게 더 잘 이해되지요. 다음 코드를 실행해보겠습니다.

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

이전에도 말씀드렸듯이 대소문자 구분은 하지 않습니다. VAR1이나 var1이나 같습니다. db는 define byte 혹은 data byte라고 읽을 수 있습니다. 한바이트의 변수를 선언하는 것입니다. dw는 한 워드의 변수를 선언하는 것입니다.

에물레이터를 실행해서 프로그램을 실행해보면 AL과 BX 값이 7과 1234h로 변하는 것을 확인할 수 있습니다.

간단하지요? 그런데 여기에 더 중요하게 봐야할 것이 있습니다. 변수는 메모리의 위치라고 말씀드렸습니다. 우리가 만든 var1, var2의 메모리 위치는 어디는 어디일까요? 우리는 메모리의 어디를 변수로 이름지은 걸까요?

As I mentioned before, it is not case sensitive. VAR1 and va1 are the same. "DB" can be read as define byte or data byte. It declares a variable with one byte size. "DW" declares a word size variable.

If you run the program with the emulator, you can see that the AL and BX values change to 7 and 1234h, respectively.

Simple. But there is something more important here. I mentioned that the variable is the address of the memory. What are the memory addresses of var1 and var2? Where do we name the address of the memory?

해답은 에물레이터에 있습니다. 우리가 만든 변수의 값이 7과 1234h입니다. 에물레이터 어딘가에 7과 1234가 보이면 그게 우리가 만든 변수의 위치입니다.

The answer lies in the emulator. The values of the variables we created are 7 and 1234h. If you see 7 and 1234h somewhere in the emulator, that's the location of the variables we created.

![](/assets/2524.png)


ret 명령어에 해당하는 숫자는 C3입니다. C3 명령은 메모리 7107h번지에 저장되어 있습니다. ret 명령이 실행되면 프로그램은 끝나게 되는데요 바로 ret 명령 다음에 07과 3412가 보입니다.

그리고 에물레이터 화면 오른쪽을 보면 mov al, [00108h] 라고 써진게 보입니다. [] 표시안에는 메모리의 주소가 들어간다고 말씀드렸습니다. 즉 al에 저장할 변수 var1의 주소가 ds:108h라는 것입니다. ds를 깜박하고 주소가 108h라고 생각할 수 있습니다. 항상 세그먼트 레지스터를 생각해야 합니다. ds의 값이 700h 이므로 최종 주소값은 7108h입니다. 

The number corresponding to the ret command (AKA, machine code) is C3. The C3 value is stored in memory address 7107h. When the ret command is executed, the program is finished. You can see 07 and 3412 after the ret command.

And on the right side of the emulator screen, it looks like ``mov al, [00108h]``. I mentioned that the address of memory is in []. That is, the address of the variable var1 to be stored in al is ``ds:108h``. You might forget ds and think of the address as 108h. But you should always think of segment registers when calculating the address. Since the value of ds is 700h, the final address value is 7108h. Check the value of address 7108h. Is it 07?

바로 우리가 선언한 변수입니다. 우리가 변수들을 ret 명령 바로 다음에 선언했는데 그게 실제 프로그램에서도 ret 명령 바로 다음에 저장된 것입니다.

이상한건 1234h를 저장했는데 왜 3412로 보인다는 것입니다. 이것은 little endian 이라는 규약입니다. 작은 인디언이 아니라 endian입니다. 반대로 big endian도 있습니다. little endian은 프로그램에서 메모리에 쓴 값을 반대 순서로 저장하는 것입니다. 16비트 1234h를 쓰면 34, 12 순서로 저장되고 32비트 12345678h을 저장하면 78, 56, 34, 12 로 저장됩니다. 메모리의 단위가 바이트이므로 87654321로 저장되는게 아니라 바이트 단위로 나눠져서 순서가 바뀝니다. 여러가지 이유가 있지만 자세한 것은 위키피디어등을 참고하시고 이 글에서는 일단 그렇다는 것만 알고 넘어가겠습니다. 두꺼운 전공책도 아니고 새로 나오는 개념들을 모두 설명하려고하면 끝이 없습니다. 핵심 줄기에 집중하겠습니다.

우리가 소스 파일에 변수를 선언하면 프로그램에도 그 위치대로 변수가 생겨날까요? 이렇게 해보면 알겠지요.

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

에물레이터로 돌려보세요. mov al, var1 다음에 7 값이 보이나요? 그리고 al에 저장하려는 메모리의 주소값이 몇인가요?
Can you see the 7 next to ``mov al, var1`` on the emulator? What is the address of the var1 that is stored in al register?

![](/assets/2525.png)

다시 한번 말씀드리지만 컴퓨터는 변수 이름이 뭔지 모릅니다. 오직 숫자만 압니다. 103h라는 숫자를 주소값으로 넘겨주면 해당 메모리의 위치를 읽어서 레지스터로 가져오는 것입니다.

그럼 변수 값 자체가 주소값이면 어떻게 될까요? 즉 포인터 변수를 흉내내보면 어떨까요?

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
소스를 보면 addr 변수가 있는데 120h 입니다. 즉 우리는 ds:[120h] 위치에 1234h 값을 저장하려는 것입니다. 에물레이터를 실행해보세요.  mov word ptr [addr], 1234h 명령이 우리가 생각했던대로 mov word ptr [120h], 1234h 로 어셈블링 되었나요? 아니니까 제가 물어본 것이겠지요.

If you look at the source, you have the addr variable that is 120h. That is, we want to store the value 1234h in the ``ds:[120h]`` address. Run the emulator. The ``mov word ptr [addr], 1234h`` command assembled into ```mov word ptr [120h], 1234h`` as we wrote? No, that's why I asked.

어셈블링된 코드를 보면 mov word ptr [10fh], 1234h로 나타납니다. 왜 그럴까요? 제가 바로 위에서 말씀드렸듯이 변수란 메모리 위치이기 때문입니다. addr이 위치한 메모리 주소가 몇인가요? 값 120h를 찾아보세요. little endian 규약에 따라 20, 01 이라고 써있겠지요. 바로 10fh 입니다. 변수는 값이 아니라 메모리 위치라는 것은 몇번을 강조해도 지나치지 않습니다.

Looking at the assembled code, it appears as ``mov word ptr [10fh], 1234h``. Why? As I said above, this is because the variable is a memory location. What is the memory address the addr located at? Find the value 120h. According to the little endian convention, the numbers we should find are 20, 01. It's just at 10fh. As I emphasized already, a variable is not a value but a memory location.

mov al, var1 의 어셈블링 코드는 mov al, byte ptr [var1] 이었습니다. 변수를 메모리 위치로 생각하는 것입니다. mov al, var1이 마치 var1 이라는 문자를 변수 값 7로 바꾸는 것처럼 보이지만 사실은 var1의 메모리 주소를 계산하고 그 다음 메모리에서 해당 주소값에 있는 데이터를 읽어오는 것입니다.

The assembling code for ``mov al, var1`` is not ``mov al, 7`` but ``mov al, byte ptr [var1]``. Think of variables as memory locations. ``mov al, var1`` looks like it would pass the value in var1 into al directly, but in fact it is calculating the memory address of var1 and then reading the data from that address in memory and store the data in the register.

mov al, var1 과 mov al, byte ptr [var1] 이 같은 명령이라는게 이상하게 느껴질 수도 있습니다. 하지만 mov al, var1은 어셈블러가 인간이 보기 편하도록 제공하는 기능이라고 생각하고 원래 기계가 이해하는 문법은 mov al, byte ptr [var1]이라고 생각하셔야 합니다.

The equality of ``mov al, var1`` and ``mov al, byte ptr [var1]`` may seem strange. However, ``mov al, var1`` is a translation that the assembler provides for human readability, and the syntax understood by the original machine is ``mov al, byte ptr [var1]``.

그럼 변수에 주소 값일 저장하는 것은 어떻게 해야할까요? 소스 코드에도 써놓았듯이 bx레지스터에 주소 값을 저장하고 [bx] 명령으로 접근하면 됩니다.

So how do you store a value in a memory? Save the address value in the bx register as you wrote it in the example source code and access it with the [bx] command.

mov word ptr [addr], 1234h 명령이 실행되면 addr 변수에 1234h가 저장됩니다. 그리고 mov bx, addr 명령을 실행하면 bx 레지스터에 1234h 값이 저장됩니다. 그리고 마지막으로 ds:[bx] 위치에 1234h 값을 저장합니다.

When the ``mov word ptr [addr], 1234h`` instruction is executed, 1234h is stored in the addr variable. And when ``mov bx, addr`` command is executed, 1234h value is stored in bx register. Finally, we store a value of 1234h in the ``ds:[bx]`` location.

에물레이터의 aux 버튼을 눌러서 메모리 창을 열고 0700:1234 주소의 메모리를 확인해보세요. 34 12 라는 값이 나타날 것입니다.

Press the aux button on the emulator to open the memory window and check the memory at address 0700:1234. A value of 34 12 will appear.

C 언어에서 포인터 변수를 읽어서 특정한 메모리 위치에 값을 쓰는 것을 간접 참조라고 부릅니다. 왜 간접일까요? 우리가 어셈블리 언어로 직접 만들었듯이 포인터 변수에 저장된 주소값에 접근하러면 우선 변수에 저장된 주소 값을 레지스터로 읽어와야 합니다. 그리고 레지스터 값을 이용해서 메모리에 접근할 수 있지요.

In C, writing a value into a memory addressed by a pointer variable is called indirect reference. Why is it indirect? To access the address value stored in the pointer variable, as we did in the assembly language, you must first read the address value stored in the variable into the register. And then you can access memory using the register values.

일반 변수는 한번에 접근됩니다. 변수의 주소를 알기때문이지요. 포인터 변수는 변수를 한번 레지스터로 읽어오고 그 다음에야 최종 접근하려는 위치에 접근됩니다. 이렇게 한번 더 메모리를 읽는 절차가 있기 때문에 간접 접근이라고 부르는 것입니다.

Normal variables are accessed at once because we know the address of the variable and read the memory once. But for pointer, CPU read the pointer value into a register, and then read the final memory with the address in the register. There is one more memory access so that it's called as indirect reference.

몇번째 말씀드리는건지 모르겠네요. C는 어셈블리의 확장판입니다!!

Again. C is an extension of assembly!!
 
 

주1: unsigned short *p = 0xb8000; 이렇게 만들면 일일이 주소값을 쓰는 것보다 변수로 주소를 지정할 수 있겠지요.
Note1: ``unsigned short *p = 0xb8000;`` this is a way to store the address value in the variable.