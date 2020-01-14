# 20200114_Setting of  Spring & Vue

## Swagger

- 왜 필요한가? 백엔드 / 프론트엔드 개발자의 소통 도구

- 협업 경험에 대해 적어달라고 하면 

  => 프론트엔드 개발자와의 협업을 위해 swagger를 도입했다고 하면 좋다

---

## 프로젝트 시작하기

### Frontend 개발 환경

- Java JDK 설치 후 환경변수 지

- node.js 설치 : 설치 시 **안정화 버전**으로 설치 권장

- yarn 설치하기(2가지 방법)

  - https://yarnpkg.com/en/docs/install#windows-stable에서 설치 프로그램 다운 받아 설치

  - npm으로 설치(global로 설치)

    ```bash
    $ npm install -g yarn
    ```

- **Vue**

  - Vue 설치

    - yarn

      ```bash
      $ yarn global add @vue/cli
      ```

    - vue

      ```bash
      $ npm install -g vue-cli
      ```

  - Vue 프로젝트 생성

    ```bash
    $ vue create <project-name>
    ```

  - Server 실행

    ```bash
    $ yarn serve
    ```

    ```bash
    $ npm run serve
    ```

  - Sass 설정

    - Webpack에서 Sass를 CSS로 컴파일 하기 위함

    ```bash
    $ yarn add node-sass sass-loader
    ```

  - Vue-router, Vuex 설치

    ```bash
    $yarn add vue-router vuex
    ```

  

  ### Backend 개발 환경

  - Spring Boot 설치

    - 자바 JDK 설치 후 환경 변수 지정

    - Maven 설치

      - [http://maven.apache.org/download.cgi](http://maven.apache.org/download.cgi) 접속 후 ***Binary zip archive*** zip 파일 다운 받아 설치

      - 원하는 폴더에 넣고 시스템 변수 등록(MAVEN_HOME을 변수명으로)

      - 시스템 변수 path에 %MAVEN_HOME%]\bin 추가

      - 버전 확인

        ```bash
        $ mvn -v
        ```

  - VS code의 Extension에서 **Spring Boot Extension Pack** 설치

  - Ctrl + Shift + P (또는 F1) -> Spring Initializer : Generate Mave Project 실행

    - Java
    - Spring Boot Version : 2.2.2
    - Dependency : Lombok, Spring Web 선택
    - 참고 : [https://code.visualstudio.com/docs/java/java-spring-boot](https://code.visualstudio.com/docs/java/java-spring-boot)

  - Lombok과 VS code 충돌 시 다음 코드 추가

    `pom.xml`

    ```xml
    <properties>
        <!-- properties에 추가 -->
    	<java.version>1.8</java.version>
        <m2e.apt.activation>disabled</m2e.apt.activation>
    </properties>
    ```

  - [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)

  

  ---

  ## 기타

  - [Webpack](https://ko.wikipedia.org/wiki/웹팩): 오픈 소스 자바 스크립트 모듈 번들러
  - [Sass(SCSS)](https://ko.wikipedia.org/wiki/Sass_(스타일시트_언어))