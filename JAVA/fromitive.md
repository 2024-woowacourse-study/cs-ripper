**Java 코드는 어떤 과정으로 실행되는가?**
* `.java` 파일을 `javac` 명령어로 컴파일 하여 `.class`파일로 만든다.
* `.class`파일을 `java` 명령어로 실행한다. 내부엔 `.class` 안의 바이트 코드는 JVM 실행 엔진이 해석하여 코드를 실행한다.
* 자주 실행하는 명령어는 실행엔진 안의  JIT가 코드를 실행하고, 나머지는 인터프리터가 해석한 후 실행한다. 

**JVM의 구조**
* Class Loader
	* Loading : `.class`파일을 메모리에 적제한다. 이때 바이트 코드는 Method Area에 저장한다.
	* Linking : 바이트 코드를 검증, static 변수 메모리 할당, 심볼릭 레퍼런스를 메모리 레퍼런스로 변환시키는 역할을 한다.
	* Initializing : static변수를 초기화 한다.
* Execution Engine
	* Interpreter
	* JIT Compiler
	* Garbage Collector
* Runtime Data Area
	* Class area(Method Area) : 클래스 단위로 실행할 바이트 코드가 적재되어 있는 영역
	* Heap : 객체가 인스턴스화 될 때 사용하는 메모리 공간
	* Stack : 함수 호출 리턴 주소, 매개변수, 지역변수 저장
	* Program Counter Register : 다음에 실행할 명령어 주소값 저장
	* Native Method Area : C, C++ 코드로 실행할 코드 저장

**Java GC**
* 메모리를 절약하기 위해 더이상 사용하지 않은 메모리 공간 할당을 자동으로 해제하는 과정을 Garbage Collection이라고 말함
* 사용자가 메모리를 직접 관리할 필요 없음
* 그러나 어느시점에 갈비지 컬렉션이 일어날지 몰라서 전체 갈비지 컬렉션을(대청소)를 하는 경우가 있는데 이를 Stop the world라고 함 

**Java GC 알고리즘**
* Mark - 객체가 더이상 참조되고 있지 않으면 제거 대상이 된다.
* Sweep - Mark된 객체에 할당 된 메모리를 해제한다.
* Compaction - 해제된 메모리 공간들을 정리한다. 

Serial GC - 단일 스레드로 동작하는 GC. JVM이 Stop-the-world를 하고 한번에 GC 하는 과정
Parallel GC - 여러 개의 스레드를 사용하여 GC 작업을 병렬로 수행. 그러나 Stop-the-world는 발생할 수 있음
CMS(Concurrent Mark-Sweep) GC - Mark-Sweep을 동시에 해서 Stop-the-world시간을 줄임. Compaction 단계는 없어 메모리 단편화가 발생할 수 있음
G1 GC - CMS GC의 업그레이드 버전. Region을 이용해 해결 메모리 단편화 문제를 해결
* Region: 이전에 Heap은 OG, YG가 있는데 이를 더 작게 나눈 단위가 Region이라고 함 각 Region안에 또다른 OG,YG가 있음
* Region에서는 Mark-Sweep-Compaction이 한번에 일어남
* Java 8부터 지원
ZGC - 10ms응답을 목표로한 초고속 GC (Mark-Sweep)단계는 포함하지만 Compaction은 점진적으로 수행해서 Stop-the-world시간이 짧음
Shenandoah(셰넌도어) GC: 모던 GC.. 초저지연을 목표로 둠 G1과 유사하게 Region단위로 GC를 동시에 함. (Mark-Swepp-Compection)이 거의 동시에 진행됨

**인터페이스 vs 추상 클래스**
* 인터페이스는 전부를 구현해야함
	* 구현체가 존재하지 않기 때문?
* 추상 클래스는 일부분이 구현되어 있고 나머지는 상속 받아 구현해야 함

**SOLID**
* 단일 책임 원칙
* 개방 폐쇄의 원칙: 구조는 닫혀있고 확장에는 열려있어야 한다.
* 리스코브 치환 원칙: 자식 객체의 동작은 부모 객체로 치환해도 동일하게 동작해야 한다.
* 인터페이스 분리 원칙: 클라이언트는 자신이 이용하지 않는 메서드에 의존해서는 안된다.
* 의존성 역전 원칙 : 상위 계층이 하위 구현 객체에 의존하지 않은 원칙

**동일성 vs 동등성**
* 동일성 : 할당받은 메모리 주소값 + 데이터 일치
* 동등성: 메모리 주소값은 일치하지 않지만 데이터 일치

**리플렉션**
* 객체에 대한 메타데이터 정보를 가져오는 데 사용( 클래스, 메서드, Argument, Parameter 등등)

**Java 버전별 차이점**



### References

https://medium.com/nerd-for-tech/java-virtual-machine-jvm-architecture-87b5bdd47403