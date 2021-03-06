# 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술
### 강사 : 김영한
### 강의 link : [인프런 inflearn.com](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#)

---

- 스프링 프로젝트 생성
- 스프링 부트로 웹 서버 실행
- 회원 도메인 개발
- 웹 MVC 개발
- DB 연동 - JDBC, JPA, 스프링 데이터 JPA
- 테스트 케이스 작성

### 0. 프로젝트 사용 기술

- Spring Boot
- Gradle : 필요한 라이브러리를 땡기고 빌드 주기를 관리해주는 Tool
- Tomcat
- Hibernate
- Thyneleaf

어떻게 스프링을 사용해야하는지?!?!

---

# 스프링 프로젝트 생성

main > java > 실제 소스

test > 테스트와 관련된 소스

`mavenCentral` 에서 기본적인 라이브러리 다운받아옴

```
12:43:42 오전: Executing task ':HelloSpringApplication.main()'...

> Task :compileJava
> Task :processResources
> Task :classes

> Task :HelloSpringApplication.main()

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.6)

2022-04-21 00:43:45.652  INFO 81392 --- [           main] h.hellospring.HelloSpringApplication     : Starting HelloSpringApplication using Java 16.0.2 on seunchoi-MacBookAir.local with PID 81392 (/Users/seunchoi/Desktop/JPA/hello-spring/build/classes/java/main started by seunchoi in /Users/seunchoi/Desktop/JPA/hello-spring)
2022-04-21 00:43:45.653  INFO 81392 --- [           main] h.hellospring.HelloSpringApplication     : No active profile set, falling back to 1 default profile: "default"
2022-04-21 00:43:46.023  INFO 81392 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2022-04-21 00:43:46.028  INFO 81392 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-04-21 00:43:46.028  INFO 81392 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.60]
2022-04-21 00:43:46.064  INFO 81392 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-04-21 00:43:46.065  INFO 81392 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 389 ms
2022-04-21 00:43:46.202  INFO 81392 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2022-04-21 00:43:46.210  INFO 81392 --- [           main] h.hellospring.HelloSpringApplication     : Started HelloSpringApplication in 0.909 seconds (JVM running for 1.102)
2022-04-21 00:44:26.851  INFO 81392 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2022-04-21 00:44:26.851  INFO 81392 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2022-04-21 00:44:26.852  INFO 81392 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
```

라이브러리 간 의존관계도 함께 땡겨옴

이전에는 WAS web server를 직접 설치해서 java 파일을 밀어넣음

요즘은 embeded로 웹서버를 띄움

- welcome page 기능

  [Spring Boot Features](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-welcome-page)

  위 메뉴얼에서 검색이 가능해야함

