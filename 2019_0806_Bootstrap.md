# 2019_0806_Bootstrap

## homework & workshop 11 Review

5개의 반응형 사이즈 조건 : xs(default값임), sm, md, lg, xl

justify-content-center : 컨텐츠 가운데 정렬



## Flex

- 기존에 배운 display 4가지 :  block, in-line, in-line block, none
- 오늘 Flex라는 것을 배울 것이다.

- Flex라는 display는 구현되어 있는 내부를 보는 것이라고 생각하면 된다.
- 유연하게 box를 컨트롤
- 축을 기준으로 컨트롤 한다. (x, y축)

- box 안의 content를 flex를 통해 조절하겠다는 것
- html은 box를 이용한 거라서 세로 정렬 하기가 쉽지 않았지만, flex 기능이 나오고 세로 정렬이 가능해졌다.
- flex 내에서는 contents의 크기를 정하는 것이 중요하다.



### flex-direction

- 기본 값은 row
- row, row-reverse(x축), column, column-reverse



### justify-content

- 메인 축을 기준으로 정렬(일반적인 메인 축은 x축)

- flex-start(왼쪽 정렬/default), **center**(가운데 정렬), flex-end(오른쪽 정렬), space-between(양쪽 정렬), space-around(양쪽 정렬-양끝 간격 포함)

```html
  <style>
    .container {
      height : 800px;
      padding : 16px;
      border : 2px solid black;
      display: flex;
      flex-direction : row;
      justify-content: space-between;
    }
    .item {
      padding : 16px;
      border : 2px solid black;
      font-weight: bold;
      height: 50px;
      width: 50px;
    }

  </style>
```



### align-items

- 메인 축의 90도를 기준으로 정렬(일반적인 메인 축은 x축)
- 기본적으로 contents가 쌓이더라도 이미 space-around 된다.



### flex-wrap

- wrap(container 내에서만 쌓인다), nowrap(default)



## Media Query

- viewport : 모바일 화면 기준의 폭 개념(?)

```html
  <style>
    @media (max-width : 1024px) { /* width가 1024px보다 작을 때 -> True*/
      h1{
        color : darksalmon;
      }
    }
  </style>
```



## 기타

- flexbox frog : flex box에 대한 개념을 게임적 요소를 포함하여 이해를 돕는 사이트

- https://daneden.github.io/animate.css/
- https://fontawesome.com/ 아이콘을 class화 하여 제공하는 사이트
- https://codepen.io/ > popular