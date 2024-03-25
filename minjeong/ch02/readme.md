## 2장 스프링 부트3 시작하기

### 2.1 스프링과 스프링 부트

|            | 스프링   | 스프링 부트                          |
|------------|-------|---------------------------------|
| 설정파일       | 수동 구성 | 자동 구성                           |
| XML        | 테스트2  | 사용 x. 자바 코드로 모두 작성              |
| 인메모리 db 지원 | 지원 x  | 자동 설정 지원                        |
| 서버         | 수동 설정 | 내장형 서버 제공(톰캣,제티,언더토우 같은 WAS 내장) |
- WAS : 웹 애플리케이션 서버
- 스프링부트는 jar파일만 만들면 WAS 설정 없이 바로 애플리케이션 실행 가능

### 2.2 스프링 컨셉트

#### IOC
```
public class A{
 private B b; -- 코드에서 객체를 생성 x. 외부에서 받아온 객체를 b 에 할당
}
```
- 제어의 역전 : 다른 객체를 직접 생성하거나 제어하는 것이 아니라 외부에서 관리하는 객체를 가져와 사용하는 것

#### DI
```agsl
public class A{
    //스프링 컨테이너에서 B객체를 만들어 A 객체에 주입
    @Autowired
    B b;
}
```
- 의존성 주입 : 어떤 클래스가 다른 클래스에 의존
- IOC를 구현하기 위한 방법
#### 스프링 컨테이너
- 빈의 생명주기를 관리
- @Autowired같은 애너테이션을 사용해 빈을 주입받을 수 있게 DI를 지원

#### 빈
```agsl
@Componet //MyBean 클래스를 빈으로 등록
public Class MyBean{ }
```
- 스프링 컨테이너에서 관리하는 객체
- 빈 등록 방법 여러가지 : XML 파일 설정, @Component 애너테이션 추가

#### AOP
- 관점 지향 프로그래밍
- 프로그래밍에 대한 관심을 핵심 관점, 부과 관점으로 나누어서 관심 기준으로 모듈화 하는 것

### PSA
- 이식 가능한 서비스 추상화
- 스프링에서 db에 접근하기 위한 기술로 JPA,MyBatis,JDBC등이 있음. 여기서 어떠한 기술을 사용하든 일관된 방식으로 db에 접근하도록 인터ㅇ페이스를 지원하는 것
- WAS도 PSA의 한 종류. 톰캣이 아닌 언더토우, 네티와 같은 다른 곳에서 실행해도 기존 코드 그대로 사용 가능


### 2.4 스프링 부트3 코드 이해햐기

#### @SpringBootApplication
- 스프링부트 사용에 필요한 기본 설정을 해줌

#### @SpringBootConfiguration
- 스프링 부트 관련 설정을 나타냄
- @Configuration 상속해서 단들어짐

#### @ComponentScan
- 사용자가 등록한 빈을 읽고 등록
- @Component라는 애너테이션을 가진 클래스들을 찾아 빈으로 등록
- 보통은 Componet를 단독으로 사용하지 않고 감싸는 애너테이션을 사용

  | 에너테이션                        | 설명        |
    |------------------------------|-----------|
  | @Configuration               | 설정 파일을 등록 |
  | @Repository                  | ORM 매핑    |
  | @Controller, @RestController | 라우터       |
  | @Service                     | 비지니스 로직   |

#### @EnableAutoConfiguratoini
- 자동 구성을 활성화
- 스프링 부트 서버가 실행될 때 스프링 부트의 메타 파일을 읽고 정의돈 설정들을 자동으로 구성하는 역할

#### @RestController
```agsl
@RestController
public Class TestController{
    @GetMapping("/test)
    public String test(){
        return "hello world!";
    }
}
TestController를 라우터로 지정해 /test라는 GET 요청이 왔을 때 test() 메서드를 실행할 수 있도록 구성 
```
- 라우터 역할
- 라우터 : HTTP 요청과 메서드를 연결하는 장치
- @RestController == @Controller + @ResponseBody
- @Controller는 @Component 애너테이션을 소유