# Spring

### Spring vs Springboot

스프링은 자바르 기반으로한 대표적인 애플리케이션 프레임워크입니다. DI(의존성 주입)와 AOP(관점 지향 프로그램을) 핵심으로 코드의 재사용성과 유지보수성을 높이는 데 중점을 둡니다. 대규모 시스템 개발에 적합하며 기본적인 설정과 커스터마징의 자유도가 높습니다. 프로젝트 설정이 비교적 복잡하고 초기에 많은 XML 설정이 필요해 프로그램 구성이 번거로울 수 있습니다.

스프링 부트는 스프링의 복잡함을 해결하기 위해 등장한 쉽고 빠른 개발을 위한 확장판입니다. 자동 설정을 통해 필요한 설정들을 자동으로 적용하고 @SpringBootApplication 어노테이션 하나로 많은 기능을 활성화할 수 있어 프로젝트를 빠르고 쉽게 시작할 수 있습니다. 내장 서버(Tomcat 등)를 지원하고 있기 때문에 외부 서버 설정 없이 JAR 파일 형태로 배포가 가능해 배포와 운영 관리가 용이합니다. 빠른 개발과 배포를 요구하는 프로젝트시 사용하면 좋습니다.

### Spring에서의 IOC(Inversion of Control, 제어 역전)

스프링 프레임워크의 핵심 개념으로 객체 생성과 의존성 관리를 대신해 주는 기능입니다. 원래는 개발자가 직접 `new` 선언을 통해 객체를 생성하고 연결해야 하지만, IoC 컨테이너가 이 과정을 대신하면서 객체를 생성하고 필요한 의존성을 주입하며 생명주기를 관리합니다.
컴포넌트 간 결합도가 낮아지고, 코드의 모듈화와 유지보수가 필요합니다. IoC 덕분에 의존성이 필요한 객체들은 자동으로 주입되어 테스트 시에 필요한 객체로 대체할 수 있어 단위 테스트도 훨씬 간편해집니다.

1. 빈 등록 : 프로젝트 시작 시 @Component, @Service, @Repository 같은 어노테이션을 스캔해, 빈으로 쓸 객체들을 찾아 컨테이너에 저장합니다.
2. DI : 각각의 빈들이 의존성을 요구하면 IoC 컨테이너가 필요한 의존성끼리 연결해줍니다.
3. 싱글톤 관리 : 한 번 등록된 빈은 싱글톤으로 관리해 메모리를 절약하고 관리를 편하게 해줍니다.
4. 생명주기 관리 : 빈 생성부터 소멸까지 관리해줍니다.

### DI(Dependency Injection, 의존성 주입)

객체가 필요한 의존 객체를 스스로 만들지 않고 외부에서 주입받는 방식입니다. 객체가 직접 의존성을 관리하면 객체 간 결합도가 커져 코드 변경이나 유지보수가 어려워집니다. DI를 사용하면 의존성을 외부에서 주입받기 때문에 객체 간 결합도를 낮추고 더 유연하고 확장 가능한 코드를 만들 수 있습니다.
생성자 주입, 세터 주입, 필드 주입 방식이 있습니다.
스프링은 DI를 IoC와 함께 사용해 객체관 의존성을 외부에서 관리하고 주입합니다. 스프링 컨테이너가 객체를 생성하고 필요한 의존 객체를 주입해줍니다.

### Bean이 뭐냐

IoC 컨테이너에 의해 관리되는 객체입니다. 스프링 애플리케이션에서 사용할 객체들이 스프링 컨테이너에 등록되어 관리되는 형태를 Bean이라고 합니다.

### 스프링 Bean의 생명 주기

IoC 컨테이너가 빈을 생성하고 관리하는 과정입니다. 빈은 컨테이너가 시작될 때 생성되고, 의존성 주입, 초기화, 소멸 등의 과정을 거쳐 마지막에는 컨테이너가 종료될 때 파기됩니다.
스프링 빈의 생명 주기는 크게 `생성 -> 사용 -> 소멸` 단계로 나눌 수 있습니다.

1. 생성 : 스프링 컨테이너가 시작될 때 빈 정의에 따라 빈을 생성합니다.
2. 의존성 주입 : 빈이 생성되면 의존성 주입을 통해 빈이 필요한 다른 빈들을 주입합니다. (생성자 주입, 세터 주입, 필드 주입)
3. 사용 : 빈이 생성되고 초기화가 완료되면 사용할 준비가 완료되어 스프링 컨테이너가 필요할 때마다 가져와서 사용합니다.
4. 소멸 : 빈이 더 이상 필요 없거나 스프링 컨테이너가 종료될 때 소멸됩니다.

