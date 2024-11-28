
## JPA란?
java 코드와 Relational Database의 패러다임 불일치를 완전히 해결하는 것은 아니고,
분리된 계층간의 데이터를 매핑하는데 도움을 주는 도구임

패러다임 불일치를 어느정도 위해 제공되는 기능은 아래와 같음

3 - 1. 영속성 컨텍스트
* 객체를 메모리에 관리하고, 데이터베이스와 동기화
* 영속화 된 데이터는 1차 캐시에 있고 같은 트랜젝션 내 동일한 엔티티를 재사용하여 불필요한 접근을 줄임

## JPA 사용 이유
* 엔티티로 구성을 하게 되면, jpa가 알아서 외래키를 참조해서 데이터를 가져옴
* 상속된 객체와 테이블을 매핑해줌
[JSR 220](https://download.oracle.com/otn-pub/jcp/ejb-3_0-fr-eval-oth-JSpec/ejb-3_0-fr-spec-persistence.pdf?AuthParam=1732682157_55aba3b4a7687a5ecc835450c1c8e175)
## JPA 영속성 컨텍스트
EntityManager가 관리하는 메모리 저장소. 데이터베이스에 저장된 데이이터와, 등록할 데이터를 관리함.

엔티티의 상태는 아래와 같이 있음
1. 비영속 : 영속성 컨텍스트와 연결되지 않은 상태
2. 영속 : 영속성 컨텍스트에서 관리되는 상태
3. 준영속 : entityManageer가 관리하지 않은 엔티티. 변경되도 반영되진 않음
4. 삭제: 삭제된 상태

## JPA N+1 문제
- 연관관계가 맺어져있는 엔티티를 불러올 때 연관된 엔티티까지 함께 불러오는 이슈  

## JPA N+1 문제의 해결
* 지연로딩, fetch join

## JPA Transaction 전파
 
## OSIV
* 영속성 컨텍스트 생존 범위가 controller부터 view까지 유지하는 것
* 요청의 시작과 끝까지 영속성 컨텍스트가 유지되기 때문에 connection 범위가 넓어짐. 많은 요청이 들어오면 커넥션을 맺을 수 없게되는 현상이 나타남
* 맨 처음 datasource가 default datasource이므로 read, write db 분리를 위해서면 Transaction전파 레벨을 변경해야하는 불편함이 있었음