# 20191029_Fundamental_of_Javascript

## 자바스크립트

- HTML -> CSS -> Javascript
- Javascript는 browser 조작 언어(였어요..)
  - Node.js : browser뿐만이 아니라 상용 프로그래밍 언어로도 쓸 수 있다!
- 자바의 인기에 편승하려고 자바스크립트라는 이름을 씀
- 브라우저를 조작하기 위한 언어
- IE JS / Chrome JS / FireForx JS 등의 신기한 버전들이 있다.
  - 규격 통일이 필요해짐 : ECMA에서 만든 JS (**ECMA Script**)
    - ES2015 쯤부터 Chrome이 지키기 시작
    - Firefox는 원래부터 잘 지킴
    - IE는 ES와 별개로 마음대로..
- 언어적인 업그레이드가 ES2015(ES06) 규격이 나올 때부터 급 부상하였다.



## Vanilla JS

- Modern Framework를 쓸 수 있게 하는 라이브러리
- DOM / BOM / JavaScript
  - doucoment
  - XMLHttpRequest : javascript를 통해서 특정 endpoint에 요청을 전송



### 개발자 도구 - Console 창

- window : 브라우저 자체를 하나의 객체로 표현한 것(한번 properties를 다 탐구해볼 것)
- window.history
- window.document.getElementsByTagName('p')
- window.print
- ctrl+l : 모든 line 삭제



## javascript를 해보자

### 변수 선언

```javascript
let x = 1 // 변수 타입을 쓰지 않으면 전역변수로 지정(비추...!)
// global variable은 scope를 망치는 부분이 있기 때문에 최대한 사용을 지양하도록 
x = 2

if (x == 2) {
    let x = 3
    console.log(x)
}

console.log(x)
```

- let은 block scope 내에서 쓰이는 변수를 지정하는 type

```javascript
const MY_FAV = 10 // 상수명은 대문자로

console.log('내가 좋아하는 숫자는 ' + MY_FAV)
console.log(`내가 좋아하는 숫자는 ${MY_FAV}`)
```

- const는 상수(재선언, 재할당 둘 다 안된다.)

```javascript
MY_FAV = 20 // TypeError: Assignment to constant variable.
```

- TypeError: Assignment to constant variable.
  - 상수는 새로운 값을 할당하려고 하면 위와 같은 에러를 낸다.

- var는 앞으로 쓰지 말 것! Hoisting 개념이 있고 이상하게 동작하는 경우가 너무 많다.



### Javascript의 자료형

- **Primitive Type**(원시 자료형/기본 자료형)

  - 숫자 / 글자

  ```javascript
  typeof 2 
  // "number"
  
  typeof Infinity
  typeof -Infinity
  typeof NaN
  ```

  - boolean(true/false) : 대문자로 쓰면 undefined라고 뜬다.

  ```javascript
  !true //!는 bang이라고 하는 not operator이다.
  true ? '참입니다.' : '거짓입니다.'
  ```

  - Empty value : undefined(default), null

  ```javascript
  typeof undefined
  // "undefined"
  typeof null
  // "object"
  null == undefined
  // true
  null === undefined
  // false
  [] === []
  // false
  // 주소값까지 같은 것인지 확인
  ```

  - Javascript에서는 이 원시 자료형들이 객체화 되어 있기 때문에, 자료형 자체는 메소드를 쓸 수 있다.

- Implicit Type Conversion(묵시적 형변환)

  - 너무 자유로운 형변환때문에 엣지케이스 정의하기가 너무 어렵다...

### for문

```javascript
for (let i = 0; i < menus.length; i++) {
	console.log(menus[i])
}
// 또는
for (let menu of menus){
	console.log(menu)
}
// 또는
menus.forEach(function(menu){
	console.log(menu)
})
// 또는
menus.forEach(menu => {
	console.log(menu)
})
```



### 함수 선언

