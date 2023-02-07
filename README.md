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

## **6-2. 회원 리포지토리 테스트 케이스 작성**

테스트 케이스는 src/test/java/(만들어진폴더)/ 에 패키지, 클래스를 만들어서 할 수 있다.

repository 패키지를 만들고 안에 MemoryMemberRepositoryTest 클래스를 만든다.
![](./images/07-01.png)

MemoryMemberRepository에 대한 테스트케이스를 작성할 때<br>
그 클래스의 이름은 테스트를 하려는 클래스 이름 옆에 Test라는 글자를 붙여 짓는 것이 관례이다.

클래스는 다음처럼 적는다.

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.Optional;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clearStore();
    }

    @Test
    public void save() {
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        assertEquals(member, result);
        assertThat(result).isEqualTo(member);
    }

    @Test
    public void findByName() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring2").get();
        assertThat(result).isEqualTo(member2);
    }

    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2);
    }
}
```

여기서 assertEquals는 `org.junit.jupiter.api.Assertions.*`에 있는 메소드이고<br>
assertThat은 `org.junit.jupiter.api.Assertions.*`에 있는 메소드이다.

이 두개로 테스트한 메소드의 반환값과 Member 객체가 같은지 확인할 수가 있다.

그리고 테스트를 수행하는 메소드는 @Test라는 어노테이션을 달고 있어야한다.

@AfterEach 어노테이션은 테스트가 하나 끝날 때마다 메소드를 실행할 때 쓴다.

@AfterEach에 메소드에 repository 객체에 store 객체를 clear하는 구문이 있는데<br>
findByName과 findAll 메소드가 테스트하는 Member 객체들의 이름이 서로 같은게 존재하기 때문이다.

클래스 이름 옆에 실행 버튼을 눌러 모든 테스트 케이스들을 실행할 수 있다.

## **6-3. 회원 서비스 개발**

[6-1](#6-1-비즈니스-요구사항-정리)에 웹 어플리케이션 계층 구조를 살펴보면 마지막으로 서비스를 만들어야하는게 보인다.<br>1
서비스는 리포지토리와 비슷하게 DB에 데이터를 다루는 건 똑같다.<br>
하지만 리포지토리는 DB에 데이터를 생성, 수정, 삭제, 조회같은 직접적으로 DB에 데이터를 다룬다면<br>
서비스는 기획자나 다른 협업하는 사람도 알 수 있는 메소드 이름으로 리포지토리보다 더 비즈니스 적인 로직을 사용한다.

서비스도 리포지토리, 도메인 패키지와 같은 폴더 위치인 src/main/java/(만들어진폴더)/ 에 service라는 패키지를 만들고 MemberService라는 클래스를 만든다.

MemberService 클래스 코드는 다음처럼 짜면 된다.

```java
public class MemberService {
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /**
     * 회원 가입
     */
    public Long join(Member member) {
        // 같은 이름이 있는 중복 회원 X
        validateDuplicateMember(member); // 중복 회원 검증

        memberRepository.save(member);
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> { // null이 아닌 어떤 값이 있으면
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });
    }

    /**
     * 전체 회원 조회
     */
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId){
        return memberRepository.findById(memberId);
    }
}
```

join 메소드는 validateDuplicateMember 메소드를 통해 먼저 중복되는 이름이 있는지 확인한다.

validateDuplicateMember 메소드에 .ifPresent는 Optional에 붙는 메소드인데<br>
만약 Optional에 감싸진 값이 null이 아닌 값이라면 인자에 람다를 실행한다.

여기서 이름이 중복되는 멤버가 있는지 확인하고 만약 있다면 IllegalState 예외를 던진다.<br>
만약 중복되는 멤버가 없다면 memberrepository에 멤버를 저장하고 ID를 반환한다.

findMembers는 memberRepository에 모든 멤버를 반환하고<br>
findOne은 memberId로 멤버 한명을 찾는다.

## **6-4. 회원 서비스 개발**

MemoryMemberRepository 클래스처럼 MemberService 클래도 test 폴더에 테스트하는 클래스를 만들어주면 되는데 테스트하고자 하는 클래스에 ⌘ ⇧ T 를 입력하면 test 폴더에 테스트 케이스 구조를 바로 만들 수 있다.

추가로 프로덕션 코드에 메소드는 영어로 써야하지만 테스트 케이스에 메소드는 한글로 써도 상관이 없다.<br>
테스트 코드이기 때문에 빌드할 때 포함이 안되기도 하고 좀 더 직관적일 수 있기 때문이다.<br>
실제로도 협업할 때 한글로 많이 쓰기도 한다고 한다.

테스트 케이스 코드는 다음과 같다.
```java
class MemberServiceTest {
    MemberService memberService;
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach() {
        memberRepository.clearStore();
    }

