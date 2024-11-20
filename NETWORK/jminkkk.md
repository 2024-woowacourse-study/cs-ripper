### 브라우저에 URL을 입력했을 때 무슨 일이 일어날까?

1. 브라우저가 로컬 네임 서버에서 일치하는 도메인 네임 기록을 확인
2. 없는 경우 dns 시스템에 요청을 보내서 일치하는 ip를 찾아서 반환
3. 브라우저가 해당 서버와 **TCP 연결**을 시작한다.
4. 브라우저가 웹서버에 **HTTP 요청**을 보낸다.
5. 서버가 요청을 처리하고 **응답**을 보낸다.

### Ethernet

LAN 내의 호스트들이 올바르게 정보를 주고 받을 수 있게 하는 기술

통신 매체를 통해 신호를 송수신 하는 방법, 데이터 링크 계층에서 주고받는 데이터(프레임) 형식 등이 정의

### CSMA

매체 접근 제어(MAC) 프로토콜의 하나이다. 여러 개의 호스트가 하나의 매체/링크를 공유하게 되면 전기신호가 충돌하여 통신을 할 수 없게 된다.

CSMA는 이 충돌을 방지하기 위한 프로토콜이다.

### IPv4에서 주소 부족 문제의 이유와 해결법

IPv4면 나타낼 수 있는 주소가 2^32개이다. 웹이 점점 발전함에 따라 필요한 주소 수가 늘기 때문에 부족해지는 현상이 발생

1. NAT(Network Address Translation)
    - private IP를 public IP로 변환하여 여러 기기가 하나의 public IP 공유
2. IPv6
    - 6바이트를 사용하기 때문에 사용할 수 있는 주소가 무한대이다.

### 라우팅 프로토콜에서 Distance Vector vs Link State

### TCP/IP

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/1430a828-8bfa-4acb-bfb9-0585b0e739c2/25a7ad30-5f33-4ecf-8712-834a02c08aa0/image.png)

- Application
    - 특정 서비스를 제공하기 위해 응용 프로그램끼리 정보를 주고 받는 계층
    - HTTP, SSH
- Transport
    - 송신된 데이터를 수신 측 어플리케이션에 확실히 전달하기 위한 계층
    - TCP
- Internet
    - 수신 측까지 데이터를 전달하기 위해 사용
    - IP
- Network
    - 물리적인 기기 간 전송을 담당하는 계층
    - Ethernet

### TCP(**Transmission Control Protocol)**

데이터 전송 시 신뢰성을 보장하기 한 프로토콜로 3 way handshake라는 방식을 사용한다.

TCP의 주요 특징

- TCP 프로토콜은 3 way handshaking을 통해 연결을 수립하고 4 way handshaking을 통해 연결을 종료한다.
- 혼잡제어, 오류제어, 흐름제어를 제공하여 신뢰성을 보장한다.

**syn → syn-ack → ack**

### TCP

1. 연결 수립
2. 데이터 전송
3. 연결 종료

### TCP handshaking

3가지의 세그먼트

ACK → 연결요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄 

SYN → 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송

FIN → 데이터 전송이 끝났음을 알림

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/1430a828-8bfa-4acb-bfb9-0585b0e739c2/d7f3b6c0-7b98-422a-9e70-b7cf484c6623/image.png)

### TCP Error Control

송수신 과정에서 잘못 전송된 세그먼트가 있을 경우, 패킷 전송을 재시도를 보장한다.

1. 중복된 ACK 세그먼트가 도착한 경우
2. 타임아웃이 발생한 경우

### TCP Congestion Control

많은 트래픽으로 인해 패킷의 처리 속도가 느려지거나 유실될 수 있는 경우

송신 호스트가 네트워크의 혼잡을 판단하여 혼잡의 정도에 맞는 데이터만큼만 송신

### TCP Flow Control

수신 호스트가 한번에 받아 처리할 수 있을 만큼만 전송하는 것

즉, 송신 호스트가 수신 호스트의 처리 속도를 고려하며 송수신 속도를 맞추는 기능

### 그럼 UDP는 왜 쓰나?

TCP에 비해 송수신자, 등만 패킷의 요소로 가지고 있기 때문에 간단하고 빠른 송수신이 필요한 경우에 사용할 것 같다.

### UDP로는 신뢰성 있는 통신을 할 수 없나?

TCP에서 하는 작업(흐름, 혼잡, 예외 제어)들을 프로그래머가 구현하면 신뢰성을 보장할 수 있다.

### 종단 간 신뢰성 보장 어떻게 할 수 있나?

종단 간 신뢰성은 데이터의 손상과 손실이 없는 것을 말한다.

### HTTP란 무엇인가? (HyperText Transfer Protocol)

application layer에서 응용 프로그램 간의 데이터를 전송하기 위한 프로토콜

웹 브라우저와 서버간의 데이터 전송을 위해 사용되며 요청과 응답으로 이루어져 있다.

주요 특징

1. 요청과 응답
2. 미디어 독립적
    1. 주고 받을 자원의 특성과는 무관하게 자원을 주고받을 수단의 역할
3. 상태를 가지고 있지 않으며
4. 지속 연결을 지원