```javascript
function add(x, y){
    return x + y
}

const sub = function(x, y){
    return x - y
}

const mul = (x, y) => {
    return x * y
}
// ES6에서 새로 나온 function

const ssafy = function (name) {
    return `안녕, ${name}`
}

// parameter를 생략할 땐 parameter가 1개일 때
// block 없애는 건 표현식이 1개일 때
const ssafy1 = name => `안녕, ${name}`


const square = function (num) {
    return num ** 2
}

const square2 = num => num ** 2

// => : arrow function, hash rocket 등으로 불린다.
```

- 첫 번째 경우는 Hoisting이 일어나기 때문에 지양하는 방법이다.



### Array

```javascript
const nums = [1, 2, 3, 4]

console.log(nums[0])
console.log(nums[nums.length-1])
console.log(nums.reverse()) // 원본도 바뀐다. 선택지가 이것밖에 없음
nums.push(0)
console.log(nums)
nums.pop()
console.log(nums)

//unshift, shift, includes, 
nums.unshift(5) // 0번 index에 저장
console.log(nums)
nums.shift() // 0번 index의 값 pop
console.log(nums)
// queue 구현 가능

console.log(nums.includes(0))
console.log(nums.includes(7))

console.log(nums.indexOf(2)) // 2가 있는 위치의 index

//======================================================
let newNums = []
nums.forEach(function(num){
    newNums.push(num*num)
})
console.log(newNums)

const squaredNums = nums.map(function(num){
    return num ** 2
})

console.log(squaredNums)
```



### object

- 파이썬의 dictionary와 비슷하다.
- 기본적으로 객체지향이기 때문에 dot으로 property 접근도 가능하다.(dot notation)

```javascript
const me = {
    name: 'john',
    sleep: function(){
        console.log('쿨쿨')
    }, // 함수를 넣을 수도 있다.
    appleProducts: {
        macBook: '2018pro_mac',
        iPad: '2018pro_ipad',
    },// 다른 object를 넣어도 된다.
}

console.log(me['name'])
console.log(me.name)
//console.log(me[name]) // 이렇게 쓰면 작동 안함
console.log(me['sleep']())
console.log(me.sleep())
console.log(me.appleProducts.macBook)
```



### eventlistner

- dino 게임 만들기

```javascript
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
      min-height: 100vh;
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
  </script>
</body>

</html>
```

- **To be continue!**



## 기타

- ECMA : 하드웨어 관련 규격을 만드는 단체 -> 현재는 소프트웨어 규격도 만들기 시작
- 규격이라는 것은 중요 : 규격이 없으면 vendor들이 자기 마음대로 만들기 때문
- jQuery : 자바스크립트를 쉽게 쓸 수 있는 라이브러리(made by John Resig)
- 현재 javascript는 세미콜론을 안 쓰기 시작했다.
  - 브랜던 아이크(Javascript 창시자 / FireFox 창시자 / Brave 창시자)는 떼기를 원하는 중 ㅎㅎ..
- django 배포는 서칭해서 해보면 된다~~!
- Javascript는 UTF-8 Encoding

- 새로운 프로그래밍 언어를 배울 때 다음과 같은 플로우를 먼저 공부하고 진행
  - 컴퓨터 프로그래밍 언어는 저장 및 조작(제어)이다.
  - 저장 부분은 **'무엇을, 어디에, 어떻게'** 저장하는지 확인
    - 무엇을 : 자료형, Data Type
      - 숫자 / 글자/ boolean / Empty Value
    - 어디에 : identifier 변수명, Container Type
    - 어떻게 : 등호(=)
  - 조작(제어) 부분에서 1. 기존에 알던 언어와의 문법적 차이 2. 언어의 관례를 공부
    - javascript에서는 상수는 ALLCAP을, 변수명/함수명은 camelCase를, 클래스는 PascalCase를 쓴다.
    - python에서는 snake_case를 쓴다.
  - function과 class까지 구현

- **typescript** : javascript를 견고하게 정의한 언어