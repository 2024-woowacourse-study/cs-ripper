# [Spring]

### Spring vs Springboot
    
두 차이점은 스프링 부트는 의존성 관리를 더 쉽게할 수 있습니다. was도 스프링 부트는 톰캣을 내장하고 있어서 별도의 설정을 하지 않아도 되지만 스프링은 was 설정이 필요합니다.
    
또 스프링에서는 빈 등록과 의존성 주입을 설정해주어야 하지만 스프링 부트는 AutoConfiguration를 지원합니다.
    
### Spring에서의 IOC
    
IoC는 제어의 역전으로 객체의 객체의 생명 주기를 개발자가 관리하는 것이 아닌 프레임워크가 관리하는 것을 말합니다.
    
스프링에서는 IoC 컨테이너가 빈 객체들의 생명 주기를 관리합니다.
    
### DI
    
의존성 주입은 객체의 의존성을 외부에서 주입받는 것
    
IoC 컨테이너에 있는 빈을 주입해줌
    
DI의 방법은 생성자, 필드, 세터 주입이있다.
    
필드 주입: final이여야 함. 때문에 불변성을 보장하기 어려워짐
    
세터 주입: 런타임 시에 의존성이 변경될 가능성이 있고, 선택적으로 주입 
    
생성자: 생성 시점 최초 1번만 주입. 불변 보장. 순환 참조 방지
    
### Bean이 뭐냐
    
스프링이 관리하는 객체로, 빈으로 등록하게 되면 자동 주입이 가능하다.
    
@Componet 어노테이션 또는 @Bean 어노테이션으로 주입
    
### 스프링 Bean의 생명 주기
    
객체 생성, 초기화, 사용 및 소멸로 구성
    
스프링 IoC 컨테이너 생성 → 스프링 빈 생성 → 의존관계 주입 → 초기화 콜백 메소드 호출→ 사용 → 소멸 전 콜백 메소드 호출 → 스프링 종료
    
- 초기화 콜백(init) : Bean이 생성되고, Bean의 **의존성 주입이 완료된 뒤 호출**된다.
- 소멸 전 콜백(destroy) : 스프링이 종료되기 전, **Bean이 소멸되기 직전에 호출**된다.
    
콜백 메서드: 빈의 생명주기 중 특정 시점에 자동으로 호출되는 메서드
    
### 스프링 Bean Scope
    
스프링 빈이 기본적으로 싱글톤 스코프로 생성
    
@Scope 어노테이션으로 설정 가능
    
싱글톤: 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프

프로토타입: 프로토타입 빈의 생성과 의존 관계 주입까지만 관여 (종료 메서드 호출 X)    
- 프로토타입 스코프인 경우, 싱글톤과 달리 항상 새로운 인스턴스를 생성하여 반환
    
웹 관련 스코프
    
- request: HTTP 요청당 하나의 인스턴스
- session: HTTP 세션당 하나의 인스턴스
- application: 서블릿 컨텍스트당 하나의 인스턴스

### @Component vs @Bean
    
@Component는 접근이 가능한 클래스 레벨에 등록, 주로 직접 구현한 객체에 사용
    
@Bean 외부 라이브러리 등 클래스 객체에 접근이 불가능한 경우에 등록 config 가 필요함
    
### 그럼 @Bean은 항상 @Component 가 붙은 클래스 내에서만 사용해야 하는가?
    
@Configuration가 붙은 클래스 내에서 등록이 가능
    
@Configuration에 사용하게 되면 CGLIB를 사용한 프록시가 생성되며, 같은 빈을 여러번 호출하더라도 같은 인스턴스를 반환한다.
    
### Spring Web MVC 구조
    
DispatcherServlet
    
![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/1430a828-8bfa-4acb-bfb9-0585b0e739c2/b4c1d32c-f13f-4956-b07f-e3abef45d083/image.png)
    
### Servlet
    
자바에서 웹 요청을 처리할 수 있도록 한 기술
    
HttpServlet 등의 클래스를 상속받아 사용할 수 있다.
    
서블릿들은 생성, 초기화, 소멸의 생명주기를 가진다.
    
### Servlet Container
    
요청을 처리하는 서블릿들을 담고 **컨테이너의 생명주기를 관리**하는 컨테이너
    
대표적으로 톰캣이 있다.
    
### Spring Web MVC의 Servlet 활용
    
Spring Web MVC는 웹 요청을 처리하는 프레임워크
    
스프링 mvc에서는 DispatcherServlet을 중심으로 처리하는데 DispatcherServlet는 모든 요청을 핸들러보다 앞에서 받는다.
    
### 프론트 컨트롤러 패턴?
    
모든 요청을 단일 진입점에서 처리하는 패턴
    
- 공통 로직의 중복 제거
- 일관된 요청 처리

### Servlet Filter vs Spring Interceptor
    
Client → Filter → DispatcherServlet → Interceptor → Controller
    
필터는 서블릿 컨테이너 앞단에 위치하였고 스프링 인터셉터는 더 뒤인 어플리케이션 컨텍스트 앞단에 위치하였다.
    
필터는 따라서 스프링 어플리케이션 컨텍스트 내부의 빈들에게 접근하지 못한다.
    
반면 인터셉터에서는 어플리케이션 컨텍스트의 빈들에게 접근이 가능하다.
    
### Filter는 어떻게 Bean으로 등록되나?
1. @Configuration + FilterRegisterationBean 빈 등록
2. Filter + @Component
3. @WebFilter
4. @WebFilter + @Component

### AOP와 프록시
    
AOP는 여러 모듈에 공통된 횡단 관심사를 분리하여 처리하는 개념
    
스프링에서는 프록시를 사용하여 AOP를 구현할 수 있다.
    
프록시는 객체의 대리자를 만들어서 직접 접근하지 않고 프록시 객체를 통해 간접적으로 접근하는 것
    
스프링 aop는 JDK 동적 프록시, CGLIB 프록시 기반의 프록시 aop 제공
    
### Tomcat의 구성요소
    
톰캣은 크게 5개의 컴포넌트로 이루어져 있다.
    
1. Server: **전체 컨테이너**
2. Service: 하나 **이상의 Connector와 하나의 Engine을 그룹화하는** 계층, Connector를 통해 받은 클라이언트의 요청을 적절한 Engine으로 전달
3. Connector: 클라이언트와 Tomcat 서버 사이의 통신을 담당하는 컴포넌트
4. Engine: Java 서블릿 API를 구현한 핵심 엔진
5. Host: **특정 도메인 이름에 대한 요청을 처리하는 가상 호스트**
