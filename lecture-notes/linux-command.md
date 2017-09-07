layout: true
.top-line[]

---
class: center, middle

# Linux Command

---
## Kernel & Shell
* Kernel(커널)
    - 커널은 리눅스 운영체제의 핵심으로 하드웨어를 관리하고 응용 프로그램의 실행 환경을 제공
* Shell(쉘)
    - 사용자가 리눅스 운영체제를 사용하기 위한 인터페이스로 명령을 입력하고 명령의 결과를 확인함
    - 대표적인 쉘 프로그램으로 Bash가 있음
* 만일 사용자가 ls(파일 목록 보기) 명령을 실행하면
    - 쉘에서 ls라고 사용자가 입력하면 쉘이 ls라는 프로그램을 찾아서 커널에게 실행(exec)을 요청(시스템 콜)
    - 커널은 exec요청에 따라 ls프로그램을 실행
    - ls가 실행되면 ls는 필요에 따라 여러 요청(시스템 콜)을 커널에게 하게 됨

---
## File & Process
* 리눅스에서는 모든 것을 파일로 추상화하여 접근 함
    - 실제로 파일이 아닌데, 파일 처럼 open/read/write/close 할 수 있음
    - 디렉토리, 하드웨어 디바이스 등
* 프로세스
    - 프로세스는 프로그램이 실행되어 커널에 의해 관리되고 있는 것을 말함
    - 프로그램 하나가 여러 번 실행되어 여러 프로세스가 생길 수 있음

---
## 파일 & 디렉토리 탐색 명령
.left-column-50[
* ls: 파일 목록 보기
* cd: 디렉토리 변경
* mkdir: 디렉토리 생성
* rmdir: 디렉토리 삭제
* pwd: 현재 디렉토리 보기
* 특별한 디렉토리
    - .  현재 디렉토리를 의미
    - .. 상위(부모) 디렉토리를 의미
    - ~  홈 디렉토리를 의미
]
.right-column-50[
```bash
*jyheo@JYHEO-LAPTOP:~$ ls
android-lecture  doc  test.py
*jyheo@JYHEO-LAPTOP:~$ mkdir temp
*jyheo@JYHEO-LAPTOP:~$ cd temp
*jyheo@JYHEO-LAPTOP:~/temp$ ls -al
total 4
drwxrwxrwx 0 jyheo jyheo 512 Sep  7 13:58 .
drwxr-xr-x 0 jyheo jyheo 512 Sep  7 13:58 ..
*jyheo@JYHEO-LAPTOP:~/temp$ cd ..
*jyheo@JYHEO-LAPTOP:~$ ls -al ~/temp
total 4
drwxrwxrwx 0 jyheo jyheo 512 Sep  7 13:58 .
drwxr-xr-x 0 jyheo jyheo 512 Sep  7 13:58 ..
*jyheo@JYHEO-LAPTOP:~$ rmdir temp
*jyheo@JYHEO-LAPTOP:~$ ls -a
.   android-lecture  .bash_logout  doc       .python_history            test.py
..  .bash_history    .bashrc       .profile  .sudo_as_admin_successful  .viminfo
*jyheo@JYHEO-LAPTOP:~$ pwd
/home/jyheo
```
]

---
## 파일 조작 명령
.left-column-50[
* cp: 파일 복사
* mv: 파일 이동 또는 이름 변경
* rm: 파일 삭제
* cat: 파일 내용 출력
* less: 파일 내용을 한 페이지씩 출력
* head: 파일의 앞 10줄만 출력
* tail: 파일의 뒤 10줄만 출력
* grep: 파일에 특정 문자열(패턴)을 찾아서 보여줌
* wc: 파일의 줄 수, 단어 수, 글자 수 통계
]
.right-column-50[
```bash
jyheo@JYHEO-LAPTOP:~$ ls
android-lecture  doc  test.py
*jyheo@JYHEO-LAPTOP:~$ cp ./doc/raspberry/lecture-notes/linux-command.md .
jyheo@JYHEO-LAPTOP:~$ ls
android-lecture  doc  linux-command.md  test.py
*jyheo@JYHEO-LAPTOP:~$ mv linux-command.md lixcmd.md
jyheo@JYHEO-LAPTOP:~$ ls
android-lecture  doc  lixcmd.md  test.py
*jyheo@JYHEO-LAPTOP:~$ cat lixcmd.md
*jyheo@JYHEO-LAPTOP:~$ less lixcmd.md
*jyheo@JYHEO-LAPTOP:~$ head lixcmd.md
*jyheo@JYHEO-LAPTOP:~$ tail lixcmd.md
*jyheo@JYHEO-LAPTOP:~$ grep "파일" lixcmd.md
 만일 사용자가 ls(파일 목록 보기) 명령을 실행하면
 리눅스에서는 모든 것을 파일로 추상화하여 접근 함
    - 실제로 파일이 아닌데, 파일 처럼 open/read/write/close 할 수 있음
 ls: 파일 목록 보기
*jyheo@JYHEO-LAPTOP:~$ wc lixcmd.md
 109  352 2701 lixcmd.md
*jyheo@JYHEO-LAPTOP:~$ rm lixcmd.md
```
]

---
## 와일드카드(* , ?)
* 와일드카드 * 는 모든 경우
    - ab* 라고 하면 ab로 시작하는 모든 파일을 의미, abc, abcde, abd 등
* 와일드카드 ? 는 한 글자에 대해서만 모든 경우
    - ab? 라고 하면 abc는 맞지만, abcd는 아님

