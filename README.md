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

# **5. 웹 개발**
웹 개발에는 크게 3가지로
- 정적 컨텐츠
- MVC와 템플릿 엔진
- API

가 있다.

정적 컨텐츠 방식은 서버에서 하는 것 없이 클라이언트한테 그대로 보내주는 것이고<br>
MVC와 템플릿 엔진 방식은 JSP, PHP 같은 템플릿 엔진으로 서버에서 동적으로 바꿔서 클라이언트한테 보내주는 방식이다.<br>
API는 데이터를 제공받기 위한 규격을 의미하는데 데이터 구조 포맷으로 데이터를 클라이언트한테 보내주는 방식이다.

## **5-1. 정적 컨텐츠**

Spring Boot에서 src/main/resources/static/ 폴더에서 정적 컨텐츠를 생성할 수 있다.<br>
만약 `hello-static.html`을 만들고 프로젝트를 실행한 뒤, /hello-static.html 주소를 입력하면 `hello-static.html` 파일이 보인다.

Spring Boot에서 정적 컨텐츠가 보이는 방식은 이렇다.

1. 내장 톰캣 서버가 요청을 받고 스프링한테 넘긴다.
2. 스프링이 먼저 컨트롤러 쪽에 `hello-static.html`이 있는지 찾는다. (컨트롤러가 우선순위를 가짐)
3. `hello-static.html`로 맵핑이 된 컨트롤러가 없으니 resources/static/ 폴더에 hello-static.html을 찾는다.
4. 있으니 클라이언트 쪽으로 보내준다.

## **5-2. MVC와 템플릿 엔진**

MVC: Model, View, Controller

과거에는 Controller와 View라는게 따로 분리되어 있지 않았고 View에 모든 걸 다했다. JSP가 그렇다.<br>
이 방식을 모델1 방식이라고 한다.

url에 파라미터를 받아오는 방식은 다음처럼 하면 된다.

먼저 위에서 만들었던 HelloController 클래스에 `hello-mvc`를 Get으로 맵핑해준다.
```java
@GetMapping("hello-mvc")
public String helloMvc(@RequestParam("name") String name, Model model) {
    model.addAttribute("name", name);
    return "hello-template";
}
```
helloMvc 메소드에 `@RequestParam("name") String name` 파라미터가 있는데 url에 name이라는 키의 값은 이 파라미터가 받아온다.<br>
그 값을 model에 name이라는 키로 할당하고 hello-template이라는 파일을 렌더링해준다.

hello-template이라는 파일이 있어야 렌더링 되므로 resources/templates/ 폴더에 `hello-template.html` 파일을 만들어준다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Hello</title>
</head>
<body>
<p th:text="'hello. ' + ${name}" >hello!. empty</p>
</body>
</html>
```

이제 Spring Boot를 실행하고 name파라미터와 함께 주소를 입력하면

![](./images/05-01.png)

name 파라미터 값이 ${name}에 잘 들어간 것을 확인할 수 있다.

Spring Boot에서 MVC와 템플릿 엔진 처리 방식은 이렇다.

1. 내장 톰캣 서버가 요청을 받고 스프링한테 넘긴다.
2. 스프링은 컨트롤러를 살펴보는데 해당 주소가 맵핑 되어 있는 메소드가 있으니 실행한다.
3. model에 name이라는 키로 값을 할당해주고 hello-template를 스프링한테 보내준다.
4. 스프링이 viewResolver를 동작시킨다. viewResolver는 뷰를 찾아주고 템플릿 엔진을 연결시켜주는 역할이다.
5. viewResolver는 templates/ 에 html 파일을 찾고 템플릿 엔진한테 넘긴다.
6. 템플릿은 렌더링을 해서 변환된 파일을 클라이언트한테 보내준다.

정적 컨텐츠는 6단계에 변환하는 단계가 없다.

## **5-3. API**

API는 데이터만 보내주기에 다음 처럼 메소드를 짜면 된다.
```java
@GetMapping("hello-string")
@ResponseBody
public String helloString(@RequestParam("name") String name) {
    return "hello " + name;
}
```

여기서 @ResponseBody는 메소드가 return하는 값이 뷰 파일 이름이 아닌 http에 Body부에 들어가는 값을 의미한다.

Spring Boot를 실행하고 name 파라미터와 함께 /hello-string 주소를 입력하면
![](./images/05-02.png)

문자열이 잘나오는 것을 볼 수 있고 우클릭, 페이지 검사를 눌러보면
![](./images/05-03.png)

진짜로 문자열만 있는 것을 확인할 수 있다.

JSON을 보내줄 땐 자바에 객체를 보내주면 되는데<br>
아래 메소드를 추가로 입력해준다.

```java
@GetMapping("hello-api")
@ResponseBody
public Hello helloApi(@RequestParam("name") String name) {
    Hello hello = new Hello();
    hello.setName(name);
    return hello;
}

