#메모리 읽고 쓰기 Read/Write memory

데이터를 레지스터에 넣어봤습니다. 그런데 데이터가 여러개라면 어떻게 해야될까요? 주소록을 만들거나 수백명 학생들의 학점 관리를 해야하는데 몇개뿐인 레지스터를 가지고 처리할 수 없겠지요. 그래서 메모리에 데이터를 쓰고 읽고 해야합니다.

보통의 프로그래밍 언어에서는 변수를 하나의 이름으로 생각합니다. C에서는 int a; 와 같이 a라는 이름으로 변수를 지정합니다. 그런데 컴퓨터에게는 변수란게 없습니다. 변수는 사람이 기억하기 쉽도록 이름을 지어놓은 것입니다. 변수란 사실은 메모리의 특정한 위치를 말합니다. C에서는 포인터라는 개념을 배우는데요 이게 메모리 주소를 말합니다. 어셈블리 언어에서는 변수없이 포인터로만 데이터를 사용할 수 있어야합니다. 어셈블러에서는 변수 이름을 만들 수 있도록 해주는데 사실은 눈속임입니다. 그냥 특정 메모리 주소를 변수 이름으로 바꿀 뿐입니다. 

다시한번 말씀드리겠습니다. 변수는 눈속임입니다. 컴퓨터는 이름을 모릅니다. a가 뭔지 모릅니다. 컴퓨터가 이해할 수 있는 것은 주소입니다. 주소는 숫자이니까 이해할 수 있습니다. (왜 수가 첫번째 글이 되었는지 납득이 되시나요? 모든게 수입니다!!)

실습을 해볼까요? 쓸데없이 복잡합니다만 다 이해가 안되도 상관없습니다. 메모리 주소를 표현하는 방법만 생각하세요.

We've put the data into a register. But what if you have multiple data? For example, If we need to create an address book or manage credit for hundreds of students, but all data can not handle it with just a few registers. So we have to write and read data in memory.

In a normal programming language, we think of a variable as a name. In C, ``int a;`` Specify a variable with the name a. But processor actually have no idea about variables. A variable is actually a name to specific location in memory. Programmers can remember the memory location because it has a name. In C, you learn the concept of pointers, which is the memory address. In assembly language, data must be available only as pointers, not name. The assembler allows you to create variable names, which is actually a gimmick. It just changes a specific memory address to a variable name.

I will tell you again. The variable is gimmick. The processor does not know the name. It does not know what "a" is. The processor can understand only the address because the address is a number. (Do you understand why the number is the first one? Everything is number !!)

Let's try an exercise. It is a little bit complicated, but it does not matter if you do not understand everything. Just think of code to represent the memory address.


```
org 100h
mov ax, 0b800h
mov ds, ax
mov cl, 'A'
mov ch, 11011111b
mov bx, 15eh
mov [bx], cx
ret
```


메모장을 실행해서 이 코드를 입력하고 var.txt 라는 이름으로 저장한 후 emu8086.exe var.txt를 실행합니다.

그리고 에물레이터 화면의 밑부분에 있는 screen이라는 버튼을 누릅니다. 그럼 빈 화면의 윈도우가 나타나지요. 그리고 single step 버튼으로 프로그램을 ret까지 실행해봅니다. screen 화면에 A라는 핑크색 글자가 나타났나요?

소스를 설명하겠습니다.
Run Notepad, type this code, save it as var.txt, and run emu8086.exe var.txt in the terminal.

Then press the button called screen at the bottom of the emulator window. Then a blank window appears. Then run the program with the single step button. Did you see the pink letter A on the screen?

Let's read the source.

처음에 org 100h는 죄송하지만 설명하지 않겠습니다. org 100h은 그냥 프로그램 처음에 무조건 넣는 걸로 하겠습니다. 굳이 간단하게 설명하자면 에물레이터를 처음 실행하면 ip가 100h인데 그렇게 프로그램이 100h 위치에서 실행한다는 것을 의미합니다. 왜 100h가 필요한지 왜 다른 값이 아니라 100h인지 등등 설명할게 많은데 사실 알아도 지금은 전혀 쓸모없는 지식입니다. 8086에서만 해당되는 내용이고 요즘 프로세서와는 상관없는 내용이므로 그냥 그렇게 쓰겠습니다.

