# Java

### Java 코드는 어떤 과정으로 실행되는가?

#### `1` 컴파일

자바 소스 파일(.java)은 자바 컴파일러에 의해 바이트 코드(.class)로 변환됩니다.

#### `2` 클래스 로딩

JVM은 Class Loader를 통해 바이트코드를 메모리에 로드하고, 클래스의 메타데이터를 분석해 준비합니다.

#### `3` 런타임 인터프리팅 및 JIT 컴파일

JVM은 바이트코드를 인터프리터와 JIT(Just-In-Time) 컴파일러로 실행해 성능을 높입니다.

#### `4` 실행

JIT 컴파일된 네이티브 코드가 CPU에서 실행되어 최종적으로 프로그램이 수행됩니다.

### JVM의 구조

#### 클래스 로더 시스템

클래스를 로드하고 초기화하는 역할을 담당합니다.

#### 메모리 영역

JVM의 메모리는 메소드 영역(클래스 메타데이터), 힙(객체 저장), 스택(메소드 호출 및 변수), 네이티브 메소드 스택, PC 레지스터로 나뉩니다.

#### 실행 엔진

바이트코드를 해석하고 실행하며, JIT 컴파일러를 통해 네이티브 코드로 변환해 성능을 최적화합니다.

#### 네이티브 인터페이스(Native Interface)

외부 라이브러리(C, C++)와 상호 작용하기 위해 JNI(Java Native Interface)를 사용합니다.

#### 가비지 컬렉터

메모리 관리를 위해 사용되지 않는 객체를 자동으로 회수해 메모리 누수를 방지합니다.

### Java GC

JVM이 자동으로 메모리 관리를 해주는 시스템이로, 더 이상 참조되지 않는 객체를 찾아 메모리에서 제거합니다. 주로 힙 영역에서 사용되지 않는 객체를 수거하며, 개발자는 명시적으로 메모리를 해제할 필요가 없습니다.

### Java GC 알고리즘

- **Serial GC**: 단일 스레드를 사용하여 메모리 회수를 진행합니다. 작은 애플리케이션에 적합합니다.
- **Parallel GC**: 여러 스레드를 사용해 멀티 코어 환경에서 빠르게 GC를 수행합니다.
- **CMS(Concurrent Mark-Sweep) GC**: 힙을 계속해서 모니터링하면서 필요할 때 메모리를 회수합니다. 멈추는 시간을 최소화하기 때문에 반응이 중요한 애플리케이션에 적합합니다.
- **G1(Garbage First) GC**: 대용량 힙을 작은 영역으로 나누어 점진적으로 수거하는 방식으로, 최근 JVM의 기본 GC로 자주 사용됩니다.
- **ZGC**: 대용량 메모리에서도 멈추지 않는(low-latency) 수집을 목표로 하며, 최신 JVM에서 지원합니다.

### 인터페이스 vs 추상 클래스

- **인터페이스**: 메소드의 시그니처만 선언하며, 구현은 상속받는 클래스에서 합니다. 다중 상속이 가능하며 `default`, `static` 메소드를 가질 수 있습니다.
- **추상 클래스**: 일부 메소드를 구현할 수 있으며, 상태(필드)도 가질 수 있습니다. 단일 상속만 가능하며, 상속받는 클래스에서 필드를 공유해야 할 때 유용합니다.

### SOLID

- **S**: 단일 책임 원칙 (Single Responsibility Principle) – 클래스는 하나의 책임만 가져야 합니다.
- **O**: 개방-폐쇄 원칙 (Open-Closed Principle) – 확장에는 열려 있고 수정에는 닫혀 있어야 합니다.
- **L**: 리스코프 치환 원칙 (Liskov Substitution Principle) – 자식 클래스는 부모 클래스의 행동을 일관되게 유지해야 합니다.
- **I**: 인터페이스 분리 원칙 (Interface Segregation Principle) – 클라이언트가 사용하지 않는 인터페이스는 구현하지 말아야 합니다.
- **D**: 의존성 역전 원칙 (Dependency Inversion Principle) – 고수준 모듈이 저수준 모듈에 의존하지 않고, 둘 다 추상화에 의존해야 합니다.

### 동일성 vs 동등성

- **동일성 (\==)**: 두 객체의 메모리 주소가 같은지 비교하며, 기본형 타입은 실제 값을 비교합니다.
- **동등성 (`equals` 메소드)**: 두 객체의 내용이 같은지 비교하며, 객체의 상태나 값이 같은지를 기준으로 합니다.

### 리플렉션

런타임에 클래스, 메소드, 필드의 정보를 조회하거나 동적으로 접근하는 기능입니다. 예를 들어, 프레임워크에서 리플렉션을 통해 개발자가 작성한 클래스를 로딩하고, 메소드를 호출하거나 필드 값을 조작할 수 있습니다.

### Java 버전별 차이점

- **Java 8**: 람다 표현식, 스트림 API, `Optional`, `default` 메소드 추가 등.
- **Java 9**: 모듈 시스템, `jshell` 추가, JDK 구조 변경.
- **Java 10**: 지역 변수 타입 추론 (`var`).
- **Java 11**: 표준 LTS 버전, 여러 유틸리티 메소드 개선, `HttpClient` 추가.
- **Java 12-13**: 스위치 표현식(switch expressions) 실험적 추가, 텍스트 블록 지원.
- **Java 14**: 레코드(Record), instanceof 패턴 매칭 도입.
- **Java 15**: `sealed` 키워드로 상속 제한, Text Blocks 정식 도입.
- **Java 17**: LTS 버전으로, `sealed` 클래스와 패턴 매칭 등 정식 도입.
