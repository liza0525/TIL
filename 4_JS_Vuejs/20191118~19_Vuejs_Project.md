# 20191118~19_Vuejs_Project

- Vue router
- Vue UI : 그래픽 기반으로 Vue 설정

```bash
$ vue ui
```

- Vuex

  - 양방향 데이터 저장소
  - 현재까지 배운 Vue는 Data를 제일 상단 아키텍처에 보냄

- Front는 Vue로, Server는 Django로

  - Auth를 구성할 때 Vue와 Django를 연결하는 것이 쉽지 않다.

    - JSON Webserver에게 어떻게 인증된 사람인지 인증하는 것을 배울 것임

      (JSON Web Token)

    - JSON Web Token은 후에 모바일에서도 쓸 것임

- 배포 : AWS, 헤로크, Doker 등 현업에서 쓸 기술

---

|   Vue.js   | Django |
| :--------: | :----: |
| Vue router |  DRF   |
|    Vuex    |        |
|    JWT     |        |

---

drf_and_vue

ㄴ todo-front(Vue.js)

ㄴ todo-back(Django)

---

## Vue Router

### vue ui로 router 플러그인 설치

```bash
$ vue ui
```

- 가져오기 > 폴더 가져오기 > 플러그인/의존성 확인해보기
- 플러그인에서 vue router와 vuex 추가 버튼이 생김
- 플러그인 추가 > @vue/cli-plugin-router 추가

- 이전에는 bash명령어로 쳤던 것을 vue ui로 한 것 뿐임

### git bash에서

```bash
$ npm install vue-router
$ vue add router
```

- ? Use history mode for router? (Requires proper server setup for index fallback
  in production) - > Yes

- JS는 다른 파일에서 어느 모듈을 import 해서 쓰게 하려면, 해당 JS 파일에서 export를 먼저 해줘야 가능해진다.



### router > index.js

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'
import Login from '../views/Login.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: Home
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  },
  {
    path: '/login',
    name: 'login',
    component: Login,
  },
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router

```

- 위와 같이 url 등록(django의 urls.py와 비슷)

```js
export default router // 맨 밑에 쓰면 된다.
```

- router를 설정했기 때문에 SPA라도 url이 변경되며 history 저장이 가능해짐

- alias 하나 알아가기 :)

```javascript
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
```





## Bootstrap 사용해보기

### bootstrap

- public > index.html에 CDN 추가

### Vuetify

- Bootstrap과 비슷한 Vue의 ui framework



## Log in 페이지 만들기

- 그동안에 Login 페이지를 만들 땐 form 태그를 이용했지만, 이는 하나의 component이기 때문에 Vue에서는 form 태그로 만들기보다는 component file을 생성한다.

- 주소가 바뀌어야 하는 Login.vue는 views에 만들고,

  Login component의 부분적인 또 다른 component, LoginForm은 components 폴더 내에 만든다.



## Django로 server단 만들기

- Django 프로젝트 시작 후 다음 pip를 모두 깔아준다.
- gitignore에 ignore할 파일들 등록

```bash
$ pip install djangorestframework
$ pip install djangorestframework-jwt
$ pip install django-cors-headers
```

-  각 framework github을 참고하여 settings.py의 INSTALLED_APP 또는 MIDDLEWARE에 등록
  - cors headers의 MIDDLEWARE는 첫번째 것만 넣으면 됨('corsheaders.middleware.CorsMiddleware')
- https://jpadilla.github.io/django-rest-framework-jwt/ 
  - Usage 부분 settings.py 어디에든 복붙
  - Additional Settings도 settings.py에 복붙
  - datetime package를 import
  - JWT_AUTH에 settings가 lint에 안 맞는다면 그냥 settings를 지워주면 된다.
- JWT detail은  https://velopert.com/2389 참고!



### 로그인

- 세션은 통과 가능자를 저장한 장부 같은 것이라면
- JWT는 팔찌를 나눠줘서 그것을 갖고 있는 사람을 통과 시키는 것이라 생각하면 된다.
-  https://auth0.com/?utm_source=jwtio&utm_campaign=craftedby 참고
- JWT의 구성은 header/payload/signature
- Vue session으로 세션 관리(이제 Django가 하지 않음)

```bash
$ npm i vue-session
```

-  https://www.npmjs.com/package/vue-session 참고
- vue lifecycle :  [https://medium.com/witinweb/vue-js-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-7780cdd97dd4](https://medium.com/witinweb/vue-js-라이프사이클-이해하기-7780cdd97dd4)  참고
  - endpoint는 크게 4가지 : created, mounted, updated, destroyed
  - 이 중 mounted, updated 위주로 쓰며, 보이기 직전에 많이 쓰기 때문에 beforeMount 위주로 쓰게 될 것이다.



### mount point 설정

```javascript
new Vue({
  router,
  render: h => h(App) // Main Template인 App.vue
}).$mount('#app')

// 또는
new Vue({
    el: '#app', // 이것이 mount point
    ...
})
```





## 기타

- 요즘은 서비스는 나눠도 single language를 쓰자는 추세 -> node.js를 많이 쓰고 있음
- React : component 기반의 SPA(Vue와 매우 비슷)
- SPA 취약점
  - history 저장 x
    - console에 window.history를 치면 지금까지 방문한 사이트에 대한 내용(세션)이 저장된다.
    - 하지만 SPA에서는 이를 저장할 수 없음(그래서 뒤로가기도 할 수 없다.)
  - SEO(Search Engine Optimization) 취약 
    - 사람들은 구글, 네이버를 통해 사이트를 들어가는 경우가 많다.
    - 구글, 네이버 등이 스파이더로서 전세계 모든 사이트를 돌아다니며 크롤링(합법임) -> 검색어와 정보의 매칭(랭킹까지)

- #(shebang)은 history에 저장되는 요소 중 하나다.
- 가상환경 만들기 -> 실행

```bash
$ python -m venv todo-venv
$ source todo-venv/Scripts/activate
```

- git에서는 SHA1을 쓰고 있다.