# Chatbot Challenge(Python)_Day4

-  Static Web Page : 사람과 상관 없이 정적으로 보여주는 페이지
- Dynamic Web Page : 여러가지 서비스를 동적으로 보여주는 페이지

- 배포 후에 타인에게 보여줄 수 있음
  - Dynamic page는 배포가 어렵지만 Static page는 배포가 쉬움(github을 이용하여)

- 사용자의 입력값을 받아 활용하는 것을 오늘 시간에 할 것임

---

### Fake Google 만들기

- form 태그 : 정보를 받아서 다른 페이지에 넘기는 태그
- python faker => fake 데이터를 많이 만들어 주는 패키지
  - pip install Faker

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route("/")
def home():
    return render_template('home.html')
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>검색엔진</title>
</head>
<body>
    <h1>뭐든 검색해보세용 헤헤</h1>
    <img width="400" height="150" src="http://image.itdonga.com/files/2013/12/09/1.png">
    <form action="https://www.google.com/search">
        <input name="q">
        <button>구글로 검색</button>
    </form>
    <br>
    <img width="400" height="150" src="https://t1.daumcdn.net/cfile/tistory/99C632335A2A72A918">
    <form action="https://search.naver.com/search.naver">
        <input name="query">
        <button>네이버로 검색</button>
    </form>
    <br>
    <img width="400" height="150" src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Daum_communication_logo.svg/1200px-Daum_communication_logo.svg.png">
    <form action="https://search.daum.net/search">
        <input name="q">
        <button>다음으로 검색</button>
    </form>
</body>
</html>

```

---

### 전생앱 만들기

```python
from flask import Flask, render_template, request
from faker import Faker

names={}

app = Flask(__name__)
fake = Faker('ko_KR')

@app.route('/pastlife')
def pastlife():
    return render_template('pastlife.html')

@app.route('/result')
def result():
    name=request.args.get('name') #name이라는 그릇 안에 있는 사용자의 입력값만 갖겠다는 함수

    if name in names :
        jobs = names[name]
    else:
        jobs = fake.job() 
        names[name] = jobs #names 딕셔너리에 새로운 data를 넣는 문법


    # 1. names에 해당하는 이름이 있는지 없는지 확인

    # 2. 없다면 => 랜덤으로 fake 직업을 보여줌, dictionary에 저장

    # 3. 있다면 => names에 저장된 직업을 보여줌

    #dictionary
    return render_template('result.html', jobs=jobs, name=name)

if __name__ == '__main__':
    app.run(debug=True)
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>전생앱 :: 당신의 전생을 알려드립니다 :)</title>
</head>
<body>
    <h1>당신의 전생은!?!?!?</h1>
    <form action="/result">
        <p>당신의 이름은?</p>
        <input name="name">
        <button>확인</button>
    </form>
</body>
</html>
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
    <h1>{{ name }}님의 전생은 '{{ jobs }}'(이)었습니다.</h1>
</body>
</html>
```

---

### Fake 궁합 사이트 만들기

```python
@app.route('/destiny')
def destiny():
    p1=request.args.get('p1')
    p2=request.args.get('p2')

    if p1+p2 in nameGroup:
        yourPer = nameGroup[p1+p2]
    else:
        yourPer = random.randint(51,100)
        nameGroup[p1+p2] = yourPer
        nameGroup[p2+p1] = yourPer


    return render_template('destiny.html', p1=p1, p2=p2, yourPer=str(yourPer))
```

- 두 단어의 조합으로 키를 써도 된다.
- dictionary in dictionary로 해도 된다.
- 이중 dictionary(dict in dict)

```python
if p1 in nameGroup:
    if p2 in nameGroup[p1]:
       yourPer = nameGroup[p1][p2]
    else:
        yourPer = random.randint(51,100)
        nameGroup[p1][p2] = yourPer
else:
    yourPer = random.randint(51,100)
    nameGroup[p1] = {p2:yourPer}	
```

- HTML 파일

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>두구두구두굳구 궁합</title>
</head>
<body>
    <h1>궁합을 알려드립니다!</h1>
    <form action="/destiny">
        <p>당신의 이름</p>
        <input name="p1">
        <p>그분의 이름</p>
        <input name="p2"><br>
        <button>확인하기</button>
    </form>

</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>쨔잔 궁합 결과~!</title>
</head>
<body>
    <h1>★{{ p1 }}님과 {{ p2 }}님의 궁합은 {{ yourPer }}%입니다★</h1>
</body>
</html>
```

---

- 딕셔너리

```python
for k,v in nameGroup.items(): # key, value를 뽑는 방법
list(nameGroup.keys()) # key만 뽑는 방법
list(nameGroup.values()) # value만 뽑는 방법
sum(score.values()) # score 딕셔너리 내의 value를 모두 더할 수 있음!
```