### 스프링 Bean Scope

스프링 컨테이너에서 언제 빈이 생성되고 어떤 시점에 빈이 공유되는지 정의하는 것입니다.

1. 싱글톤 : 기본 스코프 / 하나의 인스턴스만 생성되고 모든 요청에서 공유합니다. / 컨테이너에서 요청될 때마다 같은 객체가 반환됩니다.
2. 프로토타입 : 요청할 때마다 새로운 인스턴스를 생성합니다 / 상태를 갖지 않는 독립적인 객체가 필요할 때 유용합니다. / 컨테이너가 초기화만 스프링 컨테이너가 관리하고 이후에는 객체의 생명 주기를 관리하지 않습니다 -> 객체의 소멸 시점은 개발자가 관리해야 합니다.
3. 세션 : HTTP 세션을 기준으로 빈을 관리합니다. / 클라이언트별로 하나의 빈을 생성해 각 클라이언트의 HTTP 세션에 하나의 빈 인스턴스를 할당합니다.
4. 애플리케이션 : 애플리케이션 범위에서 빈을 관리합니다. / 웹 애플리케이션 전체 라이프사이클을 통틀어 빈이 하나만 생성되고 유지될 때 사용됩니다.
5. 리퀘스트 : HTTP 요청 단위로 빈을 관리합니다. / 요청이 끝나면 소멸 / 여러 요청이 동시에 있을 때, 각각 다른 빈을 사용할 수 있도록 분리할 수 있습니다.
6. 웹 소켓 : 웹 소켓 연결이 유지되는 동안 빈은 하나만 존재하고, 해당 연결을 사용하는 동안 유지됩니다.

### @Component vs @Bean

`@Component`와 `@Bean`은 스프링 프레임워크에서 객체를 스프링 컨텍스트에 등록하는 데 사용되는 어노테이션이지만, 사용 목적과 방식에서 차이가 있습니다.

#### @Component

클래스에서 이 어노테이션을 추가하면 스프링의 `@ComponentScan`에 의해 자동으로 검색 및 등록됩니다.

#### @Bean

수동으로 빈을 등록하는 방식으로 메서드 수준에서 사용됩니다.
`@Configuration` 어노테이션이 붙은 클래스에서 주로 사용하며, 해당 메서드가 **반환하는 객체**를 빈으로 등록합니다. 외부 라이브러리 클래스나 직접 컨트롤하기 어려운 객체를 빈으로 등록할 때 유용합니다.

### 그럼 @Bean은 항상 @Component 가 붙은 클래스 내에서만 사용해야 하는가?

`@Bean`으로 등록된 메서드가 스프링 컨텍스트에 의해 호출되어야 하므로 스프링이 관리하는 클래스 내에서만 `@Bean`을 사용할 수 있습니다.

### Spring Web MVC 구조

- Model-View-Controller

#### 모델(Model)

애플리케이션의 데이터나 비즈니스 로직을 담당합니다. 데이터베이스에서 가져온 데이터를 객체 형태로 표현하고 사용자의 요청을 처리하는 로직을 포함합니다. DTO나 Service 계층도 모델에 속합니다.

#### 뷰(View)

사용자에게 보여지는 화면을 담당합니다. 사용자 인터페이스를 구성하는 요소입니다. Thymeleaf 등
컨트롤러에서 데이터를 뷰로 전달하고 뷰는 이를 사용자에게 출력하는 기능을 합니다.

#### 컨트롤러(Controller)

사용자의 요청을 처리하고 적절한 모델을 선택해 뷰에 전달하는 역할을 합니다.
스프링에서는 `@Controller` 또는 `@RestController` 어노테이션을 사용하여 컨트롤러를 정의합니다.
`@RequestMapping` 등 어노테이션을 사용해 요청 URL을 매핑하고, 처리된 결과를 모델과 함께 뷰에 전달합니다.

#### 동작 흐름

클라이언트 요청 -> DispatcherServlet(HandlerMapping 사용해 URL 패턴에 맞는 컨트롤러 찾아 호출) -> 컨트롤러(결과 모델이 담아서 뷰ModelAndView에 전달) -> 뷰(ViewResolver) 컨트롤러가 반환한 뷰 이름을 실제 뷰 페이지로 전환 -> 응답

### Servlet

클라이언트의 요청을 처리하고 그에 대한 응답을 생성해 웹 브라우저와 같은 클라이언트에게 반환하는 역할을 합니다. 클라이언트의 요청에 대해 **동적**으로 작동하는 웹 애플리케이션 컴포넌트이며 html을 사용해 요청에 응답합니다. HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속 받습니다.

