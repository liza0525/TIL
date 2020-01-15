# 01_Fundamental of Spring

> 이 문서는 블로그 '이러쿵 저러쿵'의 Spring 기본 개념 관련 문서를 통합적으로 정리했습니다. 해당 블로그 사이트는 하단의 [Reference](#reference)의 링크로 확인할 수 있습니다. 그 외의 참고 사이트 링크 또한 모두 [Reference](#reference)에 추가되었습니다.



## 스프링이란?

- 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임 워크



## 스프링의 특징

- 경량 프레임워크
- API를 사용하지 않고 객체 간의 관계를 구성
- 자바 SE로 된 POJO를 자바 EE에 의존적이지 않게 연결해주는 역할
  - POJO : Plain Old Java Object [(위키백과)](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object)
  - Java SE : Java Standard Edition
  - Java EE : Java Enterprise Edition
- 제어 역행(IoC) 기술을 통해 애플리케이션 간의 결합을 도모
- 관점지향 프로그래밍(AOP / Aspect Oriented Programming)
  - 핵심적인 관점 / 부가적인 관점을 나눈 후, 그 관점을 각각 모듈화
- 컨테이너 기능 : 애플리케이션 객체의 생명 주기와 설정 및 관리
- 컴포넌트 기반
- 영속성
- 확장성



## 스프링의 주요 구성 요소

### **IoC(제어 역행 / Inversion of Control)** 

- 메소드나 객체의 호출 작업이 외부에 의해 결정
- 대부분의 프레임워크에서 사용하는 방법
- 개발자의 역할 : 프레임워크에 필요한 기능을 개발하고 그를 조립



### DI(의존성 주입 / Dependency Injection)

#### 일체형과 분리형

- **일체형(HAS-A)** : 객체 A로 인스턴스 B를 생성

  ```java
  Car c = new Car();
  // 객체 Car를 이용하여 인스턴스 c 생성
  ```

  - 객체 내부의 프로세스에 대해 신경 쓸 필요가 없음

- **분리/도킹(부착)형(Association)** : 인스턴스 A에 다른 객체로 생성한 인스턴스 B를 이용

  ```java
  Car c = new Car();
  Tire t = new Tire(); 
  c.setTire(t);
  // 인스턴스 c에 다른 객체로 생성한 인스턴스 t를 이용
  ```

  - 각 객체의 세팅을 개별적으로 해주어야 함
  - 각 객체(예시에서는 Tire 객체)를 유동적으로 바꿔줄 수 있음

- 위의 일체형과 분리형 중 **분리형**이 DI의 개념이라고 할 수 있다.
- 각 객체 간의 결합도를 낮추는 것이 DI를 사용하는 목적

#### DI의 종류

- Setter Injection(세터 주입)

```java
Car c = new Car();
Tire t = new Tire();
c.setTire(t);
```

- Construction Injection(생성자 주입)

```java
Tire t = new Tire();
Car c = new Car(t);
```

> 스프링에서는 이런 과정을 자동화함

### 스프링에서의 DI

> 부품들을 생성하고 제품을 조립해주는 공정 과정을 대신해 주는 라이브러리

- 작은 부품부터 큰 부품으로, 제품을 만드는 순서가  역순(IoC)
- 부품을 모아서 조립하는 것을 도움
- IoC Container : 제품 제작 작업을 Container에 담아서 처리

#### DI 구현 - XML

- 객체의 생성과 도킹에 대한 내용을 XML에 분리

  - 이는 JAVA compile 없이 XML 변경으로만으로 내용을 조정할 수 있음
  - java 코드에서는 XML을 parsing하여 container에 담음

- 빈(Bean) 객체는 반드시 클래스를 사용(Abstract Class나 Interface 사용 불가)

  - 생성 시, 객체 초기값 입력

- 예시

  `config.xml`

  ```xml
  <bean id="record" class="di.SprRecord"></bean>
  <bean id="view" class="di.SprRecordView">
  	<property name="record" ref="record"></property>
  </bean>
  ```

  - `<bean></bean>` : 빈 객체 생성
  - `id` : 빈 객체 고유 이름(접근 가능자)
  - `name`: 객체의 이름
  - `class`: 생성할 클래스
  - `constructor-arg` : 초기값 설정(생성자 함수 사용)
  - `<property></property>` : 초기값 설정(Setter 함수 사용)

  `java code`

  ```java
  // ApplicationContext가 IoC Container 역할
  ApplicationContext ctx = new ClassPathXmlApplicationContext("config.xml");
  RecondView = (RecordView) ctx.getBean("view");
  ```

  

### AOP

### PSA

- Portable Service Abstractions / 서비스 추상화
- 성격이 비슷한 여러 종류의 기술을 추상화 



---

### Reference

- [블로그 이러쿵 저러쿵 - 스프링 프레임워크 기본 개념 강좌](https://ooz.co.kr)
- [블로그 개발자인듯 개발자아닌듯 - 스프링의 특징](https://nanamix.tistory.com/496)
- [블로그 새로비 - 스프링 AOP 총정리](https://engkimbs.tistory.com/746)
- 위키백과