    @Test
    void 회원가입() {
        /* 만약 테스트 코드가 매우 길땐 아래 패턴으로 짜면 알기 쉽다. */

        // given: 무언가 데이터가 주어졌고
        Member member = new Member();
        member.setName("spring");

        // when: 이걸로 실행했는데
        Long saveId = memberService.join(member);

        // then: 결과가 이게 나와야 해
        Member findMember = memberService.findOne(saveId).get();
        assertThat(findMember.getName()).isEqualTo(member.getName());
    }

    @Test
    public void 중복_회원_예외() {
        // given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");
        
        // when
        memberService.join(member1);

        // then
        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }

    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```
여기서 @BeforeEach는 @AfterEach와 반대로 각각의 테스트 케이스 메소드가 실행되기 전에 메소드를 실행시키는 어노테이션이다.<br>
@BeforeEach에 메소드를 보면 MemberService에 MemoryMemberRepository 객체를 넘기고 있는데 이에 따라 MemberService도 코드를 조금 수정해야한다,

MemberService에 MemberRepository 객체를 선언했던 코드를 다음처럼 변경한다.
```java
private final MemberRepository memberRepository;

public MemberService(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
}
```
MemberService 생성자가 외부에서 객체를 받고 그 객체를 MemberRepository의 객체인 memberRepository에 대입하고 있다.<br>
이런 방식을 DI(Dependency Injection)이라고 부흔다.

다시 MemberServiceTest 클래스로 돌아와서 각각의 테스트 케이스 메소드들을 살펴보면<br>
given, when, then이라는 주석이 보이는데 테스트 케이스를 작성할 때 이 주석과 함께 이런식으로 작성하고 다른 사람이 코드를 봤을 때 이해하기도 쉽고 좀 더 깔금해진다.<br>
의미는 주석에 나와있는대로다.

중복_회원_예외 메소드는 중복된 이름의 회원이 가입을 했을 때 IllegalStateException 에러가 나오는지 테스트하는 메소드다.

이 때 JUnit에 assertThrows를 쓰면 에러 테스트를 좀 더 수월하게 할 수 있다.<br>
첫번째 인자로 기대하는 에러, 두번째 인자로 에러가 나오는 실행문을 람다로 작성한다.<br>
코드에서 보이듯이 에러 객체를 반환하고 에러 메세지도 assertThat 구문을 통해 테스트하는 것을 볼 수 있다.

# **7. 스프링 빈과 의존관계**
## **7-1. 컴포넌트 스캔과 자동 의존관계 설정**

본격적으로 테스트가 끝났으니 위에서 만든 Member라는 도메인 객체와 MemberService, MemoryMemberRepository를 사용해볼 차례다.<br>
그전에 [6-1](#6-1-비즈니스-요구사항-정리)에서 보이듯이 마지막으로 컨트롤러를 만들어야한다.

기존에 만들었던 HelloController 클래스와 비슷하게 controller/ 폴더에 MemberController 클래스를 작성한다.<br>
```java
@Controller
public class MemberController {
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```
@Autowired라는 어노테이션이 보이는데 이 클래스 입장에서 @Autowired는<br>
스프링이 MemberController 클래스를 생성할 때 MemberController 생성자를 실행하고 인자에 객체는 **스프링 컨테이너에서에서 관리하는 객체를** 가지고 오는 역할이다.

이 말은 위에서 만들었던 서비스 클래스를 스프링 컨테이너가 관리하게 해줘야 한다는 말인데<br>
그러기 위해선 서비스 클래스도 컨트롤러와 비슷하게 클래스 위에 @Service라는 어노테이션을 달아줘야한다.
```java
// MemberService 클래스
@Service
public class MemberService {
    ...
}
```
이때 컨트롤러나 서비스 등의 어노테이션을 이용하여 스프링 컨테이너가 관리하게 만드는 방식을 **'컴포넌트 스캔'** 방식이라고 하고, 스프링 컨테이너가 관리하는 이런 자바 객체들을 **'스프링 빈'** 이라고 한다.

서비스에서 리포지토리 객체를 가지고오는 메서드도 아래처럼 수정하고
```java
@Service
public class MemberService {
    ...
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    ...
}
```
위에서 만들었던 리포지토리 클래스도 아래처럼 수정한다.
```java
// MemoryMemberRepository 클래스
@Repository
public class MemoryMemberRepository implements MemberRepository {
    ...
}
```

스프링 빈을 등록하는 방법에는 2가지가 있다.
그 첫번째가 위에서 했던 컴포넌트 스캔과 자동 의존관계 설정 방식이다.

이 방식은 @Component 어노테이션을 이용하여 스프링 빈을 만드는 방식이다.<br>
@Controller, @Service, @Repository도 해당 파일에 들어가보면 @Component 어노테이션이 붙어있다.

컴포넌트 스캔의 대상은 src/main/java/(만들어진 폴더)/ 바로 안래 애플리케이션 클래스가 있는 패키지 내부까지다.<br>
때문에 main/java/ 폴더에 어떤 패키지와 클래스를 만들고 어노테이션으로 스프링 빈을 만들려고 해도 만들어지지 않는다.

그리고 스프링은 기본으로 스프링 빈을 싱글톤으로 등록한다.<br>
즉, 유일하게 하나의 인스턴스만 등록하고, 이 인스턴스 하나를 공유한다.<br>
싱글톤이 아니게 설정할 수 있지만 보통 싱글톤으로 사용한다.

## **7-2. 자바 코드로 직접 스프링 빈 등록하기**


자바 코드로 직접 스프링 빈을 등록하기 위해서 먼저 컴포넌트 스캔 방식을 지워야한다.

MemberService와 MemoryMemberRepository 클래스에서 클래스명 위엥 @Service, @Repository 어노테이션을 지우고, MemberService에 @Autowired를 지운다.

src/main/java/(만들어진 폴더)/ 에 SpringConfig 라는 클래스를 하나 만들고<br>
아래처럼 코드를 짠다.
```java
@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```
`@Configuration`을 단 클래스는 빈 설정을 담당하는 클래스가 된다.<br>
그리고 `@Bean`을 단 메소드는 스프링이 호출해서 스프링 빈에 등록해준다.

MemberService 객체를 반환하는 메소드에 MemberSerice 생성자는 인자로 MemberRepository 객체를 하나 받기 때문에<br>
memberRepository 메소드를 안에서 호출해 객체를 받는다.

위에서 DI 방식을 보았는데 이 DI에는 3가지 방법이 있다.
- 필드 주입(Field Injection)
    ```java
    // MemberController
    @Controller
    public class MemberController {

