# how to study modern assembly

어셈블리를 굳이 한번 공부해보고 싶으시다면 이런 방법으로 해보시기 바랍니다. 요즘 어셈블리 관련해서 어떤 책들이 있는지는 모르겠습니다만 옛날 책들이 많아서 책에 있는 코드가 실행이 안되는 경우가 많을 것입니다. 특히 너무 옛날 책들이 마우스나 키보드, 심지어 플로피 드라이브의 모터 제어같은 내용이 있는데 요즘 Windows7같은 환경에서는 아마도? 실행이 안될 것이므로 쓸데없습니다. 그래서 몇가지 방법들을 추천합니다.

1. 리눅스 환경에서 nasm을 가지고 64비트 어셈블리와 시스템콜 호출을 해보세요. gas는 문법이 너무 다르지만 nasm은 emu8086과 문법이 유사해서 편리합니다.
1. vmplayer나 qemu등의 x86 가상 머신을 가지고 80x86의 부트로더를 만들어보세요.
1. mmx, sse 같은 멀티미디어 어셈블리를 가지고 이미지 축소나 확대같은 이미지 처리를 만들어보세요. 압축이나 암호화 알고리즘에도 많이 쓰이는 방법입니다.
1. 운영체제 만들기 책들이 몇개 있으니 한번 도전해보세요.
1. 솔직히 여기까지 읽어보셨다면 이제 어떤 어셈블리 코드라도 대강 이해할 수 있으실 겁니다. 여기서 접으시고 나중에 실무에 필요할 때 펜티엄 메뉴얼같은거 보시면서 어떤 명령어인지 참고하시면 어떤 코드든 이해하실 수 있습니다. 포기하세요. 편해요 ;-)

If you want to study modern assembly once, try this method. I do not know which books are related to assemblies these days, but there are a lot of old books, so the code in the book often can not be executed. Especially when the old books have contents such as mouse, keyboard, and even motor control of floppy drive. I can not use it because it will not run. So I recommend some methods.

Some people asked me how to study modern assembly.
They tried too old books that describes how to contol mouse, keyboard and even floppy disk.
Those books were written when MS-DOS was common platform for PC.
At that time, PC devices were controlled directly but it's impossible on the modern OSes.

Let me introduce several steps:
1. On the Linux, write some 64-bit assembly code and system call with nasm. Gas has too different grammar, but nasm is handy because it has similar grammar to emu8086.
1. Create an 80x86 boot loader on an x86 virtual machine such as virtualbox or qemu.
1. Create image processing such as shrinking or enlarging images with multimedia assemblies such as mmx and sse. It is also a popular method for compression and encryption algorithms.
1. There are several books on creating an operating system.

references:
* http://www.drpaulcarter.com/pcasm/ : easy guide to make bootloader with NASM
* http://programminggroundup.blogspot.kr/ : system programming with assembly on Linux
* Greate Code book: guide to understand inside of computer
 
