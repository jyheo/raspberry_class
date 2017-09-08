# Linux Command 실습

## VirtualBox 설치

## VirtualBox에 Ubuntu 설치

## Ubuntu 터미널에서 다음을 수행하시오.

* 현재 수행중인 프로세스 **전체** 의 수를 출력하시오.
    - 힌트: ps, wc, 파이프
* curl 프로그램을 이용하여 http://www.ee.surrey.ac.uk/Teaching/Unix/ 의 내용을 받는다.
```bash
$ curl http://www.ee.surrey.ac.uk/Teaching/Unix/ > unix.html
```
    - 만일 curl이 없다면 apt install로 설치한다. sudo와 apt 명령에 대해 조사해본다.
```bash
$ sudo apt install curl
```
* unix.html에 포함된 글자 수, 단어 수, 줄 수를 알아보시오.
* unix.html에서 <li> 태그가 있는 라인만 모아서 li.html 파일을 만드시오.
* unix.html에서 모든 태그를 제거한 파일 unix_content.html 파일을 만드시오.
    - sed로 태그 제거 방법
```bash
    $ sed -e 's/<[^>]*>//g' unix.html
```
* unix_content.html에서 공백 라인 제거
    - 힌트: 공백 라인을 골라내는 grep 사용, 공백 라인만 나옴
```bash
$ grep '^$' unix_content.html
```

## 실습 결과 보고서를 업로드 한다.
