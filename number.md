# 컴퓨터 세상의 원리: 수 The principle of the computer world: number

숫자가 먼저입니다.

The number was born first.

어셈블리보다 숫자가 먼저 태어났지요. 수는 모든 만물의 원리라고 피타고라스도 말했습니다. 당연히 컴퓨터도 숫자로 돌아갑니다. 수학자들이 컴퓨터라는 시스템을 생각해낸게 이상하지 않지요.

The number is older than the assembly. As Pythagoras said, the number is the principle of all things. The principle of the computer is naturally the number. The computer was invented by mathmaticians.

왜 컴퓨터가 숫자로 이루어졌는지는 점점 자연스럽게 알게됩니다. 지금은 일단 어셈블리 언어에 사용하는 2진수와 16진수를 알아보겠습니다. 2진수 16진수를 알아야 어셈블리 언어를 입력하고 이해할 수 있습니다. 일단 emu8086 화면에 나오는 모든 숫자가 16진수이기 때문에라도 16진수가 뭔지 알아야합니다.

We will understand how the computer was related to the number in near future. First you must know the binary number and hexadecimal number. When you write assembly code you should use the binary and hexadicimal. And every number emu8086 shows is the hexadecimal.

## 10진수

우리는 10진수를 씁니다. 0부터 9까지의 숫자 표기를 이용해서 수를 표현합니다. 이 숫자를 여러개 붙여서 특정한 값을 표현합니다. 예를 들어 754는 각 자리마다 10의 지수승으로 이루어져있습니다.

Unfortunitely we use decimal number system. We use the number from 0 to 9 when we write a number. And you sometimes write one more numbers serially to write big number. For the big number, each digit means the numerical index.

For example, let us think about 754.

```7*10^2 + 5*10^1 + 4*10^0```

7은 10의 제곱승, 5는 10의 1승, 4는 10의 0승입니다. 10의 0승은 1이고요.

7 is multiplied by square of 10 and 5 is multiplied by 10. 4 is multiplied by 10 to the power of 0. 10 to the power of 0 is 1.

만약에 자리가 바뀌면 완전히 다른 숫자가 됩니다. 574는 754가 아니지요.

If the position is changed the number would be absolutely different. 574 is not 754.

## 2진수 Hexadecimal

컴퓨터는 전자적으로 2가지 상태를 가진 물질로 이루어져있습니다. 전자적인 신호가 있으면 1 없으면 0의 상태를 가진 물질입니다.\(주1\) 그래서 0부터 9까지 10개의 숫자를 기억할 수 없습니다. 그래서 컴퓨터는 2진수를 사용하는 것입니다
The processor is made of materials with two states, 1 and 0. If there is an electronic signal, the state of the material become "1" or 0.(Note 1) So computer cannot count 10 numbers from 0 to 9. So the computer use binary numbers.

2진수란 수를 나타내는 숫자 표기가 2개인 것입니다. 0과 1입니다. 10진수는 열개의 숫자 표기가 있었지요.
Binary number is a numbering system that has two numbers 0 and 1 while the decimal number has ten digits.

2진수와 10진수를 구분하기 위해서 우리는 보통 2진수의 숫자 뒤에 b라고 씁니다. 영어로 2진수가 binary이기 때문입니다.
To distinguish between binary and decimal numbers, we usually write b after the binary number. Because the binary number is "binary" in English.

2진수의 수 10100101b는 10진수로 몇일까요?
What is the binary number 10100101b in decimal number?

10진수 754는 각 자리마다 10의 제곱승을 했습니다. 마찬가지로 2진수도 각 자리마다 2의 제곱승을 하면 됩니다. 가장 오른쪽은 2의 0승, 한칸 왼쪽은 2의 1승 이렇게 자리가 올라갈때마다 2를 곱하면 됩니다.
The decimal number 754 has a power of 10 for each digit. Likewise, a binary number can be raised to the power of two for each digit. The rightmost is 0 of 2, the left of 1 is 2, and so on.

```10100101b = 1*2^0 + 1*2^2 + 1*2^5 + 1*2^7 = 1 + 4 + 32 + 128 = 5 + 160 = 165```