그 다음이 0b800h라는 주소를 ds에 저장합니다. 0b800h라는 값을 곧바로 ds 레지스터같은 세그먼트 레지스터에 곧바로 저장할 수는 없습니다. 8086 프로세서의 하드웨어적인 제약사항입니다. 그냥 프로세서가 그렇게 못하도록 만들어진 것입니다. 그래서 값을 바로 쓸 수 있는 범용 레지스터에 먼저 저장한 후에 레지스터의 값을 ds에 복사한 것입니다.
At first there is ``org 100h`` but let's ignore it for now. ``org 100h`` will just be put in the beginning of the program unconditionally. Briefly whenever you run the emulator, ip is set to 100h by ``org 100h``. So the program will be placed at 100h. It does not matter why do we need 100h, not other values for now. You can skip it because this is only for 8086 and it is not related to the processor these days.

It then stores the address 0b800h in ds. You can not immediately store a value of 0b800h directly into a segment register, such as a ds register. This is a hardware limitation of the 8086 processor. It's just that the processor is designed to prevent it from doing so. So we first store the value in a general-purpose register that can be written immediately, and then copy the value of the register to ds.


그리고 ds가 또 나타나는 지점은 mov ds:[bx], cx입니다. 여기의 ds:[bx]라는 표현이 바로 메모리의 위치를 나타내는 표현입니다. 이전 글에서 메모리 주소는 세그먼트 주소와 범용 레지스터를 결합해서 나타낸다고 말씀드렸습니다. ds에는 0b800h 값이 있고 bx에는 15eh 값이 있으므로 ds:[bx]는 0b8000h + 15eh라는 주소를 말합니다. 그럼 [] 표시는 뭘까요? mov bx, cx는 bx 레지스터에 cx레지스터의 값을 복사하라는 명령입니다. bx에 메모리 주소가 있고 bx에 저장된 메모리 위치에 cx 값을 넣으라는 명령을 내리고 싶을 때는 bx에 []를 붙입니다. 그래서 결국 mov [bx], cx 혹은 mov ds:[bx], cx 가 됩니다. ds는 생략이 가능합니다. 프로세서가 알아서 메모리 주소를 계산할 때 ds를 읽습니다.(주1) mov [bx], cx로 바꿔서 실행해보세요. 결과는 같습니다.

그리고 cl에 'A'를 씁니다. 이건 화면에 출력할 문자입니다. '1'을 써도 되고 'a'를 써도 됩니다. 작은 따옴표는 그 문자의 아스키코드를 말하는데 아스키코드라는 것을 아신다면 이해하시면 되고 아니라면 그냥 'A'는 A라는 문자 자체를 의미한다고 생각하시면 됩니다.

ch에는 이상한 값을 쓰는데 이것은 그냥 바탕은 핑크색 글자는 흰색이라는 것을 의미합니다.
And ds appears again in ``mov ds:[bx], cx``. Here ``ds:[bx]`` is the representation of memory location. In the previous article, I mentioned that the memory address is represented by a combination of the segment address and the general purpose register. Since ds has a value of 0b800h and bx has a value of 15eh, ``ds:[bx]`` refers to the address 0b8000h + 15eh. And what is the [] for? ``mov bx, cx`` is a command to copy the value of the cx register to the bx register. If you have a memory address in bx and want to issue a command to put cx in the memory location stored in bx, wrap bx with []. So in the end it will be ``mov [bx], cx`` or ``mov ds:[bx], cx``. The ds can be omitted because ds is default segment register. When the processor calculates the memory address, it reads ds if no segment register is specified.(Note 1) Switch to ``mov [bx], cx``. The result is the same.

Then 'A' is written to cl register. This is the character to be printed on the screen. You can use either of '1' or 'b'. The single quotes refer to the ASCII code of the character. If you don't know the ASCII code, just think of 'A' as the letter A itself.

ch has a strange value, which means that the character will be white color on the pink color background.

bx는 화면에서 어느 위치를 말합니다. 혹시 옛날 386AT 컴퓨터가 기억나시나요? 흑백 화면이나 녹색 화면일때도 있었고 브라운관 모니터였지요. 그때 화면은 지금의 윈도우처럼 그래픽이 출력되는 화면이 아니었습니다. 가로 80글자 세로 25글자만 출력할수 있었습니다. 그게 바로 지금 보시는 screen 화면입니다. 옛날의 화면 출력을 에물레이트해준 것이지요.