```bash
jyheo@JYHEO-LAPTOP:~/temp$ touch ab abc abcd abcde abd
*jyheo@JYHEO-LAPTOP:~/temp$ ls ab*
ab  abc  abcd  abcde  abd
*jyheo@JYHEO-LAPTOP:~/temp$ ls ab?
abc  abd
*jyheo@JYHEO-LAPTOP:~/temp$ ls ?bc
abc
*jyheo@JYHEO-LAPTOP:~/temp$ ls *d
abcd  abd
*jyheo@JYHEO-LAPTOP:~/temp$ rm *
jyheo@JYHEO-LAPTOP:~/temp$ ls
jyheo@JYHEO-LAPTOP:~/temp$
```

---
## Redirection & Pipe
* Stanard In/Out(표준 입출력)
    - 프로그램이 printf()와 같은 함수로 출력을 하면 일반적으로 표준 출력으로 표시됨
    - 보통은 터미널 화면이 표준 출력으로 사용됨
    - 표준 입력은 scanf()와 같은 함수로 입력을 받을 때 사용하는 것으로 보통은 키보드 입력이 사용됨
* Redirection은 표준 입출력을 쉘에서 파일로 쉽게 변경해주도록 함
* program > file : program 표준 출력을 file로 저장(기존 file내용 삭제)
* program < file : program 표준 입력을 file의 내용으로 사용
* program >> file : program 표준 출력을 file에 추가하여 저장
* program1 | program2 : 파이프, program1의 표준 출력을 program2의 표준 입력으로 사용함

---
## Redirection & Pipe

```bash
*jyheo@JYHEO-LAPTOP:~$ ls -al > lsout
jyheo@JYHEO-LAPTOP:~$ cat lsout
*jyheo@JYHEO-LAPTOP:~$ ls >> lsout
jyheo@JYHEO-LAPTOP:~$ cat lsout
*jyheo@JYHEO-LAPTOP:~$ ls -al | wc
     14     123     805
*jyheo@JYHEO-LAPTOP:~$ wc < lsout
     18 127 839
jyheo@JYHEO-LAPTOP:~$ wc lsout
     18 127 839 lsout
```

---
## Process 관련 명령
* ps: 프로세스 목록을 보여줌
* kill: 프로세스에 시그널을 보냄
    - 시그널: 프로세스에게 어떤 신호을 보내는 방법
    - kill 프로그램으로 보낼 만한 시그널로는 SIGTERM, SIGKILL, SIGUSR 정도

```bash
*jyheo@JYHEO-LAPTOP:~$ ps
  PID TTY          TIME CMD
  112 tty2     00:00:02 bash
  330 tty2     00:00:00 ps
*jyheo@JYHEO-LAPTOP:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 12:30 ?        00:00:00 /init
jyheo        2     1  0 12:30 tty1     00:00:01 /bin/bash
jyheo      112     1  0 13:57 tty2     00:00:02 /bin/bash
jyheo      331   112  0 14:35 tty2     00:00:00 ps -ef
*jyheo@JYHEO-LAPTOP:~$ kill -SIGKILL 112
```

---
## 파일 텍스트 정렬/편집
* sort: 입력 파일을 정렬하여 출력함
* find: 특정 조건의 파일을 찾음
* sed: 스트림 에디터, 파일의 내용을 편집하는 명령어들을 입력으로 주어 파일을 편집함
    - 예를 들어 어떤 파일에서 ABC라는 문자열을 DEF로 바꾼다.
    - 파일의 특정 위치에 다른 파일을 끼워 넣는다.
* nano: 간단한 편집기, 명령어가 표시되므로 쉽게 사용 가능
* vi: 전통적인 오래된 편집기, 명령 모드와 편집 모드로 나뉘어져 있음

---
## 파일 텍스트 정렬/편집
```bash
jyheo@JYHEO-LAPTOP:~$ cat fruits
banana
apple
kiwi
*jyheo@JYHEO-LAPTOP:~$ sort fruits
apple
banana
kiwi
*jyheo@JYHEO-LAPTOP:~/doc$ find -name "*.md" -print
./android/android-lecture/labs/actionbar-navigation-lab.md
./android/android-lecture/labs/activity-intent-lab.md
./android/android-lecture/labs/adapter-view-lab.md
./android/android-lecture/labs/android-intro-lab.md
... 생략
```

---
## Help
* whatis: 프로그램의 용도를 알려줌
* man: 프로그램의 자세한 사용 매뉴얼
* apropos: 특정 기능을 하는 프로그램을 찾아줌

```bash
*jyheo@JYHEO-LAPTOP:~/doc$ whatis cp
cp (1)               - copy files and directories
*jyheo@JYHEO-LAPTOP:~/doc$ man cp
*jyheo@JYHEO-LAPTOP:~/doc$ apropos rename
file-rename (1p)     - renames multiple files
File::Rename (3pm)   - Perl extension for renaming multiple files
git-mv (1)           - Move or rename a file, a directory, or a symlink
lvrename (8)         - rename a logical volume
mv (1)               - move (rename) files
prename (1)          - renames multiple files
rename (1)           - renames multiple files
rename.ul (1)        - rename files
vgimportclone (8)    - import and rename duplicated volume group (e.g. a hardware snapshot)
vgrename (8)         - rename a volume group
```

---
## References
* http://www.ee.surrey.ac.uk/Teaching/Unix/
