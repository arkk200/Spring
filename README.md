### **인프런에 [이 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)에서 학습하고 있습니다.**

# **1. 프로젝트 생성**
[spring initializr](https://start.spring.io)는 스프링에서 운영하는 사이트로 스프링 부트를 기반으로 스프링 프로젝트를 만들어준다.<br>
(아래는 spring initializr 사이트 화면이다.)
![](./images/01-01.png)

Project 란에 Maven / Gradle Project가 있다.<br>
요즘엔 Gradle Project를 주로 쓴다.

그 외에 버전, 메타 데이터를 설정한다.<br>
Dependencies에는 **Spring Web**과 **Thymeleaf 라이브러리**를 추가해준다.<br>
그리고 밑에 GENERATE 버튼을 클릭하면 zip 파일을 준다

압축을 풀고 intelliJ나 eclipse로 bundle.gradle을 열어주면 된다.<br>
열면 Dependencies로 설정한 라이브러리 등등을 다운 받아준다.

폴더 구조는 이렇게 생겼다.<br>
![](./images/01-02.png)

src폴더에 build.gradle을 보면 설정했던 내용들이 들어있다.

src폴더에 main/java폴더에 들어가면 spring initializr 사이트에서 입력했던 메타 데이터 이름으로 폴더와 .java 파일이 있다.<br>
.java 파일에 public 클래스에 main 메서드를 실행하면 프로젝트를 실행할 수 있다.
![](./images/01-03.png)

# **2. 라이브러리 살펴보기**
Gradle은 라이브러리를 설치할 때 의존 관계가 있는 라이브러리를 할께 다운로드 한다.

**스프링 부트 라이브러리**

- spring-boot-starter-web 라이브러리에는 대표적으로
    - spring-boot-starter-tomcat (톰캣 웹서버)와
    - spring-webmvc: 스프링 웹 MVC 라이브러리가 들어있다.
- spring-boot-starter-thymeleaf는 html을 렌더링해주는 템플릿 엔진이다.
- spring-boot-starter (공통적으로 들어있음)에는
    - spring-boot와
        - spring-core 등 스프링 관련 라이브러리가 들어있고
    - spring-boot-starter-logging에
        - logback(구현체), slf4j(인터페이스)가 들어있다.

**테스트 라이브러리**
- spring-boot-starter-test 라이브러리에는
    - junit (테스트 프레임워크)
    - mockito (mock 라이브러리)
    - assertj (테스트 코드 편하게 작성하게 도와주는 라이브러리)
    - spring-test (스프링 통합 테스트)가 들어있다.