- thymeleaf 템플릿 엔진 [Thymeleaf](https://www.thymeleaf.org/)

  정적 페이지를 바로 넘겨주는 것이 아니라 뭔가 루프를 추가하거나 할 수 있음


# 스프링 기본 동작

```java
@Controller
public class HelloController {
    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```

- 뷰 리졸버가  `resources: templates` +{ViewName}+  `.html` 로 전환하여 화면을 연결함
- `spring-boot-devtools` 라이브러리를 추가하면, html 파일을 컴파일만 해주면 서버 재시작 없이
  View 파일 변경이 가능하다.

jar파일 하나 생성하고 서버에 올려서 실행하면 끝

# 스프링 웹개발 기초

MVC : Model, View, Controller

- 정적 컨텐츠 : 파일을(ex. HTML) 그대로 웹브라우저에 전달
- MVC와 템플릿 엔진 : 서버 레벨에서 뭔가 변경
- API : JSON으로 client에 데이터 전달 (서버끼리 통신할 때)

## 정적 컨텐츠

- 컨트롤러가 우선순위를 가짐
- 맵핑되는 컨트롤러가 없으면 내부 resources에서 html을 찾음
- 뭔가 html소스를 변경하지 않고 바로 서버에 전달

# MVC와 템플릿 엔진

⇒ html을 내린다

- model : 데이터
- view : 화면을 그리는데 역량 집중
- controller : 비즈니스 로직 처리

```java
@GetMapping("hello-mvc")
  public String helloMVC(@RequestParam("name") String name, Model model){
      model.addAttribute("name", name);
      return "hello-template";
  }
```

```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

## API

⇒ data를 바로 내린다

@ResponseBody : http에 포함해서 주겠다 ([https://cheershennah.tistory.com/179](https://cheershennah.tistory.com/179))

```java
@GetMapping("hello-api")
@ResponseBody
public Hello helloApi(@RequestParam("name") String name){
    Hello hello = new Hello();
    hello.setName(name);
    return hello;
}
```

JSON으로 넘겨줌

- 열고 닫는게 없어서 xml을 JSON이 이겼다

- viewResolver 찾지 않고 JSON방식으로 데이터를 만들어서 HTTP body에 직접 반환
- HttpMessageConvert가 작동함
    - 객체 - JsonConverter ⇒ **Jackson 라이브러리**
    - 그냥 문자열 - StringConverter
- 클라이언트의 HTTP Accept 해더와 서버의 컨트롤러 반환 타입을 통해 `HttpMessageConverter` 선택

## 백엔드 개발

- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)
    - repository를 interface로 만들고 구현체를 갈아낄 수 있도록

      (ex. RDB, NoSQL, JDBC, Mybatics, JPA...)

- 회원가입, 회원조회, 전체회원조회

## 테스트케이스 작성

- Junit : 테스트 코드 작성 프레임워크

```java
MemberService memberService;
MemoryMemberRepository memberRepository;

@BeforeEach
public void beforeEach(){
    memberRepository = new MemoryMemberRepository();
    memberService = new MemberService(memberRepository);
}

@AfterEach
public void afterEach(){
    memberRepository.clearStore();
}
```

```java
@Test
void 회원가입() {
    //given
    Member member = new Member();
    member.setName("hello");

    //when
    Long savedId = memberService.join(member);

    //then
    Member findMember = memberService.findOne(savedId).get();
    assertThat(member.getName()).isEqualTo(findMember.getName());
}
```

# 스프링 빈과 의존관계(DI)

- 스프링컨테이너 올라갈 때 어노테이션이 있으면 빈으로 등록하여 관리
- new 맨날하는게 아니라 주입 받아서 사용해야함

스프링 빈 등록

연결할 때 `@Autowired`를 쓴다 (선을 이어주는 역할)

### 1. 컴포넌트 스캔과 자동 의존관계 설정

- `@Component` 가 컨트롤러, 서비스, 리포지토리 다 붙어있다.
- 컴포넌트 관련 어노테이션이 있으면 스프링 컨테이너에 등록
- SpringBootApplication 하위의 디렉토리만 추척해서 빈으로 등록함

> 💡 **스프링에서의 싱글톤 디자인 패턴**
> - 싱글톤 : 유일하게 하나만 등록하여 공유
> 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다. 따라서 같은 스프링 빈이면 모두 같은 인스턴스다.
설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.


### 2. 자바 코드로 직접 스프링 빈 등록하기

```java
package hello.hellospring;

import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}
```

- 컨트롤러는 어쩔 수 없음. 컴포넌트 스캔으로 올려야함.

DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.

- setter 주입
    ```java
    package hello.hellospring.controller;
    
    import hello.hellospring.service.MemberService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    
    @Controller
    public class MemberController {
    
        private final MemberService memberService;
        
            @Autowired
        public void setMemberService(MemberService memberService){
            this.memberService = memberService;
        }
    }
    ```

- 생성자 주입 방식은 처음 조립 시점에 올려서 끝을 내야함 ← 이후에는 변경 못하도록 막는다
- 실무에서는 전형적인 컨트롤러, 서비스, 리포지토리는 주로 컴포넌트 스캔 사용
- 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록
  ⇒ 구현체 바꿔치기

- `@Autowired` 를 통한 DI는 helloConroller , memberService 등과 같이 스프링이 관리하는 객체(스프링 컨테이너에 올라가는 객체)에서만 동작한다. 
  - 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.
  - SpringConfig에 아무것도 없다면?! 직접 new로 memberService생성

# 웹 기능 추가

- 먼저 controller를 뒤지고 정적 리소스를 실행

## 스프링 DB 접근 기술

> 예제에서는 H2 Database 사용
- 스프링은 Dependency Injection에 특화되어있어서 갈아끼기가 편하다
- 개방-폐쇄 원칙(OCP, Open-Closed Principle)
    - 확장에는 열려있고, 수정, 변경에는 닫혀있다
- 다형성 : 구현체만 바꾼다

# Spring 통합 테스트

- `@SpringBootTest`
    - 스프링 컨테이너와 테스트를 함께 실행한다.
- `@Transactional`
    - 테스트 케이스에 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다.
    - 일반 서비스에 붙었을 때는 롤백하지 않는다.
    - DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
    - 테스트를 항상 같은 케이스로 중복 수행할 수 있다.

# Spring JDBC Template

- create

```java
SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

