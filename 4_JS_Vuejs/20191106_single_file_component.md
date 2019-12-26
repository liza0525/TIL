# 20191106_Single File Component

- Component 하나를 하나의 문서로(확장자는 .vue)



## Vue-cli

- Vue-CLI : Vue 프로젝트를 원활하게, 짜임새있게 만드는 라이브러리
  - CLI : Commend Line Interface(ex. git bash 등)
  - 설치는 다음과 같이  *출처 https://cli.vuejs.org/* 

```bash
npm install -g @vue/cli
// yarn global add @vue/cli // 이전에는 yarn으로도 설치 가능했지만 현재는 npm을 쓰는 것을 권장
```

- Vue의 project는 다음과 같이

```bash
vue create todo-vue-cli
```

- endpoint가 미국에 있어 너무 멀기 때문, 중국으로 endpoint 설정하겠냐는 질문이 나온다. -> Yes
- Please pick a preset -> preset은 default로(그냥 enter)
- vs code로 확인하면 많은 파일들이 생성되어 있음

**babel.config.js**

- 구형 버전의 javescript만 구동되는 browser에서도 돌아가게 하기 위해 만들어졌을 것이라 예측(특히 IE)
-  https://caniuse.com/#search=JavaScript (javascript browser support 구글 검색)
- 우리는 이걸 한동안 건들지는 않을 것임

**node_modules**

- Vue 프로젝트를 시작하면 함께 설치되는 패키지들이 모여있음
  - Django는 global 단위기 때문에 패키지들이 전역의 어딘가에 숨겨져 있음
  - 110MB이기 때문에 git push에 영향을 주므로, gitignore 파일도 존재한다.(project 최상단에 존재)



### vue파일의 구조

- `<template>` : vue component의 template
- `<script>`
- `<style>`



## 실습(Todo List)

- HelloWorld.vue 삭제

### App.vue

- 우리가 mount할 요소들을 모두 여기에 정의

```vue
<template>
  <div id="app">
    <h1>Lizzie's Todo</h1>
    <TodoList category="취업준비" />
    <TodoList category="S" />
    <TodoList category="기타" />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue' // from은 뒤에 쓰고, 상대경로를 쓰도록 한다.
export default {
  components: {
    TodoList, // key와 value의 이름이 같으면 생략 가능하다. (TodoList: TodoList와 같음)
  } // 자식 components
};
</script>

<style lang="stylus" scoped>

</style>
```



### TodoList.vue

```vue
<template>
  <div>
    <h2>{{ category }}</h2>
    <input type="text" v-model="newTodo" @keyup.enter="addTodo" />
    <button @click="addTodo">+</button>
    <li v-for="todo in todos" :key="todo.id">
      <button @click="removeTodo(todo)">x</button>
      <span>{{ todo.content }}</span>
    </li>
  </div>
</template>

<script>
export default {
  props: {
    category: {
      type: String,
      required: true,
      validator: function(value) {
        if (value.length !== 0) {
          return true;
        } else {
          return false;
        }
      }
    }
  },  
  data: function() {
    return {
      todos: [],
      newTodo: ""
    };
  },
  methods: {
    addTodo: function() {
      if (this.newTodo.length != 0) {
        this.todos.push({
          id: Date.now(),
          content: this.newTodo,
          conpleted: false
        });
        this.newTodo = "";
      }
    },
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    }
  },
}

</script>

<style scoped>
</style>
```



## 실습(youtube-searcher)

- console-log error
  -  [https://khstar.tistory.com/entry/Vue%EC%8B%A4%ED%96%89%EC%8B%9C-error-Unexpected-console-statement-no-console](https://khstar.tistory.com/entry/Vue실행시-error-Unexpected-console-statement-no-console)

- emit : 자식이 부모에게 data를 주기 위해 이벤트를 발생

```js
$emit('이벤트', 보내줄 데이터)
```

- SearchBar.vue

```vue
<template>
  <div>
    <input @input="onInput" type="text">
  </div>
</template>

<script>
export default {
  name:'SearchBar',
  methods: {
    onInput(event) {
      // console.log(event.target.value)
      // $emit 메소드는 자식 컴포넌트 -> 부모 컴포넌트 data를 올려줄 때 사용
      this.$emit('inputChange', event.target.value)
    }
  }
}
</script>

<style>

</style>
```



**Video List**

```
1. onInputChange 메소드 호출
2. Youtube API 요청
3. Youtube으로부터 응답 수신
4. Youtube Response 비디오 리스트 App Component의 data로 저장
5. data가 업데이트 되면, 컴포넌트가 템플릿을 다시 렌더링을 함(Vue가 해줌)
6. VideoList에서 변경된 결과를 보여줌
```

- youtube api 받기
  -  https://console.developers.google.com/project?hl=ko 
  - `npm install axios`
  -  https://www.googleapis.com/youtube/v3/search?key=[api_key]&type=video&part=snippet&q=[검색어]



**.env.local(hidden api key)**

```
VUE_APP_YOUTUBE_API_KEY=[키]
```

```vue
const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
```





## 기타

- Webpack : JS 코드를 하나로 묶어주는 JS 라이브러리

- npm : JS의 pip

  - pip : 전역에 깔린다, 프로젝트별 패키지 관리가 불가해서 그동안에 가상환경을 써왔음
  - npm : Global, Local 그리고 Project 단위로 패키지 관리를 할 수 있다. Default는 Project

- global로 하지 않는 경우 : 각 Project의 Versioning을 위해

- **Component vue 파일은 PascalCase로**

- vue 개발 시 vs code extension : vetur / vue vscode snippets

- 파일 별 intenting 바꾸는 방법

  - F1 > Preferences : Open Settings(JSON)에서 다음을 추가

  ```json
      "[vue]":{ // 확장자
          "editor.tabSize": 2
      }
  ```

- syntax sugar : 개발자들을 위해 자주 쓰는 문법을 간략하게 만들어 편의성을 높인 문법

- < /> : self closing tag(closing 태그가 필요 없다/)

- kakao oven : project mock up

- adobe XD : app mock up

- Vue +tab : template, script, style 모두 한번에 생성

- props와 emit을 왜 쓰는가? **observer pattern**(구글링 고고)

- chrome extension : wappalyzer