반대로 165를 2진수로 표현하려면 어떻게 해야할까요? 중고등학교 수학시간에 배웠듯이 2로 나눠서 나머지를 나열하는 방법도 있습니다. 그보다 저는 165와 가장 가까운 2의 제곱수를 생각하는 방법을 좋아합니다.

Conversely, how do you express 165 as a binary number? As you learned in school, there are ways to list the rest after dividing by 2. Rather, I like the way calculating the power of 2 which is the closest to 165.

가장 먼저 165 = 128 + 37 라고 생각합니다. 128은 2^7인 것을 외우고 있기 때문입니다. 어셈블리 언어를 공부하다보니 2의 제곱수를 자연스럽게 많이 외우게 되었습니다.

I find 165 = 128 + 37 first because 128 is 2 ^ 7. As I studied assembly language, I was able to memorize several numbers which are the square of 2.


165 = 2^7 + 37 이라면 그 다음으로 37 = 32 + 5 입니다. 32는 2^5 입니다. 그렇게 5도 4+1 이므로 2^2 + 1입니다. 결론은 165 = 2^7 + 2^5 + 2^2 + 2^0 입니다. 이걸 2진수로 바꾸는 방법은 제곱승의 숫자만큼 0을 붙이는 것입니다. 2^7은 0이 7개이므로 10000000b 입니다. 2^5는 0이 다섯개이므로 100000, 2^2는 100입니다. 다 더하면 10100101b 입니다.

After 165 next is 37 = 32 + 5. 32 is 2 ^ 5. So 5 is 4 + 1, so it is 2 ^ 2 + 1. The conclusion is 165 = 2 ^ 7 + 2 ^ 5 + 2 ^ 2 + 2 ^ 0. The way to convert this to binary is to add zero to the power of the square. 2 ^ 7 is 10000000b since there are seven zeros. 2 ^ 5 is 100000 and 2 ^ 2 is 100 because there are five zeros. Add everything to 10100101b.

어떤 방법으로 계사하던지 아니면 계산기를 쓰던지 상관없습니다. 계산기가 제일 좋겠네요.

It does not matter whether you calculate for yourself or use a calculator. Of course, the calculator is the best.

참고로 8086 어셈블리 언어에서는 2진수의 각 자리를 비트bit라고 부릅니다. 4개의 비트는 니블nibble 8개는 바이트byte 2개의 바이트를 워드word 4개의 바이트를 double word라고 부릅니다. 그리고 가장 오른쪽 비트를 low bit 라고 부르거나 Least Significant Bit라고 부르고 가장 왼쪽 비트는 Most Significant Bit라고 부릅니다.

Note that in the 8086 assembly language, each digit of a binary number is called a bit. 4 bits are one nibble and 8 bits are one byte. And 2 bytes are one word and 2 words are called as double word. The rightmost bit is called as the Least Significant Bit (LSB), and the leftmost bit is called as the Most Significant Bit (MSB).


## 16진수

컴퓨터 업계에서는 16진수를 흔하게 씁니다. 왜 그럴까요? 2진수를 쉽게 변환할 수 있고 또 2진수를 줄여서 쓸 수 있기 때문입니다. 32비트의 주소를 표현할 때 혹은 1024같이 비교적 크기 않은 값을 표현할 때 2진수로 표현하면 32비트 주소는 무려 32자리의 2진수를 써야합니다. 1024는 2^10이므로 0을 10개 써서 10000000000b로 써야합니다. 굉장히 불편합니다. 또 0의 갯수를 잘못 쓸 위험도 많아서 코드를 입력할 때 실수하기가 너무 쉽습니다.
In the computer industry, hexadecimal numbers are common. Why? This is because binary numbers can be easily converted into hexadecimal and vise versa. When representing a 32-bit address you need 32 binary digits in binary. Since 1024 is 2 ^ 10, you should use ten zeros to write 10000000000b. It is very inconvenient. It is also very easy to make mistakes when typing code because there is a high risk of misplacing the number of zeros.