### Servlet Container

서블릿을 관리하는 컨테이너입니다. 클라이언트의 요청을 받고 응답을 할 수 있도록 웹서버와 소켓으로 통신하며 대표적으로 톰캣을 서블릿 컨테이너라고 할 수 있습니다. 웹서버와 통신을 지원하며 서블릿 생명주기를 관리합니다.

### Spring Web MVC의 Servlet 활용

`DispatcherServlet`이라는 핵심 서블릿을 사용해 클라이언트의 HTTP 요청을 처리합니다. 이 서블릿은 MVC 아키텍처에서 Controller와 View를 연결하는 중심적인 역할을 하며, Model을 처리하는 다양한 컴포넌트를 관리합니다.

1. DispatcherServlet
   - Spring Web MVC의 중심 서블릿
   - 요청을 받아 적절한 핸들러(Controller)로 전달합니다.
   - 요청에 따라 적절한 비즈니스 로직을 실행하고 실행 결과를 View로 전달합니다.

### 프론트 컨트롤러 패턴?

웹 애플리케이션 아키텍처에서 클라이언트의 모든 요청을 중앙에서 처리하는 디자인 패턴입니다. 웹 애플리케이션에서 요청 처리와 관련된 복잡성을 줄이고 일관된 방식으로 요청을 관리하기 위해 사용됩니다.
MSA에서 많이 사용됨 / MSA의 여러 서비스를 관통해서 이용하는 서비스들이 있을 때 클라이언트의 요청을 한 곳에서 처리하기 위해 프론트 컨트롤러 패턴을 많이 사용함.

### Servlet Filter vs Spring Interceptor

#### Filter

요청과 응답을 가로채서 처리하는 필터입니다. 요청이 서블릿으로 전달되기 전에 필터를 적용하여 요청을 수정하거나 응답을 변형할 수 있습니다.

#### Interceptor

요청이 컨트롤러에 도달하기 전에 또는 응답이 클라이언트로 전달되기 전에 처리 로직을 실행할 수 있는 기능을 제공합니다. Spring MVC 컨트롤러 실행 전후로 작업을 처리할 수 있습니다.

로깅 등 Filter는 전역적으로 모든 요청에 대해 일괄적으로 적용되어야 할 작업에서 사용됩니다. Servlet Filter는 요청을 처리하고 나서 응답을 수정할 수 있기 때문에 응답 헤더나 본문을 변경하는 데 사용할 수 있습니다. Interceptor는 응답 본문을 직접 수정하는 용도보다는 컨트롤러 실행 후 응답을 처리하는 데 사용합니다.

### Filter는 어떻게 Bean으로 등록되나?

@Configuration 어노테이션을 사용한 클래스에스 `FilterRegistrationBean`을 사용해 필터를 Spring Bean으로 등록할 수 있습니다.
스프링 부트에서는 `@WebFilter` 어노테이션을 사용해서도 등록할 수 있지만 `FilterRegistrationBean` 방식을 더 많이 사용합니다.

```java
@Configuration
public class FilterConfig {

	@Bean public FilterRegistrationBean<MyFilter> loggingFilter() {
		FilterRegistrationBean<MyFilter> registrationBean = new FilterRegistrationBean<>();
		registrationBean.setFilter(new MyFilter());
		registrationBean.addUrlPatterns("/api/*"); // 필터를 적용할 URL 패턴 설정
		registrationBean.setOrder(1); // 필터의 실행 순서 (숫자가 낮을수록 먼저 실행)

		return registrationBean; } }
```

```java
@WebFilter(urlPatterns = "/api/*")
public class MyFilter implements Filter {

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		// 필터 초기화
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// 필터 처리
		chain.doFilter(request, response);
	}

	@Override
	public void destroy() {
		 // 필터 종료
	}
}
```

### AOP와 프록시

#### AOP(Aspect-Oriented Programming)

관점 지향 프로그래밍으로 공통적인 기능을 여러 곳에서 사용할 수 있도록 분리해 재사용성을 높이고 코드 중복을 줄이는 방법론입니다. 부로깅, 트랜잭션 관리, 보안 등을 비즈니스 로직과 분리해 처리할 수 있습니다.

#### 프록시

다른 객체를 대신해서 특정 작업을 처리하는 객체입니다. AOP에서는 주로 프록시 패턴을 사용해 실제 객체를 감싸서 추가적인 기능을 제공하는 데 사용됩니다.

### Tomcat의 구성요소

[[10분 테코톡] 리브의 스프링 부트 내장 톰캣](https://youtu.be/UlF6o3Wbi2k?si=1wfqqCfInIclCv3Z)
