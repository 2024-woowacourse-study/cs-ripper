### 자바 코드는 어떤 과정으로 실행되는가?

    1. 컴파일
        1. 작성된 .java 파일의 소스 코드가 자바 컴파일러에 의해 컴파일된다
        2. 소스 코드는 .class 파일의 바이트코드로 변환된다
    2. 클래스 로딩
        1. 자바 프로그램이 실행되면 JVM이 클래스 로더를 사용해 필요한 .class 파일을 메모리에 로드
    3. 실행 엔진이 실행
        1. 가비지 컬렉터가 동작하면서 사용하지 않는 메모리를 정리


### JVM의 구조

    자바 어플리케이션을 실행하기 위한 가상 머신
    클래스 로더, 런타임 데이터 영역, 실행 엔진, JNI, 네이티브 메소드 라이브러리로 이루어져 있다.
    클래스 로더는 클래스 파일을 JVM 내로 동적으로 로드하고 링크하는 역할이다.
    로드란 클래스 파일을 JVM 메모리에 올리는 것, 링크는 클래스 파일 사용을 위해 검증하는 과정을 뜻한다.
    런타임 데이터 영역은 자바 애플리케이션을 실행할 때 사용되는 데이터들이 있는 영역이다. 메소드, 힙, 스택 등의 영역으로 이루어져 있다.
    실행 엔진은 바이트 코드를 실행하는 부분으로 인터프리터, JIT 방식으로 동작하며 가비지 컬렉터도 있다.
    JNI는 다른 언어로 만들어진 애플리케이션을 사용할 수 있도록 하는 인터페이스이다.
    네이티브 메소드 영역은 다른 언어로 작성된 라이브러리이다.

### 자바 GC
    Reachable은 객체가 참조되고 있는 상태, Unreachable은 참조되고 있지 않은 상태
    가비지 컬렉터란 힙 영역에서 동적으로 할당했던 메모리 중 필요 없게 된 메모리 객체를 주기적으로 제거하는 프로세스이다.
    동작하는 동안 오버헤드가 생기기 때문에 너무 자주 실행되면 성능이 하락된다. stop-the-world는 모든 애플리케이션을 멈추기 때문에 큰 오버헤드가 된다.
    가비지 컬렉션은 참조되고 있지 않은 객체를 추적하고 지우는 방식으로 동작한다.
    힙 영역을 Old, Young 영역으로 나누고 객체를 분류하며, 각 영역에 대해 GC를 실행한다. Old 영역은 Young 영역에 비해 더 큰 메모리를 갖고 있어 Major GC의 경우 훨씬 오랜 시간이 걸리며 stop-the-world를 발생시킨다.
    어플리케이션 개발자는 메모리 관리에 신경 쓰지 않고 개발을 신나게 할 수 있게 된다.

### 자바 GC 알고리즘
    - Serial GC: GC를 처리하는 스레드가 1개, stop-the-world 시간이 길다
        - Minor GC 엔 Mark-Sweep, Major GC에는 Mark-Sweep-Compact
    - Parallel GC: 위와 기본적으로 같지만 Young 영역의 Minor GC를 멀티 쓰레드로 수행
        - Java 8의 디폴트
    - Parallel Old GC: Young 영역 뿐 아니라 Old 영역에서도 멀티 쓰레드로 수행
        - Mark-Summary-Compact 방식을 이용
    - CMS GC: 어플리케이션의 스레드와 GC 스레드가 동시에 실행, stop-the-world 시간을 줄임
        - GC 과정이 매우 복잡해짐
        - GC 대상을 파악하는 과정이 복잡한 여러 단계로 수행되므로 다른 GC 대비 CPU 사용량이 높음
        - 메모리 파편화 문제
        - Java 9부터 deprecated 되었고 결국 Java 14에서는 사용 중지
    - G1 GC(Garbage First)
        - Java 9+ 버전의 디폴트
        - 기존 알고리즘과 달리 Young/Old 대신 Region이라는 개념 도입
        - 영역을 나눠 탐색하고 영역 별로 GC가 일어남(메모리가 많이 차 있는 영역을 우선)

