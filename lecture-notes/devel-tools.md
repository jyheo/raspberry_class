layout: true
.top-line[]

---
class: center, middle

# GCC와 Makefile

---
## Contents
* GCC
* 디버깅
* Makefile
    - Makefile 단계1
    - Makefile 단계 2
    - Makefile 단계 3(Clean)
    - Makefile 최종
* 프로그램 분석 도구

---
## GCC
* 세상에 C 컴파일러 종류가 매우 다양함
* 대표적인 오픈 소스 C 컴파일러인 GCC(GNU C Compiler)
* GCC는 여러 운영체제에서 사용 가능한 강력한 컴파일러
    - 우리가 주로 사용하는 윈도우와 리눅스, Mac OS X에서도 모두 사용 가능함
* C++도 지원함
* 참고로 윈도우에서 사용 가능한 방법
    - GCC만 설치하길 원한다면 MINGW http://www.mingw.org/
    - Cygwin을 설치하고 GCC를 설치하는 방법 http://cygwin.com/
    - 최근에는 Windows 10의 Bash on Ubuntu on Window에서 gcc가 사용 가능함
* 본 강의 자료에서는 Ubuntu를 가정함

---
## GCC
* GCC 버전
```bash
*jyheo@JYHEO-LAPTOP:~$ gcc --version
gcc (Ubuntu 5.4.0-6ubuntu1~16.04.4) 5.4.0 20160609
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

---
## 컴파일
* 자신이 좋아하는 에디터를 하나 사용해서 hello.c 파일을 만들어 둔다.

```c
#include <stdio.h>

int main()
{
        printf("Hello, World\n");
        return 0;
}
```

---
## 컴파일

```bash
jyheo@JYHEO-LAPTOP:~/devtools$ ls
hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ gcc hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ ls
a.out  hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ ./a.out
Hello, World
```

* gcc hello.c
    - hello.c 라는 이름의 C 파일을 컴파일하고 링크해서 실행파일을 생성
    - 만든 실행파일의 이름은 a.out (MINGW GCC는 a.exe)
* 컴파일과 링크?

---
## 컴파일

```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -o hello hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ ls
a.out  hello  hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ ./hello
Hello, World
```

* gcc -o hello hello.c
    - hello라는 이름으로 실행파일을 만들어라!

---
## 컴파일

```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -c hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ ls
a.out  hello  hello.c  hello.o
```

* gcc -c hello.c
    - 링크는 하지 말고 hello.c를 컴파일만해서 오브젝트 코드(hello.o)를 생성

---
## 컴파일

```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -g -o hello.d hello.c
jyheo@JYHEO-LAPTOP:~/devtools$ ls -l
total 52
-rwxrwxrwx 1 jyheo jyheo 9616 Sep 21 07:39 a.out
-rwxrwxrwx 1 jyheo jyheo 8600 Sep 21 07:39 hello
-rw-rw-rw- 1 jyheo jyheo   73 Sep 21 07:33 hello.c
-rwxrwxrwx 1 jyheo jyheo 9616 Sep 21 07:39 hello.d
-rw-rw-rw- 1 jyheo jyheo 1504 Sep 21 07:37 hello.o
```

* gcc -g -o hello.d hello.c
    - 디버그 정보를 포함하여 hello.d라는 이름으로 실행파일을 생성

---
## 컴파일
* gcc의 옵션은 엄청나게 많지만 일단은 이것만 알고 넘어가자.
    - $ man gcc
* 추가로
    - W : 컴파일 경고 옵션, 보통 -Wall 모든 경고 옵션을 켬
    - O : 최적화 옵션, 보통 -O2를 많이 사용

---
## 디버깅
* gdb: gcc와 함께 자주 사용되는 디버깅 툴
* 디버깅 정보를 포함시키도록 -g 옵션을 주고 컴파일해야 디버깅이 쉬움
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -g -o stack stack.c
jyheo@JYHEO-LAPTOP:~/devtools$ gdb stack
GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.5) 7.11.1
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from stack...done.
(gdb)
```
* gdb를 실행하면 (gdb) 프롬프트가 나타나고 여러 gdb 명령어로 디버깅을 할 수 있음


---
## Makefile
* Makefile은 소스 코드 컴파일을 효과적으로 하는 방법이다.
* 실제로 Makefile을 입력으로 하여 빌드를 수행하는 프로그램은 make임
* 출처: http://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/

---
## Makefile
* 간단한 예를 통해 살펴보겠다.
* hellomake.c와 hellofunc.c, hellofunc.h 로 되어 있는 프로그램 소스가 있다고 해보자.
* hellomake.c

```c
#include "hellofunc.h"

int main() {
    // call a function in another file
    myPrintHelloMake();

    return(0);
}
```

---
## Makefile
* hellofunc.c

```c
#include <stdio.h>
void myPrintHelloMake(void) {
    printf("Hello makefiles!\n");
}
```