Map<String, Object> parameters = new HashMap<>();
parameters.put("name", member.getName());

Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
member.setId(key.longValue());
return member;
```

- select

```java
List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
return result.stream().findAny();
```

# JPA

- JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행하며 개발 생산성을 크게 높일 수 있다.
- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.
- JPA는 자바 진영의 인터페이스이고 그 구현체는 여러 업체가 있다. (ex. Hibernate)
- EntityManager를 주입 받아야함

    ```java
    private final EntityManager em;
    
    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }
    ```

- 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋한다. 만약 런타임 예외가 발생하면 롤백한다.

```java
@Override
public Optional<Member> findById(Long id) {
    Member member = em.find(Member.class, id);
    return Optional.ofNullable(member);
}

@Override
public Optional<Member> findByName(String name) {
    // pk 기반이 아닌 것은 query를 작성해야함
    List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
            .setParameter("name", name)
            .getResultList();

    return result.stream().findAny();
}
```

# Spring Data JPA

> 스프링 데이터 JPA는 JPA를 편리하게 사용하도록 도와주는 라이브러리

- 스프링 데이터 JPA가 SpringDataJpaMemberRepository 를 스프링 빈으로 자동 등록

    ```java
    @Configuration
    public class SpringConfig {
    
        private final MemberRepository memberRepository;
    
        @Autowired
        public SpringConfig(MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
        }
    
        @Bean
        public MemberService memberService(){
            return new MemberService(memberRepository);
        }
    
    //    @Bean
    //    public MemberRepository memberRepository(){
    //        return new MemoryMemberRepository();
    //        return new JdbcMemberRepository(dataSource);
    //        return new JdbcTemplateMemberRepository(dataSource);
    //        return new JpaMemberRepository(em);
    //    }
    }
    ```

- 공통화 할 수 있는 것은 모두 공통화해서 인터페이스를 통한 기본 CRUD 제공
    - extends 이후는 넣어줘야함

    ```java
    public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
        @Override
        Optional<Member> findByName(String name);
    }
    ```

- 프록시로 구현체 생성
    - 인터페이스만 만드니까 개발이 끝남
    - 구현체는 내가 안함

# AOP

- 공통 관심사항 (cross-cutting concern)
- 핵심 비즈니스와 공통 관심사항이 섞이면 유지보수가 힘들다
- 수정이 필요하면 다 찾아서 수정해줘야한다

```java
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))") // 타게팅, 어디에 AOP 적용할거야?
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START : " + joinPoint.toString());
        try{
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}
```

- 가짜 spring bean을 올림 (프록시)
- 대상으로하는 컴포넌트 injection받을 때 확인해볼 수 있음

    ```java
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
        System.out.println("memberService = " + memberService.getClass());
    }
    ```

    - DI : 뭔지 모르겟고 난 그냥 받아서 쓸게
    - 자바 코드를 앞뒤로 붙여주고 그런 기술도 있음...