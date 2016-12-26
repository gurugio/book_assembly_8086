# Introduction to the emu8086 program

태초의 언어를 배우기에는 지금 우리가 가진 컴퓨터가 너무 복잡합니다.  
Current computers are too complex for us to start the language of the beginning.  
태초의 언어를 배우기에 좋은게 태초의 컴퓨터가 아닐까요?  
I think the computer of the beginning is the best to learn the language of the beginning.  
그래서 태초의 컴퓨터에 들어가던 8086 프로세서를 사용하겠습니다.  
So we will use 8086 processor that was used when the computer industry started.  
8086 프로세서는 용산에있는 컴퓨터 부품 가게들 \(비디오카드나 메모리같은 부품이 아니라 칩을 말합니다\)에 가면  
몇천원에 살 수 있습니다. 그런데 8086 프로세서만 있다고해서 뭘 할 수 있는게 아니지요.  
You might be able to buy the 8086 processor on Amazon.  
But you cannot do anything with only 8086 processor chip.  
8086에 프로그램을 전달하고 또 프로그램의 실행 결과를 보려면 입출력 장치가 필요합니다.  
You need more devices to input program and out the result.  
8086에 키보드나 모니터를 달아야겠지요.  
For instance, you need a Mainboard, a monitor and a keyboard that are able to be connected to 8086 chip.  
그런 수고를 덜어서 우리 컴퓨터에서 8086을 실험하게 해주는게 emu8086이라는 프로그램입니다.  
But we have some emulation programs that help us to experience 8086 chip on our desktop.  
사실 제가 학부때는 8086 프로세서와 SRAM 등의 칩들을 직접 사다가 전선을 납땜하고 숫자 키패드와 텍스트 LCD 를 연결해서 컴퓨터를 직접 만드는 수업이 있었습니다. 지금도 있는지는 모르겠네요. 그렇게 8086 컴퓨터를 만들면 컴퓨터의 역사에서 볼 수 있는 최초의 계산기나 애플1과 동일한 물건이 됩니다.  
In some University there was classes to make hand-made computer with processor chip, SRAM and some other devices like a Text-lcd and number-keypad. You can make the similar machine to Apple-1 or the first generation calculator.  
If you don't know what the Apple-1 is, please google it. You would find an ugly machine looks like old-fashined typewriter.  
우리는 emu8086으로 아주 간단하게 태초의 컴퓨터를 체험해볼 수 있는 것이지요.  
Anyway we don't need to make any machine. Let us use the emulator program 'emu8086'.

[Download emu8086](http://www.emu8086.com/)

There are many other emulator programs.  
If you are using Linux machine, you might want to another emulator: [I8086emu](http://sourceforge.net/projects/i8086emu/files/latest/download?source=navbar)

압축을 풀면 emu003디렉토리에 emu8086.exe라는 파일이 있습니다. 실행하면 작은 텍스트 창이 나타납니다. file이라는 메뉴가 있는데 메뉴중에 emulate가 있습니다. 눌러보면 다음과 같이 뭔가 실행된듯한 하지만 이해할 수 없는 화면이 나타납니다. 이제 8086을 가지고 놀 준비가 되었습니다.

If you run the emu8086 program, you can see the small text editor window. There is an emulate menu in the file menulist.

If you select the emulate menu, you can see following window.

You might not understand anything in the window. It might looks very odd.

But now you're ready to start.

![](/assets/2520.png)

PS. Several years ago, emu8086 was a free software but current version you should pay a little money. If you're very poor, you can google the old version. This document was written in many years ago when the emu8086 was free. That's why I used emu8086. So some menu is not the same with the latest version. But the essential functions are the same. So you don't need the latest version to read this document. 

And also you don't need to emu8086 program. You can use the other free emulator. Again, the essential functions are the same in most of the emulators. You can read the register value and memory value after run a single instruction. Actuall that's all we need.

