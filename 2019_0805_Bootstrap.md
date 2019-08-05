# 2019_0805_Bootstrap

## homework & workshop 10 Review

### nth-child vs. nth-of-type

- nth-child : 모든 자식들 중에 몇 번째인지
- nth-of-type : 각 타입 중에 몇 번째인지



### 후손 셀렉터 vs. 자식 셀렉터

- 후손 셀렉터 : 대대손손 물려주는 그런 느낌 :)ㅋㅋ
- span 태그 : in-line tag

```html
<!-- 후손 셀렉터 vs 자식 셀렉터  -->
  <div>
    <div>
      <h1>
        이건 테스트입니다.
        <span>이건 inline span tag</span>
      </h1>
      <div>
        <p>저는 누군가의 후손입니다.</p>
      </div>
    </div>
  </div>
```

```css
	div > span {
  color : goldenrod;
}
```

- 이렇게 해도 span 태그에는 CSS가 먹히지 않는다 why?  바로 밑 자식한테만 영향을 주기 때문

- 자식뿐만이 아닌 후손한테 모든 영향을 주고 싶을 때는 다음과 같이 하면 된다.

```css
div span {
  color : goldenrod;
}
```



## MBTI 사이트 만들기(mbti.html)

1. 복합 선택자(선택자를 여러 개 섞어서 쓴다.)

```css
/* a 태그 바로 다음에 오는 같은 level의 태그 (형제 태그)만 선택할 땐 +를 쓴다 */
a + ul {
  background-color : goldenrod
}

/* a 태그와 함께 오는 모든 형제 태그 선택 */
a ~ ul{
  color : rgb(139, 0, 81);
}

/* 속성 셀렉터 */
a[target="_blank"] {
  border : solid 2px black;
  border-radius: 5px
}
```

- target : a 태그 속성 중 하나. target="_blank"이면, 새 탭에서 열린다.



2. 정규표현식

```css
img[alt$="TYPE"] {}
img[alt~="TYPE"] {}
img[alt^="TYPE"] {}
```

- 이런 것들을 볼 수는 있을 것이지만, 사실상 거의 쓰지 않는다.
  - 생활코딩 또는 https://regexr.com/에서 어떻게 쓰는지 공부할 수 있음



3. 초기 설정

```css
/* select all */
/* 초기 설정, 재설정할 때 많이 쓴다 */
* {
  margin : 0;
  padding : 0;
}
```



## Bootstrap

- 트위터에서 만듦 
- vs. Material (CSS 프레임워크)
- materialize(구글 디자인의 시초가 된 프레임워크가 있는 site)
- https://www.designspectrum.org/ 디자인 커뮤니티
- 2010년을 기점으로 UI가 점차 간소화 되기 시작했다(스마트폰의 보급으로 스마트폰 환경에 맞는 웹 디자인이 필요하게 되었음)

- documentation > utilities
- documentation > components를 많이 쓸 것임
- getting started 탭의 starter template을 쓰면 훨씬 편할 것



### CDN

- 빠르게 돌아가는 페이지를 만들 수 있기 때문에 꼭 쓰도록 한다.

- CDN은 보통 적절한 수준으로 캐시 설정으로 빠르게 함 : 컴퓨터가 알아서 해줌



### Bootstrap 이용하기

- 홈페이지에서 파일을 다운로드 받아 쓰기
- 클래스를 이용하여 margin, padding 등을 조절

1. margin, padding 조절

```html
  <title>Document</title>
  <style>
    #mola {
      text-align : center
    }
  </style>
</head>
<body>
  <h1 id='mola'>중앙정렬 하고 싶다아아</h1>
  <a href="#">나는 인라인</a>
</body>
</html>
```

```html
  <style>
    #mola {
      text-align : center
    }
  </style>
</head>
<body>
  <h1 class="text-center">중앙정렬 하고 싶다아아</h1>
  <a href="#">나는 인라인</a>
</body>
</html>
```



2. 색 조절

- 필요한 색들에 의미가 있는 이름을 넣어놨음 : 후에 customize가 가능하다.

- 콘텐츠 배경, 글자색, 버튼, 네비게이션 바 등 모두에 쓸 수 있다.



3. Documentation > components

- 많이 쓰는 것	
  - breadcrumb, card, carousel, jumbotron
- 잘 쓰지 않는 것(mobile first 세상에서 좋지 않은 UI이기 때문에)
  - dropdowns, modal, popovers





### Grid

- 디자인을 하는 사람들은 꼭 grid를 만들어 작업을 한다.
- 기준 숫자 12을 꼭 쓰도록 한다. (가장 많은 약수를 갖고 있기 때문에)

```html
 <div class="container-flurd bg-secondary">
      <div class="row">
        <div class="col">
          칼럼
        </div>
      </div>
    </div>
```

- container-flurd : 현 시대의 웹 컨텐츠는 정사각형 모니터에서도 깨지지 않게 볼 수 있게 한정된 공간에 들어 있지만, flurd를 쓰면 끝까지 꽉 채워준다.

- grid 간의 폭을 줄이려면 margin 대신 padding을 조정하도록 한다.
- 반응형 grid를 만들기 위해서는 다음과 같이 한다.

```html
    <div class="container-flurd bg-secondary">
      <div class="row bg-primary">
          <div class="col-sm-4 px-1"> <!--col-sm-4로 하면 된당-->
            칼럼1
          </div>
          <div class="col-sm-4 px-1 bg-info">
            칼럼2
          </div>
          <div class="col-sm-4 px-1 bg-success">
            칼럼3
          </div>
      </div>
    </div>
```

