# Chatbot Challenge(Python)_Day3

## 플라스크

1. 설치 및 실습

- http://flask.pocoo.org/

- pip로 설치, python library 중 하나 : pip install flask
- app.py라는 이름의 파일만 써야 한다 ->flask run
  - FLASK_APP=app2.py flask run (라고 쓰면 다른 이름의 파일을 써도 된다.)

- ```python
  from flask import Flask
  app = Flask(__name__)
  
  @app.route("/")
  def hello():
      return "Hello World!"
  ```

  위 코드 복사 붙여넣기 후 terminal에서 flask run 넣는다

- @app.route("/") : 주문 받는 방식(어떻게)

- def hello() : 무엇을 제공하는지(무엇을)

- ```python
  from flask import Flask
  app = Flask(__name__)
  
  @app.route("/hi")
  def hi():
      return "Hi!"
  ```

  http://127.0.0.1:5000/hi 주소에 들어가면 Hi!라는 문서를 보여준다.

- port : 가게의 문과 같은 것

  - 80 : main gate(http)
  - 443 : main gate(https)
  - 그 외의 포트는 자신이 정하는 것

- Routing : 주문이 들어오면 문서를 주는 길을 만드는 것

  - Variable Routing

  ```python
  from flask import Flask
  app = Flask(__name__)
  
  @app.route("/hello/<person>")
  def hellolizzie(person):
      return "Hello, "+ person
  	# 또는 return f"Hello {person}"
  ```

- 실습

  ```python
  # 1. /lotto =>랜덤한 로또 번호 추천
  # 2. /menu => 점심 메뉴 추천
  
  @app.route("/lotto")
  def lotto():
      numbers = range(1,46)
      lotto = random.sample(numbers,6)
      sortedLotto = sorted(lotto)
      return str(sortedLotto) #형변환 꼭 하시오
  
  @app.route("/menu")
  def menu():
      menu = ["김치찌개", "라면", "파스타", "떡볶이"]
      choice = random.choice(menu)
      return choice
  ```

  ```python
  from datetime import datetime #datetime package 속의 datetime class만 쓸거야
  
  @app.route("/newyear")
  def newyear():
      today = datetime.now()
      #만약 오늘이 1월 1일이라면, 예
      #아니면, 아니요
  
      if (today.month == 1) and (today.day == 1):
          return '예'
      else:
          return '아니요'
  
  ```

  

2. 주문서

- Protocol(어떤 방식으로 보내는지 약속, 통신규약) + Subdomain + Domain(Host name) + Top level domain(TLD) + Path



## 실습

- flask를 이용하여 html 문서 받아오기
  - render_template를 import

 ```python
from flask import Flask, render_template
import random
app =  Flask(__name__)

@app.route("/hello<name>")
def hello(name):
    #name에는 /hello/이름/ 활용 가능
    return render_template('hello.html', name=name) #앞의 name이 html에 보낼 변수

####서버를 껐다 켜지 않아도 reload가 쉽게
if __name__ == "__main__":
    app.run(debug=True) 
 ```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>안녕하세요, {{ name }}님</h1>
</body>
</html>
```

- 랜덤 메뉴 정하기
  - '쌍이 묶음으로 보내진다'라고 생각되면 Dictionary를 쓰도록 한다.

```python
from flask import Flask, render_template
import random
app =  Flask(__name__)

@app.route('/menu')
def menu():

    name = random.choice(['짜장면','떡볶이','스테이크','초밥'])
    
    lunchMenu = {
        '짜장면':'http://ojsfile.ohmynews.com/STD_IMG_FILE/2016/1214/IE002069160_STD.jpg',
        '떡볶이':'https://pbs.twimg.com/media/DzUpUB5U0AA65cE.jpg',
        '스테이크':'http://kommyhmall.com/wp-content/uploads/2017/06/%E1%84%89%E1%85%B3%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%8F%E1%85%B3.jpg',
        '초밥':'https://post-phinf.pstatic.net/MjAxOTAyMDhfMjYz/MDAxNTQ5NTk5MzIyMzQ0.-2qF-LcU_KlvpbYCWT67C3yC6M2uAIB16iE_mvqAu5Ig.t29UhvdLpmUHOcEnnyWlmF4sOyDrb6iWGtLo_Vx5cQUg.JPEG/%EC%96%B4%EB%A9%94%EC%9D%B4%EC%A7%95_%EB%8D%A4_%EC%B4%88%EB%B0%A5.jpg?type=w1200'
    }

    return render_template('menu.html', name=name, image=lunchMenu[name])
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>오늘의 점심 메뉴는 {{ name }}입니다.</h1>
    <br>
    <img src= "{{image}}">
</body>
</html>
```

- 랜덤 숫자 추천 후 최신 로또와 비교하여 등수 계산하기
  - JSON 파일을 먼저 파싱한다.

```python
@app.route("/lotto")
def lotto():

    url='https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=866'
    res = requests.get(url)

    dict_lotto = res.json()
    your_lotto = sorted(random.sample(range(1,46),6))
    same = 0
    rank = '1'

    winner = []
    for i in range(1,7):
        winner.append(dict_lotto[f'drwtNo{i}'])


    for i in range(6):
        for j in range(6):
            if winner[i] == your_lotto[j]:
                same = same + 1

    if(same == 6):
        rank = '1등'
    elif(same == 5):
        rank = '3등'
    elif(same == 4):
        rank = '4등'
    elif(same == 3):
        rank = '5등'
    else:
        rank = '꽝'


    return render_template('lotto.html', your_lotto=your_lotto, rank=rank, winner=winner)

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>추천 번호 : {{ your_lotto }}</h1><br>
    <h1>최신 당첨번호 : {{ winner }}</h1><br>
    <h1>등수 : {{ rank }}</h1><br>
</body>
</html>
```



## HTML

- <html></html>

- <head></head>

- <body></body>

- <a href = "주소"></a>

- <img src = "이미지 주소">

---

### 그 외

- John Resig(존 레식) : 매일 배운 것을 올리겠다. 
  - github 구경해볼 것
  - Khan Academy Programmer
- 초보몽키 개발공부 블로그
  - TIL 관련
- SendBird : 메신저

- 개발자로서의 커리어는 대기업뿐만이 아니라 중견, 스타트업에서도 쌓을 수 있으니 

  **눈을 높히지 말고 넓힐 것!**

- Y combinator
- SW는 End product가 매우 유연하다.
  - 대신 가장 중요한 건 **Speed** => 만들어 놓고 꾸준한 업데이트를 하면 된다!

- 뭐든 시작하면 꼭 마무리를 지어보자
- UC Berkeley Software Engineering(cs169) : 소프트웨어 공학 수업(데이빗 A 패터슨 - 튜링 어워드 수상자)
  - edx에서 들을 수 있음
  - 강동주 교수님의 수업 철학에 많은 영감을 줬음
  - Agile Development Using Ruby on Rails

- **프로그래머스 https://programmers.co.kr/** : 개발자 채용, 매칭 사이트 (대표 이확영 : 네이버/카카오 CTO)
  - 최소 50억 이상 투자 받은 곳으로 가십시오(모험 ㄴㄴ)
  - https://programmers.co.kr/learn/challenges 코딩 테스트 연습!
- 공식문서 보는 습관을 기를 것
- tim berners lee olympics 2012 (Inventor WWW) youtube 검색 :)

### GIT BASH

---

- pwd : 현재 폴더

- / : 최상위 폴더(root)

- ~ : home 폴더

