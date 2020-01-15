# 01 About Spring

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
  - 제어 역행 : 메소드나 객체의 호출 작업이 외부에 의해 결정
    - 개발자의 역할 : 프레임워크에 필요한 기능을 개발하고 그를 조립
    - 대부분의 프레임워크에서 사용하는 방법
- 관점지향 프로그래밍(AOP / Aspecㅛt Oriented Programming)
  - 핵심적인 관점 / 부가적인 관점을 나눈 후, 그 관점을 각각 모듈화
- 컨테이너 기능 : 애플리케이션 객체의 생명 주기와 설정 및 관리
- 컴포넌트 기반
- 영속성
- 확장성



---

### Reference

- [블로그 이러쿵 저러쿵 - 스프링 이해하기](https://ooz.co.kr/170)
- [블로그 개발자인듯 개발자아닌듯 - 스프링의 특징](https://nanamix.tistory.com/496)
- [블로그 새로비 - 스프링 AOP 총정리](https://engkimbs.tistory.com/746)
- 위키백과