### 인터페이스 vs 추상 클래스
    - 인터페이스: 클래스가 구현해야 하는 메서드의 집합을 정의
        - 모든 메서드는 public abstract만 (이후 default, static 가능해짐)
        - 모든 필드는 public static final 상수
        - 클래스에 다중 구현 지원
        - 인터페이스끼리 다중 상속 가능
        - 클래스와 별도로 구현 객체가 같은 동작을 한다는 것을 보장하기 위해 사용
    - 추상 클래스: 하위 클래스들의 공통점을 모아 추상화하여 만든 클래스
        - 단일 상속만 허용
        - 일반 클래스처럼 일반적인 필드, 메서드, 생성자를 가질 수 있음
        - 추상화하면서 중복되는 클래스 멤버들을 통합 및 확장할 수 있음
        - 클래스 간의 연관 관계를 구축하는 데에 초점
    - 공통점
        - 추상 메서드를 가지고 있어야 함
        - 인스턴스화 불가

### SOLID
    코드의 유지보수성, 확장성을 높이는 원칙이다.
    - SRP(단일 책임의 원칙): 클래스는 단 하나의 책임(기능)만 가져야 한다
        - 하나의 클래스에 책임이 여러 개 있다면 기능 변경 시 수정해야 할 코드가 많아짐
        - 한 책임의 변경으로부터 다른 책임의 변경의 연쇄작용을 억제할 수 있다.
    - OCP(개방 폐쇄 원칙): 확장에 열려 있고, 수정에 닫혀있어야 함
        - 새로운 기능을 추가할 때 클래스 확장을 통해 쉽게 구현할 수 있고, 변경 사항이 필요할 때 객체를 직접적으로 수정하는 것을 제한한다.
        - 추상화 사용을 통한 관계 구축을 권장
    - LSP(리스코프 치환 원칙): 서브 타입은 언제나 부모 타입으로 교체할 수 있어야 함
        - 항상 부모의 메서드 동작을 지원해야 한다
    - ISP(인터페이스 분리 원칙): 인터페이스를 각 사용에 맞게끔 잘게 분리해야 한다
        - 클라이언트의 목적과 용도에 적합한 인터페이스를 제공
    - DIP(의존 역전 원칙): 구현 클래스에 의존하지 말고 인터페이스에 의존한다
        - 클래스를 참조할 때 상위 요소인 추상 클래스, 인터페이스로 참조하라는 원칙

### 동일성 vs 동등성
    동일성은 두 객체가 완전히 같은 경우를 의미한다. == 연산자를 통해 판별 가능
    동등성은 두 객체가 같은 정보를 갖고 있는 경우를 의미한다. 객체가 다르더라도 내용만 같으면 동등성이 있다고 할 수 있다. equals 연산자를 통해 판별할 수 있다.
    
### 리플렉션
    클래스의 메소드, 타입, 변수들에 접근할 수 있도록 하는 자바 api이다. 동적으로 객체를 생성하거나 변수를 변경하고 메서드를 실행할 수 있다.
    런타임 시점에 현재 실행되고 있는 클래스에 대한 작업을 할 때 사용할 수 있다.
    ide의 자동 완성 기능, 스프링 어노테이션의 기능 등이 있다.
    
### Java 버전별 차이점
    - 5
        - 제네릭스
        - Enumeration(enum)
        - Annotation
        - Concurrency API: 스레드 풀, 동시성 컬렉션
        - Auto Boxing/Unboxing: 기본형 ↔ 래퍼 클래스
        - 향상된 for 루프
        - 가변 인자
    - 8
        - 람다 표현식
        - 스트림 api
        - 메서드 참조
        - 기본 메서드 interface
        - Optional
        - PermGen Area 제거
    - 11
        - HTTP 클라이언트 API 정식 포함
        - String 메서드 추가
        - 람다 표현식에 var 사용 가능
    - 14
        - switch 표현식 표준화
        - record
        - NullPointerExceptions: 어떤 변수가 null인지 설명해줌
    - 17
        - Sealed Class 정식
        - switch 문에서 패턴 매칭 지원
    - 21
        - Virtual Thread
        - SequencedCollection
        - Switch 문에 패턴 매칭 개선
