#매크로 함수 만들기
이전에 설명했듯이 매크로는 함수처럼 코드가 호출되는게 아니라 코드가 복사되서 들어갑니다. C의 preprocessor나 인라인 함수와 같습니다.

매크로는 이렇게 만듭니다.
```
name    MACRO  [parameters,...] 

            

ENDM
```

MACRO라는 키워드로 매크로를 선언하고 인자들도 지정할 수 있습니다.

예를 보면

```
MyMacro    MACRO  p1, p2, p3

     MOV AX, p1
     MOV BX, p2
     MOV CX, p3

ENDM

ORG 100h

MyMacro 1, 2, 3

MyMacro 4, 5, DX

RET
```

이렇게 매크로를 만들어서 쓸 수 있습니다. 함수처럼 call을 쓸 필요없이 매크로 이름만 쓰면 됩니다. 매크로는 어셈블러의 키워드이지 프로세서의 명령어가 아니니까요.

매크로가 어셈블되면 이렇게 됩니다.
```
MOV AX, 00001h
MOV BX, 00002h
MOV CX, 00003h
MOV AX, 00004h
MOV BX, 00005h
MOV CX, DX
```
대강 어떻게 쓰는지는 간단합니다. 그런데 왠지 함수를 만드는 것보다 더 고급스럽고 더 함수같아 보이는건 기분탓입니다 ;-)  emu8086말고 최신 어셈블러를 쓰면 함수를 만들 때도 함수 인자를 지정하거나하는 기능을 제공합니다. 그래서 거의 C처럼 어셈블리 프로그래밍을 할 수 있습니다.

만약에 매크로 내부에서 점프를 해야될 일이 있다면 label을 어떻게 만들어야할까요? 매크로는 코드의 반복이므로 같은 이름의 label도 반복적으로 나타나게되서 어셈블러에서 에러가 납니다. 그럴때는 다음과 같이 local이라는 키워드를 사용하면 됩니다.
```
MyMacro2    MACRO
    LOCAL label1, label2

    CMP  AX, 2
    JE label1
    CMP  AX, 3
    JE label2
    label1:
         INC  AX
    label2:
         ADD  AX, 2
ENDM


ORG 100h

MyMacro2

MyMacro2

RET

END
```