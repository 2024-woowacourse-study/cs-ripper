## Spring vs Spring Boot

1. Dependency
    1. 스프링 부트가 의존성 주입, 버전 관리가 더 쉬움.(XML 안 써도 됨)
    2. 모든 Dependency를 일일히 관리하지 않고, 묶음으로 사용할 수 있음
        1. e.g. spring-boot-starter-web, spring-boot-starter-test vs hamcrest, mockito, junit, springtest 등등…
2. Configuration
    1. 스프링은 빈 관리를 일일히 해줘야 하지만, 스프링 부트는 application.properties 또는 yml 파일로 설정할 수 있음
    2. 스프링부트의 AutoConfiguration(@SprintBootApplication)
        1. ComponentScan - component, controller, service, repository 어노테이션 붙은거 자동으로 빈등록
    3. @EnableAutoConfiguration
        1. 컴포넌트 스캔 후 사전 정의된 라이브러리를 빈 등록한다.
        2. spring-boot-autoconfigure-META_INF-spring.factories에 있는 라이브러리들이 등록된다.
3. 배포가 편리
    1. Spring은 war파일을 WAS에 담아 배포했는데, Spring Boot는 Tomcat이나 Jetty 같은 내장 WAS가 있어 jar파일로 간단히 배포가 가능하다.

## Spring IOC

- Inversion of Control, 제어의 역전
- 프로그램의 제어 흐름을 각 객체가 관리하는 것이 아니라, 외부에서 관리하는 것
- 원래는 프로그램의 구현 객체가 스스로에게 필요한 객체를 직접 생성했다.
- Spring은 AppConfig에서 객체 선택과 같은 제어 흐름을 가져감.

## DI

- 의존관계 주입
- 외부에서 구현 객체를 생성하고 클라이언트에 주입한다.
- 정적 클래스 의존관계 : 프로그램을 실행하지 않아도 코드만 보고 알 수 있는 의존 관계
- 동적 클래스 의존관계 : 런타임에서 생성된 객체 인스턴스의 참조가 연결된 의존 관계
