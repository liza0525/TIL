# Chatbot Challenge(Python)_Day1

## Python 기초

```python
print("hello")
import radom
random.sample()
```

* 마크다운 사용 방법!

---



1. 파이썬의 기초문법

- Python에서는 홑따옴표 또는 곁따옴표를 써줘야 '문자열'이라는 것을 표현할 수 있다.

- 저장 : 박스(변수)를 만들고 그 곳에 값을 넣는 것
  - ex) dust = 60 : dust라는 변수에 60을 저장한다. (수학에서의 등호와 같게 생각하면 안된다!)
  - 무엇을 저장하는가 : 현실에 존재하는 모든 숫자 /  글자(따옴표를 꼭 붙인다.) / boolean (참, 거짓 / binary에서 유래한 자료형)
  - 직접 값을 쓰지 말고 꼭 박스(변수)에 넣고 활용하는 것을 하도록 한다.

- 리스트 : 박스의 리스트, 박스가 순서대로 여러 개가 붙어있다.

  ```python
  dust = [58, 40, 70]
  print(dust[1])
  ```

  - 그러나 리스트의 순서를 알지 못하면 원하는 값을 구하기 어렵다.

    -> 딕셔너리(dictionary) : 이름표과 값이 pair 되어 있음

- 딕셔너리 : 궁극의 박스 "견출지 붙인 박스들의 묶음"

  ```python
  dust = {"강남구" : 58, "서초구" : 40}
  print(dust["강남구"])
  ```

  -> 값은 58이 출력됨

  - 보통 쓸 때 가 값마다 줄을 띄워준다.

- 조건 if/else

  ```python
  if dust>50:
      print("50초과")
  else:
      print("50이하")
  ```

  - python에서는 4 space가 기본 문법이다!

- 반복문 while/for

  - python에서는 반복문으로 while을 잘 쓰진 않는다.

    특정한 자료(리스트)를 일부분을 출력할 때 쓰는 편

  - while문

    ```python
    n=0
    
    while n<3 :
    	print("출력")
    	n=n+1 # 종료조건
    ```

  - for문 : for는 in과 함께 pair 된다. 여러 개가 있는 박스들을 한번씩 순회한다. i 대신 다른 변수를 써도 상관 x

    ```python
    for i in List:
    	print("정해진 범위 안에서 계속")
    ```

    ```python
    for name in friends:
        print(name)
    ```

    - 위 경우 print(friends[name]) 라고 작성한 경우 오류가 뜬다.

---

### API : 다른 시스템 간의 커뮤니케이션 방식

​		(응용 프로그램 프로그래밍 인터페이스 Appliucation Programing/Progamable Interface)

* 인터페이스란? 제품에 다가갈 수 있는  통로(전면) ex) 금성티비 vs. 쿡티비

  오픈소스 활용 : 함수(function)를 이용하자!

- 제작자의 권리를 지키면서 누구나 열람가능한 공개된 (소스)코드

- 함수 : input을 어떤 function에 넣어 output을 내는 것

- python의 함수 : 내장함수/외장함수

  - 내장함수 : 기본적으로 프로그램에 포함되어 있는 함수

  - 외장함수 : 외부에서 가져와서 쓰는 함수 : import로 외부 모듈을 가지고 온다

    ​		ex) random.choice(리스트)

    ​			  random.sample(리스트, 샘플링할 갯수) : 비복원 무작위 표본 추출하는 것(샘플링)

    - range(숫자1, 숫자2) : 숫자1 이상, 숫자2 미만의 숫자를 리스트에 차례대로 넣는다.

      ```python
      numbers=range(1,46) # 1 이상 46 미만의 숫자를 numbers라는 리스트에 차례대로 넣는다.
      ```

    - sorted(리스트) : 리스트를 오름차순으로 sort를 한다.

---

### CLI(Command Line Interface )

​		: 유닉스,  CP/M, DOS의 command.com 등을 일컫는다.

1. Git Bash

- ~ : home directory를 의미함 (pwd라고 하면 경로가 나온다)
- python --version : 현재 python 버전을 나타냄

![1562568321651](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562568321651.png)

- `	ls` : 폴더 안에 있는 내용물을 보는 명령어
- `cd` : 디렉토리 변경
- `cd ..` : 상위 폴더로 변경
- `pwd` : print working directory 현재 작업중인 디렉토리를 나타냄
- `code .` : 현재 폴더에서 code editor 열기
- `touch` : 파일 만들기
- `rm` : 파일 지우기(remove)
- `exit` : terminal 창 끄기
- tap 키 : 자동완성



2. VS code로 git bash 설정 후 코드 작성

- ctrl+shift+p -> shell 검색 -> select default shell -> git bash 선택

  : 터미널  default를 git bash로

- 왼쪽 창에 python에 마우스를 올리면 new file -> 파이썬 파일을 생성(.py) -> 우측 하단에 생기는 모든 창에 install 누른다.

- 코드 실행 
  - 우클릭 후 Run python file in terminal 선택 
  - git bash terminal 띄우기 : ctrl+shift+` -> python 파일명 입력
  - shift+enter : 줄 실행

---

## 실습

```python
import webbrowser

url = "http://www.google.com"
webbrowser.open(url)

#google 창 열기
```

```python
import webbrowser

url = "https://search.daum.net/search?q="
keywords = ["아이유", "수지", "설현"]

for name in keywords:
    webbrowser.open(url+name)
    
# 열고 싶은 검색어 창 한꺼번에 열기 : 리스트 작성 후 for문으로 돌린다.
```

---

## github에 모내기하는 법(순서만)

- git init
- git add .
- git commit -m "1" (실질적인 저장)
- git config --global user.email "double.y.0525@gmail.com" (git 계정 메일)
- git config --global user.name "Yunyoung Chung"
- git commit -m "1" 한번 더
- (repostory 만든 후 나온) git remote add origin https://github.com/liza0525/chatbot_python.git 
- git push -u origin master(처음으로 해당 레퍼지토리에 push할 때만)
- 로그인 창 뜨면 로그인 하면 된다 -> 바로 모내기 가능