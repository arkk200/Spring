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

# **3. View 환경설정**
src/main/resources/static 폴더에 index.html 파일을 만들고,<br>
위에서 말한 .java 파일을 실행하면 '/'경로로 index.html 파일이 렌더링된다.

서버와 브라우저가 데이터를 주고 받으려면 다음처럼 하면 된다.<br>
src/main/java/(만들어진폴더) 에 controller 패키지를 만들고, controller 클래스를 만든다.<br>

![](./images/03-01.png)


사진에 HelloController 클래스는 다음처럼 쓰고

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}

```

src/main/resources/templates/ 폴더에 hello.html을 만들고 다음처럼 적으면 된다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Hello</title>
</head>
<body>
<p th:text="'안녕하세요.' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```

HelloController 클래스에 `@GetMapping("hello")`는 주소에 /hello 경로를 의미하고 `Model model`은 스프링에서 주는 모델을 받는다.<br>
`model.addAttribute("data", "hello!!");`는 `data`라는 키에 `hello!!`라는 값을 할당한거고<br>
`return "hello";`는 templates 폴더에 hello.html을 렌더링하라는 의미이다.

hello.html에 html태그에 xmlns:th는 thymeleaf 템플릿 엔진을 선언하는 코드고<br>
p태그에 th:text에 ${data}는 위에 HelloController에 `model.addAttribute("data", "hello!!");` 에 `data` 키에 해당하는 값, `hello!!`를 의미한다.

프로젝트를 실행하고 /hello 경로로 이동해보면

![](./images/03-02.png)

hello.html에 p태그에 th:text 속성이 p태그로 감싸져 나오는 것을 확인해볼 수 있다.

# **4. 빌드하고 실행하기**

터미널을 열고 spring initialzr에서 다운받고 코드를 짰던 폴더로 이동한다.<br>
mac은 `./gradlew build`, windows는 `./gradlew.bat build`를 입력하면 build 폴더가 만들어진다.

build/libs/ 폴더로 이동하고 ~~~~SNAPSHOT.jar 파일이 있는지 확인한 후
```
java -jar ~~~~~SNAPSHOT.jar
```
을 입력하면 스프링부트가 실행된다.

build 폴더를 삭제할 때는 gradlew, gradlew.bat이 있는 폴더에서 `./gradlew clean` 또는 `./gradlew.bat clean`을 입력하면 된다.