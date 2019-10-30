# 20191030_Javascript

## Event Listener

- addEventListener([a kind of event], [function])
- 보통 function에는 익명의 함수가 들어가고, 그 함수의 parameter는 event(또는 e)가 들어간다. 
- function 키워드 대신 arrow 사용 가능, parameter가 하나인 경우에는 소괄호도 생략 가능
- 따라서 다음과 같이 작성이 가능하다.

```javascript
button.addEventListener('click', e => {
      // 함수 내용
})
```



### dino 계속

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <title>Dino</title>
  <style>
    .bg {
      background-color: #f7f7f7;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 90vh;
    }
  </style>
</head>

<body>
  <div class="bg">
    <img id="dino" width="100px" heigth="100px"
      src="https://is4-ssl.mzstatic.com/image/thumb/Purple118/v4/88/e5/36/88e536d4-8a08-7c3b-ad29-c4e5dabc9f45/AppIcon-1x_U007emarketing-sRGB-85-220-0-6.png/246x0w.jpg"
      alt="dino" />
  </div>

  <script>
    const dino = document.querySelector('#dino')
    dino.addEventListener('click', event => {
      alert('아야')
      console.log(event)
    })

    let x = 0
    let y = 0

    document.addEventListener('keydown', e => {
      // console.log(e.key)
      // console.log(e.keyCode) // 키보드를 나타내는 좀 더 확실한 identifier
      if (e.keyCode === 37) { // 등호는 꼭 3개로 쓸 것
        console.log('왼쪽로 이동')
        x -= 40
        dino.style.marginLeft = `${x}px`
      } else if (e.keyCode === 38) {
        console.log('위로 이동')
        y -= 40
        dino.style.marginTop = `${y}px`
      } else if (e.keyCode === 39) {
        console.log('오른쪽로 이동')
        x += 40
        dino.style.marginLeft = `${x}px`
      } else if (e.keyCode === 40) {
        console.log('아래로 이동')
        y += 40
        dino.style.marginTop = `${y}px`
      }
      // else {
      //   alert('잘못된 키를 누르셨습니다.')
      // }
    })

  </script>
</body>

</html>
```



### Shopping List

- 버튼이 눌렸을 때, event가 일어나도록 한다.

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
  <h1>My Shopping List</h1>
  Enter a new item: <input id="item-input" type="text">
  <button id="add-button">Add Item</button>
  <ul id="shopping-list">

  </ul>

  <script>
    const input = document.querySelector('#item-input')
    const button = document.querySelector('#add-button')
    const shoppingList = document.querySelector('#shopping-list')
	
    // 해당 버튼을 눌렀을 때 아이템이 추가
    button.addEventListener('click', e => {
      const itemName = input.value
      input.value = ''
      
      const item = document.createElement('li')
      item.innerText = itemName

      const deleteButton = document.createElement('button')
      deleteButton.innerText = 'delete'
	 
      // 해당 버튼을 눌렀을 때 아이템 삭제
      deleteButton.addEventListener('click', e => {
        item.remove()
      })

      item.appendChild(deleteButton) // item(li 태그) 객체의 자식으로 deleteButton을 추가한다.

      shoppingList.appendChild(item) // shoppingList(id가 #shopping-list인 곳) 객체의 자식으로 item을 추가한다.
    })
  </script>
</body>
</html>
```



## 자바스크립트 특징 - Asynchronous

- python은 한 줄 한 줄 실행하는 synchronous(코드와 함께 실행), blocking한 언어

- javascript은 기본적으로 synchronous하지만 필요한 경우에는 asynchronous, non-blocking하게 만들어 둔 언어

  - **why?** 브라우저 조작 언어로 태어났기 때문(+single thread)

    ​		blocking하는 코드가 생긴다면 그 다음 코드가 밀리기 때문에

    ​		따라서, 일정 시간이 걸리거나 종료를 예측할 수 없는 경우에는 

    ​		알아서 다음 코드를 돌 수 있게 언어 자체를 asynchronous하게 만든 것이다.

    ​		(asynchronous한 코드는 callstack에 쌓아두고 처리)

  - callback : 해당 함수가 실행된 후 다음에 실행될 함수들을 뜻함

    - ex ) addEventListener는 어떠한 event가 일어날 때만 실행되고, 그 내의 함수를 핸들링 하는 것이 callback(..? 맞는지 확인 要)

-  http://latentflip.com/loupe/?code=ZnVuY3Rpb24gcHJpbnRIZWxsbygpIHsNCiAgICBjb25zb2xlLmxvZygnSGVsbG8gZnJvbSBiYXonKTsNCn0NCg0KZnVuY3Rpb24gYmF6KCkgew0KICAgIHNldFRpbWVvdXQocHJpbnRIZWxsbywgMzAwMCk7DQp9DQoNCmZ1bmN0aW9uIGJhcigpIHsNCiAgICBiYXooKTsNCn0NCg0KZnVuY3Rpb24gZm9vKCkgew0KICAgIGJhcigpOw0KfQ0KDQpmb28oKTs%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D

  - 앞으로도 이 플랫폼을 계속 쓸 것임

- Javascript의 engine은 multi-thread하다, javascript의 실행 context가 single-thread일 뿐
- 우리는 Asynchronous한 함수를 만들 수 있는 것은 아니다. asynchronous 함수는 언어 단에서 이미 만들어진 것임 (*면접 질문으로 나올 수 있음*)
- addEventListener, XMLHttpRequest 등이 대표적인 asynchronous 함수이다.



## 기타

- 네트워크 관련
  - 유튜브 : 패킷의 여행
  - cmd 창에서 tracert [ip 주소] 를 치면 그 ip 주소로 가는 패킷의 경로를 볼 수 있다.(물리 레이어에서)
  -  https://ko.coursera.org/learn/computer-networking (네트워크 강의 추천)