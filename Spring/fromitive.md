### Spring vs Springboot

공통점 둘 다 프레임워크라서 개발 규격이 정해져 있음
스프링 : 비즈니스 로직을 처리하는 것을 집중하는 데 도와주는 프레임워크
스프링부트 : 톰켓과 같은 WAS가 내장되어 있어 빠르게 웹 어플리케이션을 만들어줄 수 있도록 도와주는 프레임워크

### Spring에서의 IOC

스프링 컨테이너가 Inversion of Control로서 객체의 생명주기를 스프링이 관리하는 것을 이야기 함. IoC가 있기에 개발자는 환경에 구애받지 않고 비즈니스로직에 집중할 수 있게 됨

> [INFO]
> 스프링 컨테이너란, 스프링 어플리케이션 실행에 필요한 설정과 객체들을 탐색한 후 관리해주는 주체라고 생각하면됨

#### DI

스프링 컨테이너가 객체가 필요한 의존성을 자동으로 주입하여 추상화에 의존하는 코드를 생성할 수 있게 도와줌 

#### Bean이 뭐냐

스프링이 관리하는 객체를 Bean이라고 함

#### 스프링 Bean의 생명 주기

1. 객체 생성
2. 의존성 주입
3. 빈 사용
4. 빈 소멸

#### 스프링 Bean Scope

- **singleton**: 한 번만 생성되어 애플리케이션 전체에서 공유됨.
- **prototype**: 요청마다 새로운 인스턴스를 생성함.
- **request**: HTTP 요청마다 새로운 인스턴스를 생성함 (웹 전용).
- **session**: HTTP 세션마다 새로운 인스턴스를 생성함 (웹 전용).
- **application**: 애플리케이션 전반에 걸쳐 공유됨.
- **websocket**: 웹소켓 연결마다 새로운 인스턴스를 생성함.


####  @Component vs @Bean

class에 붙일 수 있는게 Component, method에 붙일 수 있는게 Bean이며 

스프링 컨테이너가 ComponentScan 할때 먼저 스캔되어지는게 Component임 
Bean은 주로 Configuration어노테이션이 붙여진 클래스 안에 있는 메서드에 붙임으로써 어플리케이션 환경을 설정할때 사용

#### 그럼 @Bean은 항상 @Component 가 붙은 클래스 내에서만 사용해야 하는가?

Component가 붙은 하위 클래스인 @Controller, @Service, @Configuration에서도 사용 가능

#### Spring Web MVC 구조

Model : 비즈니스 로직을 처리하는핵심 데이터
View: 사용자에게 보여지는 화면 랜더링
Controller: 사용자가 보낸 요청 처리 모델 업데이트 View 호출

각 역할들이 나뉘어져 있어서 유지보수가 용이하다.



### Servlet

Java에서 Http Request를 처리하고 Http Response를 생성해주는 표준 기술
### Servlet Container

웹 Servlet을 생성 실행해주는 주체. 이 주체엔 WAS 혹은 Web서버 그 자체가 될 수 있음

### 프론트 컨트롤러 패턴?

모든 Http요청을 한 곳(스프링이면 DispatcherServlet)에서 처리하고 응답하는 것을 이야기함.

FrontController -> *Controllers

- 클라이언트가 `/example/hello`로 요청을 보냅니다.
- `DispatcherServlet`이 이 요청을 수신합니다.
- `DispatcherServlet`은 `HandlerMapping`을 통해 요청을 처리할 컨트롤러 메서드를 찾습니다.
- `HandlerAdapter`가 해당 컨트롤러 메서드를 호출하여 요청을 처리하고 `ModelAndView`를 반환합니다.
- `DispatcherServlet`은 `ViewResolver`를 사용해 뷰 이름에 맞는 뷰 파일을 찾습니다.
- 최종 뷰를 렌더링하여 클라이언트에게 응답을 반환합니다.

#### Servlet Filter vs Spring Interceptor

Filter : DispatcherServlet에 도달하기 전, 후에 처리됨. 예외 발생 시 ControllerAdvice를 사용할 수 없음
Interceptor: DispatcherServlet에 도달한 후 Controller 처리 전, 후 View랜더링 전에 특정 작업을 수행할 수 있음

#### Filter는 어떻게 Bean으로 등록되나?

1. `@Component` 어노테이션을 붙이고, Filter를 구현하면 됨
2. @Bean으로 FilterRegistrationBean를 이용해서 등록해도 됨 이 경우에는, 필터의 순서 url 지정이 가능해짐 


#### AOP와 프록시

비즈니스 로직과 전, 후 처리 로직을 분리하는 기법. 스프링에선 3가지 구성요소로 이루어짐

Aspect : 공통적으로 처리할 수 있는 로직을 정의하는 어노테이션
Advice : Aspect가 처리할 시점을 정의합니다. 메서드 처리 전, 후 리턴하기 전, 예외 발생 후 등등을 정의할 수 있음
Pointcut : 적용할 비즈니스 로직을 선별하는 곳입니다.

AOP를 사용하기 위해 Proxy 패턴을 사용할 수 있음

Proxy는 실제 비즈니스 로직을 처리하기 전이나 후 처리할 수 있게하는 디자인 패턴임. 서비스 인터페이스를 비즈니스 객체, 프록시 객체로 분리하고 프록시 객체가 비즈니스 객체를 구성하도록 만들면 비즈니스 로직 처리 전, 후에 원하는 작업을 수행할 수 있음

### Tomcat의 구성요소