### HTTP 버전별 차이점

http 1.1

- persistent connection 연결을 재사용할 수 있어 시간이 절약됩니다. 단일 원본 문서 내로 포함된 리소스들을 표시하기 위해 더 이상 여러 번 연결을 열 필요가 없기 때문입니다.
- Pipelining 첫번째 요청에 대한 응답이 완전히 전송되기 전에 두번째 요청 전송을 가능케 하여, 통신 지연 시간이 단축되었습니다.
- 청크된 응답도 지원되었습니다.

http2.0

- 요청, 응답 메시지는 프레임 단위로 나누어지고 **바이너리 형식**으로 인코딩으로 변경
- 파싱, 전송 속도가 빨라짐
- Multiplexed Stream 하나의 커넥션으로 여러 요청이 올 경우 동시에 여러 요청을 처리할 수 있다.
- Header Compression 요청 헤더를 압축할 수 있다.

http3.0

- UDP 기반 빠른 프로토콜

### HOL Blocking이 무엇인가?

### HTTPS는 무엇인가?

HTTP 프로토콜 기반에 SSL과 TLS 프로토콜이 추가된 계층인데요.

HTTP는 데이터를 암호화하지 않기 때문에 제 3자가 데이터를 가로채고 읽을 수 있습니다.

### TLS

TLS는 3가지 방식으로 데이터를 보호합니다.

1. 인증: CA 인증서로 상대 서버의 정보, 공개키, 지문 등의 정보를 확인할 수 있습니다.
2. 암호화: 대칭 및 비대칭 암호화 조합을 사용해서 데이터를 보호합니다.
3. 무결성: 데이터가 손실, 변조되는 것을 방지합니다.

### TLS handshaking / CA인증

암호화된 통신 세션을 시작하는 단계

1. 클라이언트 헬로
2. 서버 헬로: 서버의 디지털 인증서 등 포함
3. 키 교환: 서버의 인증서를 검토하여 도메인의 소유자인지 확인하고 맞다면 동일한 세션 키를 생성
4. 클라이언트 완료 및 서버 완료: 세션 키가 생성되면 완료 메시지 전송
5. 데이터 교환: 세션 키로 모든 데이터가 암호화되어 안전하게 통신

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/1430a828-8bfa-4acb-bfb9-0585b0e739c2/0703eee6-0c00-40b9-9fe0-c611ec445bbb/image.png)

### 웹 캐시 (프록시 서버)

빈번히 요청되는 데이터를 사용자와 지리적으로 가까운 웹 캐시 서버에 보관하여 빠르게 응답하는 것

클라이언트와 웹 서버 간의 프록시 서버

### 스위치 vs 라우터

라우터는 네트워크계층에서 IP주소를 기반으로 동작하며 스위치는 MAC주소를 기반으로 데이터링크 계층에서 동작한다.

스위치

- 목적지로 출발한 데이터를 중간에 **적합한 경로로 스위칭**해주는 역할을 하는것이 스위치이고 스위치는 데이터링크 계층에 속한다. **스위치는 데이터링크 계층에 속해 있으므로 MAC 주소** 기반으로 동작한다.

라우터

- 라우터 또한 목적지로 가는 적합한 경로를 찾아주는 라우팅 기능을 한다. **라우터는 IP주소를 기반으로 작동하여 네트워크 계층에** 속해있다.

### OSI 7계층은 왜 필요한가?

계층별로 책임을 분리하기 위해 사용

각 계층은 데이터 전송에 대해 책임을 가지고 있음

네트워크에 필요한 Protocol 기능들을 7계층으로 나누어 복잡성을 줄이고 계층간의 간섭을 최소화하는 모델

- 응용 계층 : 최상위 계층으로 사용자가 네트워크에 접속하는 것을 가능하게 함
- 표현 계층 : 송/수신자가 공통으로 이해할 수 있도록 정보의 데이터 표현방식을 바꾸는 기능 담당
- 세션 계층 : 네트워크 대화 제어기로 통신 시스템 간에 상호대화를 설정하고 동기화
- 전송 계층 : Port를 이용하여 응용 프로그램간 송수신 담당
- 네트워크 계층 : IP를 주소로 사용하여 호스트간 전송 담당
- 데이터링크 계층 : 송수신되는 정보의 오류와 흐름 관리 담당
- 물리 계층 : 데이터를 물리 매체상으로 전송하는 역할 담당

### DASH, CDN

DASH는 컨텐츠 파일을 서로 다른 rate의 덩어리로 만들어 제공하는 기술이다.

대표적으로 유튜브에서 하나의 영상에 다양한 해상도를 제공하며, 인터넷 상황에 맞춰서 적절한 해상도의 영상을 제공하는 기술이 있다.

CDN(콘텐츠 전송 네트워크)이란 **웹 콘텐츠를 세계 곳곳에 있는 여러 서버에 분산하여 저장하는 분산 서버 네트워크 시스템**

### 비대칭 키와 대칭키를 사용하여 기밀성, 무결성, 인증을 만족하는 통신 과정

대칭키는 암호화 복호화에 사용하는 키가 동일한 방식, 비대칭키는 암복호화