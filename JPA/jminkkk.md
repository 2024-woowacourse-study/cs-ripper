# [JPA]

### JPA란?

JPA 란 자바 진영에서 사용하는 대표 ORM(객체와 데이터베이스 간의 매핑 기술) 기술을 말합니다. 

### JPA 사용 이유

JPA는 추상화된 인터페이스를 제공하기 때문에 개발자가 반복되는 쿼리문를 별도로 신경쓰지 않아도 됩니다.

- 기본적인 crud를 별로 쿼리로 작성할 일이 많지 않음
- sql 구조를 자바 애플리케이션에서 적용하지 않아도 됨
    - ex. order 클래스 안에 member 클래스가 있는 경우 memberId만 가지고 있는 것이 아닌 @ManyToOne등을 통해 객체를 가지고 있을 수 있음

### JPA 영속성 컨텍스트

애플리케이션과 데이터베이스 사이에서 객체를 보관하는 가상의 공간으로, 엔티티 매니저를 통해 엔티티를 저장하거나 조회하면 영속성 컨텍스트에 엔티티를 보관,관리합니다.

엔티티의 생명주기?

1. 비영속
    1. 영속성 컨텍스트와 전혀 관계가 없는 상태
2. 영속
    1. 영속성 컨텍스트에 저장된 상태
3. 준영속
    1. 영속성 컨텍스트에 저장되었다가 분리된 상태로 식별자 값을 가지고 있다
    2. EntityInformation.isNew 에서 false를 반환합니다.
4. 삭제
    1. 엔티티가 삭제된 상태

![image](https://github.com/user-attachments/assets/1c726b3a-7670-420d-9bd5-db19101b4e84)

**영속성 컨텍스트가 보장하는 것: 1차 캐시, 동일성 보장, 쓰기 지연, 더티 체킹, 지연 로딩**

**1차 캐시 → 1차 캐시에서 엔티티를 찾고 없는 경우 데이터베이스를 조회, 조회한 엔티티를  1차 캐시에 다시 저장**

**동일성 보장** → 

```java
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");
System.out.print(a==b) // true
```

**쓰기 지연 →  save 메서드를 실행한 즉시 db에 쿼리나가는 것이 아닌 트랜잭션이 커밋되는 시점에 보낸다.**

**변경 감지** **→** 영속성 컨텍스트가 관리하는 영속 상태일 경우 엔티티를 변경하게 되면 변경 감지가 발생하여 수정 쿼리가 발생

### JPA N+1 문제

엔티티를 조회할 때 연관관계에 있는 엔티티까지 그 갯수만큼 n번 조회해오는 현상

지연 로딩과 즉시 로딩 각각에서 발생됩니다.

즉시 로딩

- Jpql로 전달되는 과정에서 Jpql 후 Eager 감지로 인한 N쿼리가 추가로 발생하는 경우가 있기 때문에 사용해서는 안된다.
- 즉시 로딩과 함께 jpql을 사용하는 경우에 발생(findById, **엔티티 매니저**의 `find`함수는 내부적으로 최적화되어 ㄱㅊ)
- 다른 쿼리 메서드나 직접 작성한 jpql은 jpql 문 그대로 쿼리가 발생하여 대상 엔티티를 조회해온다. 그 과정에서 연관관계의 엔티티에 대해 즉시로딩이 발동

지연 로딩

- 대상 엔티티를 조회해올 때 연관된 엔티티는 프록시로 가져오는데 이때 연관된 엔티티에 접근하게 될 경우 발생한다.
    - fetch join를 통해 연관관계의 엔티티까지 한번에 조회

### JPA N+1 문제의 해결

1. fetch join
    1. 엔티티를 조회할 때 연관된 엔티티를 함께 조회할 수 있습니다.
2. @BatchSize
    1. 특정 엔티티를 가져올 때 연관관계에 있는 엔티티를 지정한 사이즈만큼 즉시 로딩할 수 있습니다.
3. @EntityGraph
    1. fetch을 직접 작성하지 않고 어노테이션으로 fetch할 엔티티를 선언해주면 됩니다.

### JPA Transaction 전파

spring jpa에서는 transaction을 전파 속성을 통해 합류 여부를 관리할 수 있습니다.

전파 속성이란 **이미 한 트랜잭션이 실행 중일 때 추가 트랜잭션을 어떻게 처리할** 것인지에 대한 결정

예를 들어 transaction의 전파 속성이 requires_new인 경우 기존 트랜잭션에 새로 합류하지 않고 새로운 트랜잭션을 만들게 됩니다.

반면 기본값인 required는 기존에 존재하는 트랜잭션에 추가된 트랜잭션이 참여하게 됩니다.

### OSIV

영속성 컨텍스트를 뷰까지 열어두는 기능으로 영속성 컨텍스트가 유지되면 엔티티도 영속 상태로 유지된다.

**트랜잭션을 시작할 때, 데이터베이스 커넥션을 가져오게 되는데 osiv가 켜져있는 경우 응답이 반환되기 전까지 커넥션이 반환되지 않는다.**