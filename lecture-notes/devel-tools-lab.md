# 개발 도구 실습

## 실습 업로드
* webbrowser로 https://oslab.pythonanywhere.com/ 접속하여 업로드
* curl을 써서 업로드
    - curl -u 아이디:패스워드 -F 'file=@test.txt' https://oslab.pythonanywhere.com/
* 업로드한 파일 확인
    - curl -u 아이디:패스워드 http://oslab.pythonanywhere.com/uploads/uploaded_filename.ext
* 아이디/패스워드는 수업 시간에 공개함

## 실습 내용
* 강의 자료에 나오는 hellomake 예제의 소스(\*.c, \*.h) 위치와 생성 파일(\*.o, hellomake)의 위치를 다음과 같이 한 후에 정상적으로 make가 되도록 Makefile을 수정하시오. 불필요하게 반복적으로 src, objs를 쓰지 않도록 않도록 변수를 활용하시오.
    - Makefile
    - src/hellomake.c
    - src/hellofunc.c
    - include/hellofunc.h
    - objs/hellomake.o (오브젝트 코드)
    - objs/hellofunc.o (오브젝트 코드)
    - hellomake (최종 타겟, 실행 파일)
    - 힌트: make를 하면 다음과 같이 실행이 되어야 함
```bash
gcc -c -Iinclude -o objs/hellomake.o src/hellomake.c -Wall
gcc -c -Iinclude -o objs/hellofunc.o src/hellofunc.c -Wall
gcc -o hellomake objs/hellomake.o objs/hellofunc.o
```
* printf 함수를 포함하고 있는 공유 라이브러리(.so)를 찾아서, 해당 라이브러리가 제공하는 함수의 총 개수를 구하시오.
    1. printf를 사용하는 프로그램을 만든다.(a.out)
    2. a.out이 사용하는 공유 라이브러리를 찾는다. 힌트: ldd
    3. 공유 라이브러리들이 어떤 함수(symbol)를 export하는지 찾아본다. 힌트: readelf
    4. 나머지 과정 진행
* gdb 실습: 아래 stack.c 파일을 디버그 정보를 포함하여 컴파일 하고 다음 명령어들을 연습해보고, 역할에 대해 이해하고 설명하시오.
    * https://raw.githubusercontent.com/jyheo/raspberry_class/master/lecture-notes/stack.c

```bash
jyheo@JYHEO-LAPTOP:~/devtools$ gdb stack
GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.5) 7.11.1
...중간 생략...
(gdb) break push
Breakpoint 1 at 0x40060d: file stack.c, line 41.
(gdb) run
Starting program: /home/jyheo/devtools/stack

Breakpoint 1, push (data=3) at stack.c:41
41         if(!isfull()) {
(gdb) print top
$1 = -1
(gdb) next
42            top = top + 1;
(gdb) next
43            stack[top] = data;
(gdb) next
47      }
(gdb) print top
$2 = 0
(gdb) print stack
$3 = {3, 0, 0, 0, 0, 0, 0, 0}
(gdb) clear push
Deleted breakpoint 1
(gdb) next
main () at stack.c:52
52         push(5);
(gdb) next
53         push(9);
(gdb) print stack
$4 = {3, 5, 0, 0, 0, 0, 0, 0}
(gdb) step
push (data=9) at stack.c:41
41         if(!isfull()) {
(gdb) print top
$5 = 1
(gdb) continue
Continuing.
Element at top of the stack: 15
Elements:
15
12
1
9
5
3
Stack full: false
Stack empty: true
[Inferior 1 (process 569) exited normally]
(gdb) quit
```