static class Hello {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

helloApi 메소드는 Hello라는 객체를 반환하는데 얘도 마찬가지로 name 파라미터와 함께 /hello-api 주소로 이동하면
![](./images/05-04.png)

잘 받아와지는 것을 볼 수 있다.

Spring Boot에서 API 동작 방식은 이렇다.

1. 내장 톰캣 서버가 요청을 받고 스프링한테 넘긴다.
2. 스프링은 컨트롤러를 살펴보는데 해당 주소가 맵핑 되어 있는 메소드가 있으니 실행한다.
3. 그런데 ResponseBody라는 어노테이션이 붙어있으니 스프링은 viewResolver는 동작하지 않고 httpMessageConverter가 동작한다.
4. 만약 메소드에 return 값이 단순 문자라면 StringConverter가 동작하고 객체면 JsonConverter가 기본으로 동작한다.
5. 요청한 클라이언트한테 보내준다.

기본 문자처리는 StringHttpMessageConverter가 하고<br>
기본 객체처리는 MappingJackson2HttpMessageConverter가 한다.<br>
(Jackson은 객체를 JSON으로 바꿔주는 라이브러리다.)

# **6. 회원관리 백엔드 개발**

## **6-1. 비즈니스 요구사항 정리**

- 데이터: 회원ID, 이름
- 기능: 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음 (스프링 특성을 잘보기 위해 이렇게 정함)

**"일반적인 웹 어플리케이션 계층 구조"**

![](./images/06-01.png)

- 컨트롤러: MVC에 컨트롤러이다.
- 도메인: 비즈니스 도메인 객체로, 회원, 쿠폰 등 DB에 저장되고 관리되는 도메인 객체이다.
- 서비스: '회원은 중복가입이 안된다.'등, 비즈니스 도메인 객체를 활용하여 핵심 로직들이 동적하도록 한 객체이다.
- 리포지토리: DB에 접근을 하며, 도메인 객체를 저장하고 관리한다.

**"클래스 의존관계"**

![](./images/06-02.png)

메모리 구현체는 개발을 해야하는 상황에서 DB가 정해지지 않았을 때 메모리를 DB대신 사용하는 것이다. <br>
나중에 DB로 교체하기 위해 인터페이스가 필요하다.

먼저 회원서비스를 구현하기 위해 src/main/java/(만들어진폴더)/ 에 domain 패키지를 만들고 그 안에 Memeber 클래스를 만든다. 그리고 Member 클래스는 다음처럼 코드를 짠다.

```java
public class Member {

    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
얘는 Member라는 DB에 관리되는 객체이므로 도메인 패키지에 저장하는 것이다.

회원리포지토리(인터페이스)는 src/main/java/(만들어진폴더)/ 에 repository 패키지를 만들고 MemberRepository 인터페이스를 만든다. 인터페이스 코드는 다움처럼 짠다.

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member);
    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);
    List<Member> findAll();
}
```
Optional은 Null 값으로 인해 생기는 예외를 피하기 위해 사용한다.
Optional에 감싸진 값이 Null이더라도 예외가 발생하지 않는다.

메모리 구현체를 만들기 위해 만들었던 repository 패키지 않에 MemoryMemberRepository 클래스를 만든다. 코드는 다음처럼 짠다.

```java
public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream().filter(member -> member.getName().equals(name)).findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
    
    public void clearStore() {
        store.clear();
    }
}
```

`store.clear()`는 store를 비우는 구문이다.