## 메모리 주소 바인딩

- 프로세스가 실행을 위해 메모리에 적재되면 프로세스를 위한 독자적인 주소 공간이 생긴다. 이를 논리적 주소라고 하며, 프로세스마다 독립적으로 할당된다.
- 그런데 논리적 주소만으로는 실제 물리적 메모리의 주소를 알 수 없기에, 이를 연결시키는 작업을 메모리 주소 바인딩이라고 한다.
- 종류
    - 컴파일 타임 바인딩 : 컴파일 시 물리적 메모리 주소가 결정됨
    - 로드 타임 바인딩 : 실행 시작 시 물리적 메모리 주소가 결정됨
    - 런타임 바인딩 : 프로그램 실행한 후에도 물리적 메모리 주소가 변경 가능함

### 런타임 바인딩

- 주소 매핑 테이블을 통해 해당 데이터가 물리적 메모리의 어느 위치에 있는지 점검
- 기준 레지스터, 한계 레지스터, MMU(Memory Management Unit)이 필요
    - 기준 레지스터 : 프로세스의 물리적 메모리 시작 주소를 가지고 있음
    - 한계 레지스터 : 현재 CPU에서 수행중인 프로세스의 논리적 주소의 최대값, 프로세스의 크기를 가지고 있음
    - MMU : 논리적 주소를 물리적 주소로 매핑해주는 하드웨어
- MMU는 요청된 논리적 주소에 레지스터에 저장된 값을 활용하여 실제 주소를 알려준다.
- 컨텍스트 스위칭이 일어날 때마다 프로세스에 해당하는 값으로 레지스터의 값을 교체해야 한다.

## Swapping

- 메모리에 올라와 있는 프로세스를 보조 기억장치로 옮기는 행위(Swap-out)
- 보조 기억장치로 옮겨진 프로세스를 다시 메모리로 올리는 행위(Swap-in)
- 이 둘을 합쳐서 Swapping이라고 한다.

## 메모리 단편화

- 메모리 공간이 작은 조각으로 나뉘어져 있어, 사용 가능한 메모리의 공간은 충분하지만 할당이 불가능한 상태를 말함
- 내부 단편화 : 프로세스가 필요한 양보다 더 큰 양의 메모리가 할당되어 낭비되는 공간이 생김
- 외부 단편화 : 할당, 해제 작업이 반복되어 총 사용 가능 메모리 공간은 충분하지만 실제로 할당이 불가능하게 남아있는 상황. e.g. 0.5kb씩 여러 군데 남아있어 애매한 상황
- 해결 방법
    - 압축 : 메모리 공간을 재배치해 단편화되어 분산된 메모리 공간을 하나로 합침
    - 통합 : 인접한 여유 메모리 공간을 통합해 하나의 큰 메모리 공간으로 합치는 방법
    - 페이징 : 가상 메모리를 같은 크기의 블록으로 나눈다. 실제 메모리 공간을 프레임으로 나누고, 페이지와 프레임을 대응시켜 내부 단편화를 최소화한다. 외부 단편화는 해결할 수 있다.

## 페이징

물리 메모리를 일정 크기인 프레임으로 나누고, 논리 메모리를 프레임과 동일한 크기의 페이지로 나눈다.

이후 필요한 페이지를 프레임에 적재하고 실행한다.

페이지의 일부만 메모리에 들어오게 된다.

## 세그멘테이션

페이징과 다르게 프로세스를 논리적 내용을 기반으로 나눠 메모리에 배치한다.(페이징은 물리적 메모리를 일정 크기로 나눈 프레임과 동일한 크기로 분할)

다만 세그먼트 크기가 일정하지 않고 다양하기 때문에, 외부 단편화가 발생할 수 있다.

## 세그멘테이션 - 페이징 혼합

프로세스을 큰 단위의 세그먼트로 나눔(PCB, Code, Data, Stack 등 각각 따로)

이후 각 세그먼트를 페이지로 나눈다.

Virtual Address = Segment number + Page number + offset

Segment Table Entry = Control Bits + Length + Segment Base

Page Table Entry = PM Control Bits + Frame number(페이징)

