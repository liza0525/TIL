# Chatbot Challenge(Python)_Day2

## 컴퓨터 조작하기

* 모듈 쓰는 방법은 구글링을 열심히 할 것

### 스크래핑

자주 확인하는 정보를 (자동) 스크랩하기 ex) 코스피 지수, 부동산 가격 등

현실에서 자주 확인하는 정보를 얻는 루틴을 생각해보기! -> 이것을 코드로 옮기는 것을 생각해볼 것

----

- 인터넷을 통해서 제공되는 모든 서비스는 공통적인 특징이 있다.

  : 검색을 하더라도 실질적으로 주소를 바꾸는 것이다!

- 서비스는 주문 또는 요청 주체인 고객(Client) -응답 주체인 서버(Server)의 패턴을 갖고 있다.
- 그럼 'Web Service'의 또다른 공통점은? 
  
  - 요청/응답되는 패턴이 같다! => URL(주소창의 주소) + 한 장의 문장에 불과

### Requests

- requests 모듈은 package를 받아야 한다. package 관리툴은 pip(git bash에서)
- requests.get(주소) : client가 server에게 주소의 서비스를 요청하는 것
- requests.get(주소).text : 해당 주소의 문서 내용
- requests.get(주소).status_code : 해당 주소의 http 상태 코드

### Beautiful Soup

- Python 입장에서 보이기 좋게 만드는 패키지(파싱)

- bs4.BeautifulSoup(requests.get(주소).text) : 예쁘게(?) 만들어진 문서

  - 이렇게 하면 python 입장에서는 보기 좋아져서 검색어 searching이 빨라진다.

- .select : 특정 내용을 뽑아내는 것

  .select_one : 특정 내용을 뽑아내는 것(한번만)

``` python
import requests
import bs4

url = "https://finance.naver.com/sise/"
response = requests.get(url).text # 되도록이면 변수명은 줄이지 말고 구독성 좋게

document = bs4.BeautifulSoup(response, 'html.parser') 
# html.parser라고 쓰면 error가 사라진다.

kospi = document.select_one("#KOSPI_now").text 
# 해당 CSS 선택자가 있는 곳을 찾아 그에 해당하는 text를 출력

kosdaq = document.select_one("#KOSDAQ_now").text
kospi200 = kosdac = document.select_one("#KPI200_now").text

print('현재 코스피 지수는 '+kospi+'입니다.')
print('현재 코스닥 지수는 '+kosdaq+'입니다.')
print('현재 코스피200 지수는 '+kospi200+'입니다.')
```

- Python이기 때문에 좋은 package를 이용하며, 타 언어보다 짧은 코드로 같은 결과를 낼 수 있다.

```python
import requests
import bs4


url = "https://www.naver.com/"
response = requests.get(url).text
document = bs4.BeautifulSoup(response, 'html.parser')

# firstRank = document.select_one("ul.ah_l:nth-child(5) > li:nth-child(1) > a:nth-child(1) > span:nth-child(2)").text
# print("현재 실시간 검색어 1위는"+firstRank+"입니다")
firstRank = document.select(".ah_k")
#select는 list 형태로 생긴다.
for i in firstRank:
    print(i.text)
```

### os

파이썬을 통해 파일 관리하는 package

- os.listdir() : 현재 폴더에 있는 파일의 리스트
- len(리스트) : 리스트 길이
- os.system(명령어) : command 창에 명령어를 입력한 결과와 같음
- os.chdir(파일명) : 현재 실행 중인 폴더의 (하위) 폴더로 이동
- os.rename('현재파일명','바꿀파일명') : 파일명 변경

```python
import os

# print(len(os.listdir()))

print(os.listdir())
os.rename('dog.py', 'hello.py')
print(os.listdir())
```

```python
import os

os.chdir('report')
for i in range(100):
    os.system(f'touch report{i}.txt')
    #i가 계속 바뀐다는 것을 표현하기 위해 앞에 f를 붙여준다
    #f string은 python 3.6부터 가능
    #삼성 sw 테스트에서는? os.system('touch report{}.txt'.format(i)) (format string이라고 한다)
```

```python
import os

os.chdir('report')

files = os.listdir()

for filename in files:
    os.rename(filename, 'samsung_'+ filename)
```

- replace는 string의 모듈을 쓸 것(os의 replace가 아님!)

```python
import os

os.chdir('report')

files = os.listdir()

for filename in files:
    os.rename(filename, filename.replace('samsung_','ssafy_'))
```

---

### 파이썬으로 파일 다루기

- open('파일명', '모드')  : 어떤 모드로 파일을 연다.
  - 모드에는 r(읽기),w(쓰기),a(추가)가 있다
- close : open과 함께 close는 꼭 써줘야 한다!!!

```python
f = open('ssafy.txt','w')
f.write('hello ssafy')
f.close() #stream이기 때문에 꼭 닫아줘야 한다.
```

```python
with open('ssafy.txt','w', encoding = 'utf-8') as f :
    for i in range(5):
        f.write('with ssafy 싸피와 함께\n')
#with ~ as ~ : 를 쓰면 close 하지 않아도 된다.
```

```python
with open('ssafy.txt','r', encoding = 'utf-8') as f:
    #result = f.read()
    result = f.readlines() # 한 줄 한 줄 읽어옴
    print(result)
```

---

### GIT BASH 명령어 모음

- pip list : python package 리스트를 확인
- pip install 패키지이름 : 해당 패키지를 설치
- mv 해당파일명 바꿀파일명 : 파일 이름 변경

---

### 그 외

- Requests 패키지는 3.5버전에서는 깔리지 않는다. 

  => Python 버전 up(ver 3.7)을 위해 환경 변수 편집을 할 것!

- pip로 설치하면 requests에 관련된 모든 패키지가 깔린다

- Response [200] : 응답이 잘 되었다.

- web에서 우클릭 > 요소검사 후 해당 라인 우클릭 > 복사 > CSS 선택자

- 코드 블럭 후 ctrl+/ => 주석 단축키

---

### github 모내기하기

<상시로 해봐야 하는 키워드>

- git status : 현재 폴더에 git에 올라가지 않은 파일이 있는지 없는지
- git log : 저장 내역



- git add . (현재 폴더에 새로 생긴 파일을 추가한다)
- git commit -m "2" (실질적인 저장 -m : 저장 메세지)
  - git commit한 후의 빔 화면에 들어가면 esc -> :wq 입력 후 탈출
- git push : 올라간다

---

- rm -rf .git 깃 지우기

---

### Git과 Github

1. git : 코드 관리 도구(라이너스 토르발스)
2. github : git을 이용한 웹서비스(여러 사람이 만듦/마이크로소프트에 팔림)



- clone : repository를 복제한다.

  - git clone github_repository_주소

- git pull (origin master)

- pull과 clone의 차이 

  : clone은 처음에 받아올 때만/pull은 github와 컴퓨터에서 생긴 차이점만 받아옴