* hellofunc.h

```c
// example include file
void myPrintHelloMake(void);
```

---
## Makefile
* 이 파일들을 컴파일하고 링크하기 위해서는 가장 간단히 다음과 같이 할 수 있다.
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -o hellomake hellomake.c hellofunc.c
jyheo@JYHEO-LAPTOP:~/devtools$ ./hellomake
Hello makefiles!
```
* gcc -o hellomake hellomake.c hellofunc.c
    - hellomake.c와 hellofunc.c 파일을 컴파일하고 두 파일을 링크하여 실행파일 hellomake를 생성
    - 이 과정을 나누어 실행하면 다음과 같다
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -c hellomake.c
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -c hellofunc.c
jyheo@JYHEO-LAPTOP:~/devtools$ ls hellomake.o hellofunc.o
hellofunc.o  hellomake.o
jyheo@JYHEO-LAPTOP:~/devtools$ gcc -o hellomake hellomake.o hellofunc.o
jyheo@JYHEO-LAPTOP:~/devtools$ ./hellomake
Hello makefiles!
```

---
## Makefile 단계1
* 이런 방식으로 컴파일/링크를 한다면…
    - 만일 파일이 두 개가 아니라 100개쯤 되고, 그 중에 한 파일을 수정했다고 하면 어떻게 해야 하는가?
    - gcc a1.c a2.c a3.c a4.c ...중간 생략... a100.c
* 매번 컴파일/링크 때마다
    - gcc a1.c a2.c a3.c a4.c ...중간 생략... a100.c 이렇게 써야 한다.
* 이를 좀 편리하게 해보자면
* Makefile 이란 이름의 파일을 하나 만들고 내용은 다음과 같이 한다.
    - **주의!: gcc 앞은 반드시 탭(TAB)을 입력해야 함**

```Makefile
hellomake.exe: hellomake.c hellofunc.c
        gcc -o hellomake hellomake.c hellofunc.c
```

---
## Makefile 단계1
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ rm hellomake
jyheo@JYHEO-LAPTOP:~/devtools$ make
gcc -o hellomake hellomake.c hellofunc.c
jyheo@JYHEO-LAPTOP:~/devtools$ ./hellomake
Hello makefiles!
jyheo@JYHEO-LAPTOP:~/devtools$ make
make: 'hellomake' is up to date.
```
* make
    - Makefile을 읽고 파일에서 설명한 작업을 수행한다.

---
## Makefile의 기본 형식
* Makefile은 다음과 같은 형식의 규칙들로 구성된다.
```
타겟: 의존 파일
[탭]        필요한 파일들을 사용하여 타겟을 만드는 명령어
```
* 타겟은 결과 파일을 쓴다. 즉, 이 파일을 생성하기 위해 필요한 파일(의존 파일)과 명령어를 기술 하는 것임
* 의존 파일은 타겟을 만드는데 필요한 파일, 즉, 이 파일들이 변경되면 make는 명령어를 수행하여 타겟을 생성한다. 하지만 이 파일들이 변경되지 않았고, 타겟도 이미 만들어져 있다면 아무 일도 하지 않는다.

---
## Makefile의 기본 형식
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ make
gcc -o hellomake hellomake.c hellofunc.c
jyheo@JYHEO-LAPTOP:~/devtools$ make
make: 'hellomake' is up to date.
```
* 처음에 make할 때에는 명령을 수행하지만, 두번째 make에서는 아무일 도 하지 않는다. 즉, 타겟이 이미 존재하고 필요한 파일들도 변경되지 않아서 타겟을 다시 만들 필요가 없기 때문임
* 앞의 예에서는
    - 타겟이 hellomake
    - 필요한 파일(의존 파일)은 hellomake.c와 hellofunc.c
    - 명령어는 gcc -o hellomake hellomake.c hellofunc.c

---
## Makefile 단계 2
```Makefile
CC=gcc
CFLAGS=-Wall

hellomake: hellomake.o hellofunc.o
        $(CC) -o hellomake hellomake.o hellofunc.o

hellomake.o: hellomake.c hellofunc.h
hellofunc.o: hellofunc.c hellofunc.h
```

