# Chatbot Challenge(Python)_Day5

### 텔레그램 봇 만들기

---

- 텔레그램 다운로드 후 가입/로그인

- https://api.telegram.org/bot 자기가 받은 토큰/getMe 받아오기
  - 내 정보 받는 URL

![1562890889273](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562890889273.png)



- 토큰 같은 중요정보 숨길 때 
  - os단위로 숨겨둔다 
    - echo $PATH
    - python decouple : https://github.com/henriquebastos/python-decouple
    - pip install python-decouple 로 패키지를 깐다.
    - .env 파일을 생성
    - .env 파일 내에 중요 정보를 넣는데
    - .gitignore 파일을 만들고, 그 내에 git으로 관리 안 할 파일 또는 폴더명을 적는다.

---

### Web Hook

- https://core.telegram.org/bots/api
- setWebhook 주소 : https://api.telegram.org/bot872944257:AAE3j3PPKBmTsfgKaeTU3zc6TEAWoefD7Hs/setWebhook?url=https://6836fba8.ngrok.io/872944257:AAE3j3PPKBmTsfgKaeTU3zc6TEAWoefD7Hs
- deleteWebhook 주소 :  https://api.telegram.org/bot872944257:AAE3j3PPKBmTsfgKaeTU3zc6TEAWoefD7Hs/deleteWebhook

---

### 기타

- Webhook을 하기 위해 https://ngrok.com/ 회사 매니지먼트 어플리케이션
- 프로그램 다운받아서 ngrok 프로그램을 User/student 폴더에 넣는다.(더블클릭 금지)
- ngrok은 cmd로 실행할 것 -> ngrok http 5000
- @app.route('/chat')은 @app.route('/chat', method=['GET'])의 축약형
  - GET을 쓰면 입력값이 주소창으로 올라간다.
  - POST로 보내줘야 주소창으로 정보가 올라가지 않는다.

---

### Git Bash

- echo $PATH

- git은 폴더 단위로 관리
- git 밑에는 git을 깔면 절대 안된다!!(최상단에만!)
  - .git은 딱 한 개만 만들면 된다.
- .gitignore 파일은 관리하고 싶은 폴더에 만들면 된다.