그렇다고 10진수로 쓰려면 2진수의 각 자리가 몇번째 자리인지 계산해야합니다. 곱셈도 해야되고 덧셈도 해야되고 구구단을 해본지 한참된 머리나쁜 어른들에게는 너무 어렵습니다. 그래서 16진수를 고른 것입니다. 16진수를 2진수를 4개씩 끊어서 생각하면 되기때문에 비교적 간단합니다. 일이삼사는 쉽자나요?

However, to write a decimal number, you have to calculate the number of digits of each digit of the binary number. Multiply by 2 and add 1 and ... it is too difficult. So hexadecimal number is picked. It is relatively simple because you can think of hexadecimal by breaking four binary digits.


10100101b를 4개씩 끊으면 1010 과 0101입니다. 1010은 10이고 0101은 5입니다.16진수에서 10이라는 숫자가 있을까요? 아닙니다. 16진수에서 10은 A입니다. 16진수를 16개의 숫자가 있겠지요? 그런데 숫자는 10개뿐이므로 알파벳 a,b,c,d,e,f를 빌려와서 16개의 숫자를 표현합니다.
If you cut four digits from 10100101b, it is 1010 and 0101. 1010b is 10 in decimal and 0101 is 5. Is there a 10 in hexadecimal? no. In hexadecimal, 10 is A. Is there more numbers bigger than 9? No. Since there are only 10 numbers, we borrow the alphabets a, b, c, d, e, and f to represent 16 numbers.


10진수로 10은 a, 11은 b, 15는 f입니다. 대소문자는 상관없습니다.
그래서 10100101b는 4개로 나눠서 A5가 됩니다.

The decimal number 10 is A in hexadeimal, 11 is B, 15 is F. Case is not relevant.
So 10100101b becomes A5.

2진수의 숫자가 아무리 길어도 우리는 4개씩 나눠서만 생각하면 됩니다. 길고긴 32비트의 2진수를 4개씩 쪼개면 8자리 16진수로 나타낼 수 있습니다. 다시 말하면 하나의 니블이 16진수 한자리가 되는 것입니다.
No matter how long the binary number is, we can only think of it as four. Splitting long 32-bit binary numbers into four can be represented as an 8-digit hexadecimal number. In other words, one nibble is a single hexadecimal digit.


그리고 2진수를 b로 표시하는 것 같이 16진수로 h로 표시합니다. hexa-decimal이라는 표시입니다. 첫 자리가 알파벳이면 0을 붙이기도합니다. 그래서 A5는 0A5h라고 표시합니다. 또 C 언어에서는 0xA5라고 표시합니다. 0x가 16진수라는 표시입니다.

다음은 10진수, 2진수, 16진수를 표로 보여줍니다.

However, to write a decimal number, you have to calculate the number of digits of each digit of the binary number. It must be multiplied, it must be added, and it is too difficult for bad old men who have been playing games. So I picked a hexadecimal number. It is relatively simple because you can think of hexadecimal by breaking four binary digits. Is it easy to wor
Hexadecimal is expressed by postfix h, as if the binary number is represented by b. h is for "Hexa-decimal". If the first digit is an alphabet, 0 is sometimes added as prefix. So A5 is displayed as 0A5h. In addition, C language code prefer 0xA5. "0x" means a hexadecimal number.

The following table shows decimal, binary, and hexadecimal numbers.

```
decimal    binary    hexa
0    0000    0
1    0001    1
2    0010    2
3    0011    3
4    0100    4
5    0101    5
6    0110    6
7    0111    7
8    1000    8
9    1001    9
10    1010    A
11    1011    B
12    1100    C
13    1101    D
14    1110    E
15    1111    F
```

그럼 16진수의 값 1234h를 10진수로 바꿔보겠습니다. 16의 제곱수가 0에서 시작해서 하나씩 늘어난다고 생각하면 됩니다.

1234h = 4\_16^0 + 3\_16^1 + 2\_16^2 + 1\_16^3 = 4660

4660을 16진수로 표시하는 것은 2진수처럼 16의 제곱수를 외워서하던가 나눗셈을 하면 됩니다.