옛날 화면에 하나의 문자를 출력할때는 2바이트의 값을 써야합니다. 화면 한 줄이 80개의 문자를 출력할 수 있으니까 한 줄당 160바이트입니다. 350은 몇번째 줄일까요? 첫번째 줄은 0~158, 두번째 줄은 160~318, 세번째 줄이 320~ 이므로 350이라는 값은 세번째 줄에 표시한다는 것입니다.

세번째 줄에서 몇번째 칸일까요? 350-320은 30인데 한 문자당 2바이트입니다. 30은 15번째 위치를 말합니다.
The bx register indicates a location of the character on the screen. Do you remember the old 386AT computer? It had black and white screen or green screen and it had a CRT monitor. At that time, the screen was not the screen where the graphic is displayed like the window of the present. It was able to print only 80 characters on a line and 25 lines on screen. That's the screen you see right now on the emulator. The emu8086 emulates the screen output of the old days.

When printing one character on the old screen, you have to write 2 bytes. The screen is 160 bytes per line because a single line can output 80 characters. What is 15eh(350 in decimal)? The offsets of characters on first line is 0,2,4...158, the second line is 160,162,164...318, the third line is 320..., so 350 is displayed on the third line.

And where is it printed in the third line? 350 - 320 = 30. And 2 bytes per character. So 30 refers to the 15th position.

이해가 안되고 어렵다고 생각하시는게 당연합니다. 얼마나 많은 사람들이 C언어의 포인터 개념을 이해하지 못해서 컴퓨터 공학을 포기하고 전자/전기 공학으로 전공을 바꾸는지 아신다면 위안이 되실겁니다. 태어나서 이런 식으로 수학을 사용해본 일이 없으니 당연한 일입니다. 그냥 ds:[bx]만 써넣으면 재미가 없을것 같아서 스크린에 글자 출력하는 소스도 넣어봤습니다. 이해가 안되시면 그냥 넘기세요. ds:[bx]만 알면 됩니다.

우리가 여기에서 배워야할 것은 화면에 몇번째 위치냐가 아니고 메모리의 특정 위치를 지정하는 방법입니다. 다시한번 말씀드리면 ds:bx가 메모리 주소이고 레지스터 값을 읽는게 아니라 레지스터 값을 메모리 주소로 인식해서 메모리에 접근하라는 표현이 ds:[bx]입니다. 

마지막에 있는 ret 명령어는 프로그램을 끝내는 명령입니다.
It is natural that you do not understand and think it is difficult. It would be comforting to know how many people do not understand the concept of C language pointers and then give up computer science and change their major in electronics / electrical engineering. It is natural if you have never used mathematics in this way. How to printing a character does not matter. If you do not understand, just turn it over. You only need to know ``ds:[bx]``.

What we need to learn here is not how many positions are on the screen, but how to specify a specific location in memory. Again, ``ds:[bx]`` is the expression that ``ds:bx`` is a memory address and not a register value.

The final ret command is the command to end the program.

---

주1: 메모리 주소가 저장된 레지스터라니 뭔가가 생각나지 않으세요? bx 레지스터가 바로 C언어에서 포인터의 역할을 하는 것입니다. 포인터는 그냥 읽으면 정수값이지요. 그런데 *를 붙이면 그 값을 메모리 주소로 인식해서 메모리에 접근하지요. C에서의 * 연산자가 어셈블리 언어에서 []가 되었습니다. 정확하게는 반대로 []가 C언어에서 *로 계승된 것입니다. 그거 아세요? C언어는 어셈블리 언어보다는 조금 쉬우면서 비슷하게 강력하고 자유로운 언어를 만들기위해서 생겨났습니다. 그래서 앞으로 어셈블리 언어를 공부하다보면 C언어가 다르게 보일 것입니다. 인간 컴파일러가 되서 C언어 코드를 보면 어떤 어셈블리 명령어에 해당되는지가 보일 것입니다. 매트릭스의 네오가 되는 것이지요.

Note1: A regsister to store the memory address. Do not you remember something? Yes, the bx register is just a pointer to the C language. Pointers are just integer values when read. However, if you append an asterisk, it recognizes the variable as a memory address and accesses the memory. The ``*`` operator in C has become [] in the assembly language. To be precise, [] is inherited from C language to ``*``. Do you know that? The C language was created to make a language that is a bit easier for the assembly language programmer, but more powerful and flexible. So after you study assembly language, C language will look different. You will become a human compiler, you will see what assembly instructions correspond to C language code. It's becoming Neo of the Matrix.