* 이 파일에는 규칙이 3개가 있다. hellomake와 hellomake.o, hellofunc.o 이다.
* hellomake 타겟이 hellomake.o와 hellofunc.o를 필요로 하고, 각 .o는 .c와 hellofunc.h를 필요로 한다.
* .o 타겟이 .c를 의존하고 있으면 알아서 컴파일러를 실행시킴
???
* 이 파일에는 규칙이 3개가 있다. hellomake와 hellomake.o, hellofunc.o 이다.
* make는 별도로 규칙을 지정하지 않으면, Makefile에서 가장 먼저 나오는 규칙에 대해서만 처리한다.
* 타겟을 만들기 위해 필요한 파일(의존 파일)에 대한 규칙이 있을 경우 의존 파일의 해당 규칙에 대한 처리를 먼저 수행한다.
* 위에서는 hellomake를 만들기 위해 hellomake.o와 hellofunc.o가 필요한데, hellomake.o에 대한 규칙이 밑에 나오기 때문에 그 규칙을 먼저 처리한다.
* make는 타겟이 .o이고 필요파일(의존 파일)에 .c파일이 있는 경우 자동으로 C컴파일러를 실행한다. 즉, 명령어에 설명이 없어도 C 컴파일러를 실행함.
* 이때 CC변수와 CFLAGS 변수가 정의되어 있으면 사용한다.
* 위 예에서는 CC는 gcc를 사용하고 CFLAGS에는 -Wall (모든 warning을 켠다) 을 사용한다

---
## Makefile 단계 2
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ rm hellomake hellomake.o hellofunc.o
jyheo@JYHEO-LAPTOP:~/devtools$ make
gcc -Wall   -c -o hellomake.o hellomake.c
gcc -Wall   -c -o hellofunc.o hellofunc.c
gcc -o hellomake hellomake.o hellofunc.o
jyheo@JYHEO-LAPTOP:~/devtools$ make
make: 'hellomake' is up to date.
jyheo@JYHEO-LAPTOP:~/devtools$ touch hellofunc.h
jyheo@JYHEO-LAPTOP:~/devtools$ make
gcc -Wall   -c -o hellomake.o hellomake.c
gcc -Wall   -c -o hellofunc.o hellofunc.c
gcc -o hellomake hellomake.o hellofunc.o
jyheo@JYHEO-LAPTOP:~/devtools$ touch hellofunc.c
jyheo@JYHEO-LAPTOP:~/devtools$ make
gcc -Wall   -c -o hellofunc.o hellofunc.c
gcc -o hellomake hellomake.o hellofunc.o
```

---
## Makefile 단계 3(Clean)
```Makefile
CC=gcc
CFLAGS=-Wall

hellomake: hellomake.o hellofunc.o
        $(CC) -o hellomake hellomake.o hellofunc.o

hellomake.o: hellomake.c hellofunc.h
hellofunc.o: hellofunc.c hellofunc.h

.PHONY: clean
clean:
        rm hellomake hellomake.o hellofunc.o
```

* 오브젝트 코드와 최종 타겟을 지우는 clean 규칙을 넣어보자.
* clean이란 이름의 규칙은 실제로 타겟을 만드는 규칙이 아니기 때문에 .PHONY: clean을 넣어준다.
* 그러면 clean 규칙을 실행하려면 어떻게?
* make clean 이라고 하면 됨, 그러면 clean 규칙을 찾아서 실행하게 된다.

---
## Makefile 단계 3(Clean)
```bash
jyheo@JYHEO-LAPTOP:~/devtools$ make
make: 'hellomake' is up to date.
jyheo@JYHEO-LAPTOP:~/devtools$ make clean
rm hellomake hellomake.o hellofunc.o
jyheo@JYHEO-LAPTOP:~/devtools$ make
gcc -Wall   -c -o hellomake.o hellomake.c
gcc -Wall   -c -o hellofunc.o hellofunc.c
gcc -o hellomake hellomake.o hellofunc.o
```

---
## Makefile 최종
```Makefile
CC=gcc
CFLAGS=-Wall

ifeq ($(OS),Windows_NT)
  EXT=.exe
  RM=del
else
  EXT=
  RM=rm
endif

DEPS=hellofunc.h
TARGET=hellomake$(EXT)
OBJS=hellomake.o hellofunc.o

$(TARGET): $(OBJS)
        $(CC) -o $(TARGET) $(OBJS)

%.o: %.c $(DEPS)
        $(CC) -c -o $@ $< $(CFLAGS)

hellofunc.o: hellofunc.c hellofunc.h

.PHONY: clean
clean:
        $(RM) $(TARGET) $(OBJS)
```

---
## Makefile 최종
```Makefile
%.o: %.c $(DEPS)
        $(CC) -c -o $@ $< $(CFLAGS)
```
* 이건 뭔가?
    - 확장자가 .c인 모든 파일에 대해서 타겟을 .o로 만든다는 의미임
    - $@는 타겟, $<는 소스 파일을 나타냄
    - 별도로 규칙을 지정하지 않은 모든 .c 소스 파일들은 이 규칙을 따른다

---
## Makefile 최종
이상은 기초적인 수준의 Makefile이고, 실제로는 이보다 더 복잡한 경우도 많다.

---
## 프로그램 분석 도구
* strings - String
    - $ strings hellomake
* nm - Symbols(functions, global variables)
    - $ nm hellomake
* ltrace - Library call tracing
    - $ ltrace ./hellomake
* strace - System call tracing
    - $ strace ./hellomake