![image](https://github.com/user-attachments/assets/d7c3846b-837a-4d4e-97f6-ab9e3e4276c6)

## 가상 메모리

프로세스의 일부분만 메모리에 로드하고 나머지는 보조 기억장치에 두는 것.

MMU를 사용하여 논리주소, 물리주소로 나누어 프로세스의 메모리를 관리한다.

실제 메모리보다 더 큰 공간을 사용할 수 있으며, 가상 주소이기 때문에 논리적 연속성을 제공할 수 있다. 또한 물리 메모리의 주소 공간을 몰라도 된다.

## Page Fault

프로세스가 참조하려는 가상 메모리 페이지가 현재 물리적 메모리에 로드되어 있지 않을 때 발생하는 인터럽트

- 마이너 페이지 폴트 : 메모리 내에 페이지가 존재하지만 페이지 테이블에 매핑되지 않았을 때 발생
- 메이저 페이지 폴트 : 물리적 메모리에 페이지가 존재하지 않고, 디스크에서 로드해야 할 때 발생
- 과정
    - 프로세스 Block → OS가 디스크에 IO request → Block된 동안 다른 프로세스 실행 시작 → IO 작업 후 메모리로 원하는 페이지가 올라오고 다시 시작

## 페이지 교체

1. OPT - Optimal
    1. 앞으로 가장 오랫동안 사용되지 않을 페이지 교체
    2. 프로세스가 앞으로 사용할 페이지를 미리 알고 있어야 하기 때문에 불가능한 교체 방식
2. FIFO - First in First out
    1. 가장 먼저 들어온 페이지를 교체
    2. 프레임 수가 많아질수록 페이지 결함 횟수가 감소할 것 같은데, 그렇지 않을 수도 있다.
3. LRU - Least Recently Used
    1. 가장 오랫동안 사용하지 않은 페이지를 교체
    2. 각 페이지별로 존재하는 논리적인 시계인 카운터가 필요 - 페이지가 사용될 때마다 0으로 클리어되고, 시간이 지날수록 증가
    3. 오랫동안 사용하지 않은만큼 앞으로 사용할 확률이 적다는 것이 전제
4. LFU - Least Frequently Used
    1. 페이지 참조 횟수로 교체할 페이지 결정
    2. LRU는 직전 참조된 시점만을 반영하지만, LFU는 참조횟수를 통해 장기적 시간규모에서 참조성향을 고려할 수 있다.
    3. 단점은 가장 최근에 불러온 페이지가 교체될 수 있다는 점, 구현이 복잡하고 오버헤드가 크다.
5. MFU - Most Frequently Used
    1. 참조 횟수가 가장 많은 페이지 교체
    2. 가장 많이 사용된 페이지가 앞으로는 사용되지 않을 것이란 것이 전제
6. NRU - Not Recently Used
    1. LRU와 비슷함, 최근에 사용하지 않은 페이지를 교체한다.
    2. 그러나 교체되는 페이지의 참조 시점이 가장 오래되었다는 것을 보장하지 않는다.
    3. 동일 그룹 내에서 무작위 선택
    4. 적은 오버헤드로 적절한 성능
    5. 각 페이지마다 참조비트와 변형비트를 사용
        1. 참조 비트 - 페이지가 참조되지 않았을 때 0, 호출되었을 때 1, 모든 참조비트를 주기적으로 0으로 변경한다.
        2. 변형 비트 - 페이지 내용이 변경되지 않았을 때 0, 변경되었을 때 1
        3. 우선순위는 참조 비트가 변형 비트보다 높다.

## Thrasing

실제로 실행하는 것보다 하드 디스크와 메모리 사이 페이지 이동에 더 많은 시간이 걸리는 현상을 말한다.

A 페이지 실행 중 B 페이지가 필요하여 A를 버린다. 이후 B 실행 중 A가 필요하여 B를 버린다.

이 과정이 계속 반복되는 것을 말함

## 파일 시스템

### 접근 방식

- Sequential Access(순차 접근) - offset을 사용하여 순서대로 탐색
- Direct Access(직접 접근) - 임의로 특정 데이터에 접근한다.
- Index Access(색인 접근) - 색인을 먼저 찾고 대응되는 포인터를 얻어 데이터에 접근한다.

### 디렉터리 구조

- Single Level Directory - 모든 파일들이 디렉터리 밑에 존재, 모든 파일 이름은 유일해야 한다. 한 사용자만 사용해야 한다.
- Two Level Directory - 각 사용자별로 별도 디렉터리를 갖는 형태, UFD, MFD
    - UFD - 자신만의 사용자 파일 디렉터리
    - MFD - 사용자 이름과 계정 번호로 색인되어 있는 디렉터리, 각 엔트리는 사용자 UFD를 가리킨다.
- Tree Structured Directories - 각 사용자들이 자신의 서브 디렉터리를 만들어 파일을 구성한다.
    - 각 사용자가 하나의 디렉터리를 가지며 모든 파일은 고유한 경로(절대/상대)를 갖는다.
    - 비트로 파일을 구분한다.(0 = 일반 파일, 1 = 디렉터리)
- Acyclic Graph Directory(비순환 그래프) - 디렉터리가 서브 디렉터리와 파일을 공유할 수 있음
- General Graph Directory(일반 그래프) - 순환 허용 그래프 구조, 무한 루프가 가능해서 잘 안쓰임

## 디스크 블록 할당 방법

### 연속할당

- 디스크 내에서 선형적으로 연속된 블록을 차지하도록 할당
- 한 파일의 연속 할당은 디스크 주소와 블록 단위 길이로 정의된다.
- 탐색 시간이 최소화되며 순차 접근, 직접 접근이 가능하다. 구현이 단순하다.
- 파일 생성 시 필요한 공간 크기를 명시해야 하며, 할당 공간보다 필요 공간이 커지면 새 영역으로 파일을 옮겨야 한다. 단편화가 발생할 수 있다. 파일 크기 계속 변하면 구현이 복잡해진다.

### 연결할당

- 디스크 내의 파일 위치와 상관없이 접근이 가능한 동적 할당 기법
- 압축이 필요하지 않으며, 파일 공간 확장에 용이하며 단편화가 발생하지 않는다.
- 직접 접근 방식은 지원하지 않는다. 검색에 긴 시간이 필요하다. 연결 리스트로 관리되기 때문에 낭비되는 공간이 발생한다. 하나의 포인터가 없어지거나 값이 오염된다면 모든 데이터를 잃을 수도 있다.
- 이를 보완하기 위해 파일 할당 테이블(FAT)을 사용한다.(이러면 직접 접근 가능)

### 색인할당

- FAT 없이 직접 접근 방식을 사용할 수 있다.
- 각 파일은 디스크 블록 주소를 모아 놓은 배열인 색인 블록을 가진다.
- 색인 블록 항목에서 포인터를 얻어 그 블록을 읽는다.
- 디렉터리는 그 디렉터리에 속한 각 항목의 색인 블록에 대한 포인터를 소유한다.
- 탐색 속도가 빠르고 단편화 없이 직접 접근을 제공한다.
- 중간에 파일을 삽입하려면 색인 블록을 완전히 재구성해야 한다. 공간 낭비 문제가 있으며, 하나의 색인 블록으로 모든 블록을 색인할 수 없을 정도의 큰 파일은 저장할 수 없다.
- 보완하기 위한 방법 중 혼합 색인 방법이 있다.

## 캐시의 지역성

### 시간적 지역성

- 최근 참조된 주소 내용은 빠른 시일 내에 다시 참조되는 특성
- 메모리 상의 같은 주소에 여러 차례 읽기 쓰기를 수행하면 상대적으로 작은 크기의 캐시를 사용해도 효율적일 수 있다.

### 공간적 지역성

- 인접하여 저장되어 있는 데이터들이 연속적으로 접근 될 가능성이 높아지는 특성
- CPU 캐시나 디스크 캐시의 경우 한 메모리 주소에 접근할 때 해당 주소를 포함하여 해당 블록을 전부 캐시로 가져오는데, 오름차순이나 내림차순으로 다른 메모리 주소에 접근하게 된다면 효율적이다.

## 캐싱 라인

- 캐시가 아무리 가까이 있더라도, 사용할 데이터가 어디에 있는지 모르면 모든 데이터를 순회할 수밖에 없다.
- 그래서 캐시에 데이터를 저장할 때 특정 자료구조를 사용해 묶음으로 저장하는데, 이를 캐싱 라인이라 한다.