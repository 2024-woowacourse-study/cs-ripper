# [데이터베이스]

### 데이터베이스에서 인덱스를 사용하는 이유와 장단점
    
- 인덱스란 데이터를 정렬하는 키값을 말한다. 인덱스를 사용하게 되면 데이터의 정렬을 보장하기 때문에 조회 시에 범위 탐색을 빠르게 할 수 있다.
- 단점은 인덱스를 위한 메모리 공간이 추가적으로 생기기 때문에 리소스가 더 소비된다. 수정, 삭제, 생성 시에 인덱싱 작업에 대한 리소스가 든다. 인덱싱 작업은 B 트리 구조를 다시 균형을 맞추어야한다.
    
### InnoDB 인덱스 구조
    
InnoDB는 B 트리 구조를 사용해서 인덱스를 활용한다. B 트리 구조는 노드의 키가 여러 개인 균형트리를 말한다.
B+tree -> 노드들이 연결되어 있는 B tree

###  InnoDB 버퍼 풀, REDO로그, UNDO로그
    
- InnoDB의 버퍼풀은 인덱스나 데이터 캐싱에 사용되는 영역이다. 트랜잭션이 커밋 될 경우 버퍼풀을 거치게 된다.
    
- 리두로그는 버퍼풀의 데이터 유실을 방지하기 위해 사용. 장애 발생 시 복구에 사용되는 로그
    
- 언두로그는 백업을 위한 로그이다. 롤백이 발생할 경우 언두로그를 참조하여 데이터를 복구한다. 
    
### 트랜잭션이란?
    
트랜잭션은 독립적인 실행 단위를 말한다. 4가지 주요 특징으로는 ACID가 있다.
    
- A: atomic 으로 하나의 트랜잭션은 더이상 분리될 수 없는 단위이다. 즉, 전체가 실패하거나 전체가 성공되어야한다.
- C: consistency 는 트랜잭션을 실행하더라도 일관성이 있어야 한다. 즉, 트랜잭션 실행 후 데이터베이스의 규약을 어기게 되면 롤백되어야 한다.
- I: isolation은 각 트랜잭션이 격리되어야 한다는 것이다. 즉, 트랜잭션 여러개를 동시에 실행시켰을 때나 한 개씩 실행했을 때에 동일한 결과를 가져야 한다.
- D: durality는 트랜잭션은 지속되어야 한다. 즉, 트랜잭션의 실행 결과는 저장되어야 한다.
    
### 트랜잭션의 격리 수준
4가지가 있다. Read Uncommited, Read Commited, Repatable Read, SERIALIZABLE
    
- Read Uncommited는 하나의 트랜잭션에서 실행 중인 커밋되지 않은 작업을 다른 트랜잭션이 조회할 수 있는 것을 말한다.
  - 더티 리드: commit되지 않는 데이터가 조회되는 현상
- Read Commited은 하나의 트랜잭션에서 실행 중인 커밋만 트랜잭션이 조회할 수 있는 것을 말한다.
  - Non Repeatable는 한 트랜잭션 안에서 동일한 쿼리를 두 번 이상 실행했을 때, 다른 트랜잭션이 데이터를 변경(업데이트)하거나 삭제하여 결과가 달라지는 현상
- Repatable Read는 트랜잭션 내에서 한번 조회한 결과에 항상 동일한 값을 반환하는 수준이다. 모든 트랜잭션은 아이디 값을 가지는데, 데이터 변경 시 이 트랜잭션 아이디 값을 통해 구현할 수 있다.
  - 만약 조회하려는 데이터의 트랜잭션 아이디가 자신보다 작은 경우
  - Phantom Read는 동일한 조건의 쿼리를 실행했을 때, 다른 트랜잭션에서 데이터를 **삽입**(INSERT)하거나 **삭제**하여 결과 집합이 달라지는 현상
- Serializable 가장 높은 격리 수준으로 하나의 트랜잭션이 작업을 수행 중인 경우 다른 트랜잭션은 동일한 자원에 접근하지 못한다.

### MySQL의 Repeatable Read에서의 Phantom Read
    
MySQL에서는 Repeatable Read에서 Phantom Read가 발생하지 않는다. MySQL은 한 트랜잭션이 데이터를 삽입, 수정, 삭제하려는 경우 Next-Key Locking을 건다.
    
따라서 데이터를 변경할 수 없다.
    
### 데이터베이스 정규화란?
    
데이터의 중복을 제거하는 과정
    
- 1정규화: 한 개의 레코드에는 한 개의 값만 가지도록 하는 것    
- 2정규화: 기본 키의 부분집합이 결정자가 되지 않도록 분해하는 것
    
![image](https://github.com/user-attachments/assets/6c22c9bc-c0d7-4d4a-9231-3567e805a3b4)
    
![image](https://github.com/user-attachments/assets/62f8fca6-13fb-4041-a094-893d7183a7f7)
    
- 3정규화: A → B, B → C 일 때 A → C가 아니도록 하는 것
    
![image](https://github.com/user-attachments/assets/04006e59-f1f9-4e50-b40f-7d4e87ca2a29)
    
![image](https://github.com/user-attachments/assets/bbd401f8-eb17-4d96-b7c6-2b66e625fa1c)
    
### Join이란?
    
다른 테이블에 연결하는 것
    
inner join, outer join, 등
    
### RDBMS vs NoSQL
    
- RDBMS 는 저장하려는 데이터가 정형화되어 있는 경우
- NoSQL은 저장하려는 데이터가 정형화되어 있지 않는 경우, key-value, document 등
    
### CAP 정리(브루어 정리)
    
    대부분의 분산 시스템에 적용되는 정리
    
    - Consistency 데이터 일관성
        - 클라이언트가 어떤 노드에 접속했느냐에 관계 없이 언제나 같은 데이터를 봐야 한다.
    - Availability 가용성
        - 일부 노드에 장애가 발생하더라도 항상 클라이언트에게 응답을 줄 수 있어야 한다.
    - Partition tolerance 파티션 감내성
        - 네트워크에 파티션이 생기더라도 시스템은 계속 동작해야 한다.
### 파티셔닝과 샤딩
    
    파티셔닝이 테이블을 분할하여 저장하는 것이라면 샤딩은 동일한 스키마를 가지고 있는 데이터를 다수의 DB에 분산하는 것