참고로 16진수나 2진수를 10진수로 바꿀 일은 별로 없습니다. 16진수와 2진수를 서로 바꿀 일은 많습니다만 10진수로 변환하려고 나눗셈을 몇번씩 하는 일은 드뭅니다. 가끔 필요할 때는 계산기를 이용하면 됩니다.\(주2\)

Let's change the hexadecimal value 1234h to decimal.

```1234h = 4 * 16 ^ 0 + 3 * 16 ^ 1 + 2 * 16 ^ 2 + 1 * 16 ^ 3 = 4660```

To display 4660 as a hexadecimal number, you can repeat to either divide it by 16 or find the squared number closest to the hexadecimal.

Note that it is not very common to convert hexadecimal to binary or binary to decimal. There is a lot of work to change between hexadecimal and binary numbers, but it is rare to do division several times to convert binary&hexadecimal to decimal. You can use the calculator when you need it occasionally.(note 2)


## 부호의 표시 Sign

지금까지 음수는 생각하지 않았습니다. 사실 컴퓨터는 음수를 인식하지 못합니다. 8비트의 연산을 생각해보겠습니다.

0FFh는 255일까요 아니면 -1일까요? 8비트 연산이라는 것은 0과 1이 8개 있다는 것입니다. 2진수로 0000000b에서 1을 빼면 값이 뭐가 될까요? 반대로 생각해보면 어떤 수에 1을 더하면 0이 될까요?

정답은 11111111b입니다. 여기에 1을 더하면 00000000b가 됩니다. 8비트 연산이므로 원래는 100000000b이 되야하지만 1이 저장되지못하고 사라지는 것입니다. 그래서 -1은 11111111b입니다. 그럼 -2는 11111110b이고 -3은 11111101 입니다. -2에 2\(00000010\)를 더해보고 -3에도 3\(00000011\)을 더해보세요. 이렇게 계속 하다보면 -127은 10000001b입니다.

한가지 이상한건 -128은 10000000b이라는 겁니다. 양수로 128도 10000000b인데요. 여기서 우리는 한가지 약속을 합니다. MSB 즉 가장 왼쪽 비트가 1일때를 음수로 생각하자는 것입니다.

그래서 8비트의 연산에서 양수는 00000000~01111111입니다. 0부터 127입니다.

음수는 11111111~10000000입니다. -1부터 -128입니다.

8비트 00000000에 1을 계속 더해보세요. 0부터 시작해서 1, 2, 3 ... 127이고 그 다음은 -128이고 그 다음은 -127 그 다음은 -126 ... -1 그리고 다시 0이 됩니다.

이렇게 이상한 음수 표시가 있을 수 있는 것은 연산의 범위가 8비트로 제한되기 때문입니다. 일반적인 수학에서 자릿수가 제한되지는 않지요. 자리수는 무한대까지 늘어납니다. 하지만 컴퓨터에서는 프로세서가 저장할 수 있는 비트의 수가 제한됩니다. 16비트 컴퓨터 8086은 16비트까지만 저장할 수 있습니다. 즉 자릿수가 무한대가 되지 않고 숫자가 계속 증가하다가 다시 0이 되는 순간이 오기 때문에 최상위 비트 MSB가 1일 때를 음수로 정할 수 있습니다.

이상하게 생각되시면 8비트 숫자 뭐든지 계산을 해보세요. 이상하게 들어맞습니다. 처음에는 이상할 수 있습니다. 어셈블리 프로그래밍을 오래하다보면 생기는 병중에 숫자의 시작을 0으로 생각하는 것과 127다음이 -128이라고 생각해서 덧셈을 이상하게 하는 것이 있습니다.

---

주1: 플립플롭이라는 디지털회로입니다. 디지털회로까지 알고싶으시면 전자공학까지 배워야합니다. 우리는 논리회로만 생각합니다. 논리회로에서는 1/0\(참/거짓\)만 생각합니다.

주2: 윈도우나 리눅스나 모두 계산기에 프로그래머용 모드가 있습니다. 16,10,8,2진수의 계산을 쉽게 할 수 있습니다. 또 윈도우의 계산기에는 Byte, Word, Dword, Qword 모드가 있습니다. 각각 8비트, 16비트, 32비트, 64비트를 의미합니다.

