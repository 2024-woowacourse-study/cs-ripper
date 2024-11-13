### 자바 코드는 어떤 과정으로 실행되는가?
.java -> 컴파일(.class) -> JVM 로딩(클래스 로더)-> 실행(실행 엔진)

이때, 각 OS별 JVM이 있어서 "Write Once, Run Anywhere" 가능

> JIT(Just-In-Time) 컴파일러
> - 인터프리터의 단점을 보완하기 위해 도입
> - 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 **전체를 컴파일하여** **네이티브 코드로 변경**, 한번 컴파일된 코드는 **네이티브 코드로만 실행하는 방식** (네이티브 코드는 캐시에 보관)
> - 하나씩 인터프리팅하는 것과 캐시에 보관하기 때문에 한 번 컴파일된 코드는 빠르게 수행

### JVM의 구조
Class Loader, Runtime Data Area, Execution Engine

- Class Loader: 클래스 파일을 로딩
  - Loading: 클래스 파일을 로드
  - Linking: 클래스 파일을 사용하기 위해 검증하고, 기본 값으로 초기화하는 과정
  - Initialization: static 변수 초기화, static 블록 실행
- Runtime Data Areas:
  - Method Area: 클래스 정보, static 변수 등
  - Heap: 객체 저장
  - Stack: 메소드 실행과 로컬 변수
  - PC Register: 현재 실행 중인 명령어 주소
  - Native Method Stack: 네이티브 코드 실행

### JVM Class Loader의 특징

- 위임 모델
    - 자신에게 **클래스 로딩 요청이 들어오면 자신의 부모 클래스 로더**에게 클래스 로딩 요청을 보내고 **부모 클래스 로더가 클래스를 찾지 못하면** 자신이 클래스를 탐색
- 계층 구조
    - 상위 클래스 로더의 클래스는 **하위 클래스에서 볼 수 있지만** 그 반대는 불가능 하다.
    - 클래스 로더의 책임은 분리하고 클래스 로더는 자신이 책임지는 클래스를 로딩 가능
    - `class.getClassLoader().getParent())`

### 자바 GC
자바의 메모리 관리 방법으로 **Heap 영역에서 동적으로 할당했던 메모리** 중 필요 없게 된 메모리(= 가비지) 객체를 모아 **주기적으로 제거하는** 프로세스

- 가비지 판단 방법    
    ![image](https://github.com/user-attachments/assets/e1cfcae9-d032-406c-8f95-01b101583c5a)
    - 객체들은 실질적으로 Heap영역에만 생성, Method Area와 Stack에서는 Heap 영역에 생성된 객체의 주소만을 참조
    - 객체에 대해 레퍼런스가 있다면 Reachable로 구분되고, 객체에 유효한 레퍼런스가 없다면 Unreachable로 구분해 버리고 수거

### Minor GC 동작과정
Minor GC: Young Generation에 대한 GC

1. 새로 생성된 객체가 Eden 영역에 할당된다.
2. 객체가 계속 생성되어 **Eden 영역이 꽉 차게 되고** Minor GC가 실행된다.
    1. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동된다.
    2. Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
3. 1~2번의 과정을 반복하고 Survivor 0과 Survivor1을 왔다 갔다 한다.(1**개의 Survivor 영역은 반드시 빈 상태가 된다.**)
4. 이러한 과정을 반복하여 계속해서 살아남은 객체는 Old 영역으로 이동(Promotion)된다.

### Major GC 동작과정

Promotion: Young 영역에 의해 시작되었으나, GC 과정 중에 제거되지 않은 경우 age 임계값이 차게 되어 Old로 이동하는 것

Promotion가 자주 발생되어 Old 영역도 꽉 차게 되면 Major GC 발생

Major GC가 일어나면 Thread가 멈추고 Mark and Sweep 작업을 해야 해서 CPU에 부하를 줌

→ 최적화를 위한 다양한 GC 알고리즘 탄생

### Java GC 알고리즘

- Serial GC: 단일 스레드, 적은 메모리
    - Minor GC 에는 Mark-Sweep을 사용하고, Major GC에는 Mark-Sweep-Compact
- Parallel GC: 다중 스레드, 처리량 중시
    - Serial GC와 기본적인 알고리즘 동일 & Young 영역의 Minor GC를 멀티 스레드
- CMS GC: 애플리케이션 중단 최소화
    - 애플리케이션의 스레드와 GC 스레드가 동시에 실행되어 stop-the-world 시간을 최대한 줄이기 위해 고안
    - GC 대상을 파악하는 과정이 복잡한 여러 단계로 수행되기 때문에 다른 GC 대비 CPU 사용량이 높다
    - 메모리 파편화 문제
- G1 GC: 대용량 메모리, 짧은 GC 시간
    - G1 GC는 전체 힙 영역을 Region이라는 영역으로 체스판과 같이 분할
    - Heap Memory 전체를 탐색하는 것이 아닌 영역(region)을 나눠 탐색하고 영역(region) 별로 GC가 일어난다

### 인터페이스 vs 추상 클래스
인터페이스는 행위를 추상화할 때 사용하고 추상 클래스는 타입을 추상화할 때 사용합니다.

추상 클래스: 단일 상속만 가능, 상태와 생성자를 가질 수 있음

인터페이스: 다중 구현 가능, 상태를 가질 수 없음

### SOLID
코드의 유지보수성을 높히기 위한 원칙

- Single Responsibility: 한 클래스는 하나의 책임만
- Open-Closed: 확장에는 열려있고 수정에는 닫혀있어야 함
- Liskov Substitution: 하위 클래스는 상위 클래스를 대체할 수 있어야 함
- Interface Segregation: 인터페이스는 작게 분리
- Dependency Inversion: 구체적인 것이 아닌 추상화에 의존해야 함

### 동일성 vs 동등성

동일성 → 아예 동일한 객체

동등성 → 대등하다고 봐도 되는 객체

### equals와 hashCode의 관계

- equals와 hashCode는 항상 같이 오버라이딩해야 함
- 한쪽만 오버라이딩하면 해시 기반 컬렉션에서 문제가 발생 가능
  - HashMap, HashSet은 hash 값을 기준으로 hashCode()를 통해 먼저 버킷을 찾고, 버킷 내에서 equals로 동일한 값인지 판단하기 때문에

### 리플렉션
런타임에 클래스 정보, 변수 등 메타 정보를 알아내고 조작하는 것

### Java 버전별 차이점

Java 주요 버전별 특징

- Java 5: Wrapper Type, Enum, 제네릭, 어노테이션
- Java 8: Lambda, Stream API, Optional
- Java 11: var 키워드, HTTP Client
- Java 17: Sealed Classes, Pattern Matching, Record
- Java 21: Virtual Threads,