        @Autowired
        private MemberService memberService;
    }
    ```
    필드에 바로 @Autowired 어노테이션을 다는 방식이다.<br>
    필드 주입 방식은 스프링 컨테이너 외에 외부에서 주입할 수 있는 방법도 없고 final로 선언할 수도 없기에 좋은 방법은 아니다.
- setter 주입
    ```java
    public class MemberController {

        private MemberService memberService;

        @Autowired
        public void setMemberService(MemberService memberService) {
            this.memberService = memberService;
        }
    }
    ```
    생성자가 생성된 후 @Autowired에 의해 호출 되어 주입되는 방식이다.<br>
    단점은 memberService를 세팅하는 메소드에 public이 붙어있어야 해서<br>
    한번만 호출돼도 되는 메서드가 굳이 노출된다는 것이다.
- 생성자 주입
    ```java
    // MemberController
    @Controller
    public class MemberController {
        private final MemberService memberService;

        @Autowired
        public MemberController(MemberService memberService) {
            this.memberService = memberService;
        }
    }
    ```
    기존에 위에서 했던 방식이다.<br>
    생성자 위에 @Autuwired 어노테이션을 달아서 객체를 주입하는 방식이다.<br>
    이 방법을 쓰는걸 권장한다.

주로 정형화된 컨트롤러, 서비스, 리포지토리는 컴포넌트 스캔, 정형화되지 않거나, 상황에 따라 구현 클래스를 변경해야할 땐 설정을 통해 스프링 빈으로 등록한다.

위에서 임시로 만든 MemoryMemberRepository를 교체할 때 설정을 통해 스프링 빈으로 등록한다면 코드에 손 안되고 교체할 수 있다.

그리고 @Autowired는 스프링 컨테이너가 관리하는 객체에서만 동작한다.
```java
// MemberController

public class MemberController {
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```
이 클래스 같은 경우, 컴포넌트 스캔이나 따로 설정으로 스프링 빈으로 등록하지도 않았기에 @Autowired는 무용지물이 된다.

# **8. 회원 관리 예제, 웹 MVC 개발**

## **8-1. 회원 등록**
회원 가입 기능을 만들기 위해 MemberController 클래스에
```java
// MemberController
@GetMapping("/members/new")
public String createForm() {
    return "members/createMemberForm";
}

@PostMapping("/members/new")
public String create(MemberForm form) {
    Member member = new Member();
    member.setName(form.getName());

    memberService.join(member);

    return "redirect:/";
}
```
코드를 추가하고 resources/templates/ 아래에 members 폴더와 createMemberForm html파일을 만들었다.<br>
createMemberForm.html 파일은 다음처럼 생겼다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Create Member Form</title>
</head>
<body>
  <div class="container">
    <form action="/members/new" method="post">
      <div class="form-group">
        <label for="name">이름</label>
        <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
      </div>
      <button type="submit">등록</button>
    </form>
  </div>
</body>
</html>
```
form 태그를 보면 action이 /members/new에 메소드가 post방식인 것을 볼 수 있고 input 태그에 name 속성의 값이 name인 것을 볼 수 있다.

MemberController에 보이는 @PostMapping은 Post 방식의 맵핑으로 Get과 반대로 클라이언트에서 데이터를 받을 때 사용한다.<br>
@PostMapping에 붙은 메소드는 MemberForm 객체를 받는데<br>
MemberForm 클래스는
```java
package hello.hellospring.controller;

public class MemberForm {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
프로젝트를 실행하고 /members/new 주소로 이동하면
![](./images/08-01.png)
createMemberForm.html 파일이 렌더링되는데 이름을 입력하고 등록버튼을 누르면<br>
/members/new로 post 요청이 가게 되고<br>
@PostMapping("/members/new")를 단 메소드에 인자인 MemberForm에 setName 메소드를 스프링이 호출해서 String name 필드에 name속성의 값이 name이었던 input 태그의 값을 넣게 된다.

때문에 MemberController 클래스에서 @PostMapping을 단 create메소드를 보면<br>
Member객체에 이름을 form에 name 필드의 값으로 설정하는 것을 볼 수 있다.

실제로 출력해보면 잘 나옴

이게 뭐 때문에 input에 name 속성 값을 가지고 MemberForm에 setter 메소드를 호출하는지 확인을 해보니 name값을 setter 메소드 명에 포함하고 있어야 감지한다는 것을 확인했다.<br>
만약 input에 name값이 name1이라면 MemberForm에 setter 메소드명은 setName1 또는 setname1이 되야 한다는 것이다.

## **8-2. 회원 조회**
회원 조회는 Get 요청만 하면 되기에 MemberController 클래스에서
```java
@GetMapping("/members")
public String list(Model model) {
    List<Member> members = memberService.findMembers();
    model.addAttribute("members", members);
    return "members/memberList";
}
```
@GetMapping만 추가해주면 된다.<br>
조회할 때 모든 멤버 리스트를 조회하므로 memberService.findMembers()로 멤버 리스트를 받고 model에 members라는 키로 Member 리스트인 members를 값으로 할당해준다.

members/memberList를 반환하므로 members/ 폴더에 memberList.html 파일을 살펴보면
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="https://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Member List</title>
</head>
<body>
<div class="container">
  <div>
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>이름</th>
        </tr>
      </thead>
      <tbody>
      <tr th:each="member : ${members}">
        <td th:text="${member.id}"></td>
        <td th:text="${member.name}"></td>
      </tr>
      </tbody>
    </table>
  </div>
</div>
</body>
</html>
```
처럼 생겼다.<br>
model에 키값을 할당했기에 thymeleaf 템플릿 엔진에서 th:each를 이용해서 값을 하나씩 불러오고<br>
Member 객체에 getter메소드인 getId와 getName가 있었으므로 member.id와 member.name으로 정보를 표시한다.<br>
(필드의 접근제어자가 있기 때문에 스프링이 필드가 아닌 getter메소드에 접근하는 것이다.)