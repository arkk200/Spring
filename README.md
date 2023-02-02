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