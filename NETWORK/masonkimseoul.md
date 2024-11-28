# [네트워크]

## 브라우저에 URL을 입력했을 때 무슨 일이 일어날까?

1. URL에 입력된 값을 파싱하여 HTTP Request 메세지를 만든다. 이때 웹 서버로 메세지를 전송하기 위해 OS가 제공하는 프로토콜 스택을 활용한다.
2. 수신자의 IP 정보가 필요하므로, 캐싱된 IP가 있는지 확인한 후 없다면 DNS 서버에서 도메인에 해당하는 IP 정보를 조회한다.
3. 얻은 IP 정보로 웹 서버와 TCP 연결을 설정한다.
4. 연결에 성공하면 브라우저는 HTTP, HTTPS 요청을 생성하여 서버로 전송한다.
5. 웹 서버는 요청을 처리하고 상태코드와 리소스(HTML 페이지, 이미지 등)를 포함한 응답을 브라우저로 보낸다.
6. 이후 브라우저가 받은 리소스로 페이지를 렌더링한다.

## Ethernet

LAN을 통해 데이터를 패킷 단위로 나누어 전송하는 프로토콜

CSMA/CD 프로토콜로 데이터를 송수신한다.

## CSMA

MAC(Media Access Control)프로토콜 중 하나로, 여러 개의 호스트가 하나의 매체(링크)를 공유할 때 충돌이 발생하지 않도록 한다.

채널 사용 전 다른 호스트가 그 채널을 사용하는지 체크하고 접속한다. 패킷 송출 전 반송파 검출을 통해 채널이 비어있는지 확인한다.

충돌을 완전히 방지할 수 없지만 그 가능성은 줄일 수 있다.

왜냐하면 전파 지연 때문에 현재 비어있다고 판단하는 시점과 전송 시점이 달라져서 충돌이 일어날 수도 있기 때문이다.

### CD(Detect)

IEEE 802.3(LAN 이더넷)에서 사용한다. 유선 네트워크에서는 충돌을 확인할 수 있기에 사용한다.

충돌이 없으면 자신이 보낸 신호만 받게되고, 충돌이 있으면 다른 신호와 함께 받게 된다.

동작 순서는 다음과 같다.

1. 채널이 Idle할 때까지 감지하다가, Idle하면 전송 후 대기
    1. 충돌이 발생하면, 다른 노드가 충돌을 감지하지 못할 것을 대비해 짧은 충돌 신호를 전송한다.
2. 만약 진짜로 충돌이 발생했다면, Exponential Backoff 과정을 거친다.
    1. 재충돌을 방지하기 위해 랜덤한 시간동안 대기하다가 전송한다. 바로 보내면 충돌 일으킨 노드와 다시 충돌할 확률이 높기 때문
3. 전송 횟수 K = K + 1
    1. K가 최댓값(일반적으로 15)보다 크다면 전송을 중지하고, 아니라면 0 ~ (2^k) - 1 사이의 R을 뽑는다.
4. Wait Time만큼 기다린 후 다시 1번 과정을 시작한다.
    1. Wait Time = R * Maximum propagation time 또는 R * Average Transmission time for a frame

### CA(Avoidance)

IEEE 802.11(무선 LAN)에서 사용한다. 충돌을 감지하기 힘들기 때문이다.

노드가 충돌을 감지하기 위해선 신호 전송과 동시에 신호를 수신한다. 만약 충돌이 발생하지 않으면 노드는 자신이 보낸 신호만을 수신하지만, 충돌이 발생하면 노드는 자신이 송신한 신호와 다른 노드가 송신한 신호인 두 개의 신호를 수신한다.

## IPv4에서 주소 부족 문제의 이유와 해결법

32비트 주소체계를 사용하고 있으므로, 가능한 주소의 최대 개수는 약 43억 개 정도이다. 인터넷의 성장으로 IP 주소의 수요가 증가했고, IoT 기기들도 네트워크가 필요하게 되어 주소 부족 문제가 발생했다.

다음 해결 방법으로 주소 부족 문제를 해결할 수 있다.

- CIDR(Classless Inter-Domain Routing)
    - a.b.c.d/n 형태로 표기하는 방식으로 조금 더 효율적으로 주소를 사용할 수 있도록 한다.
    - n은 서브넷 마스크에서 32비트 중 MSB(Most Significant Bit)부터 이어지는  1의 개수를 의미한다.
- NAT(Network Address Transmition)
    - 사설 IP 주소와 공인 IP 주소 간 변환을 통해, 사설 네트워크는 하나의 공인 IP를 사용하도록 한다.
- DHCP(Dynamic Host Configuration Protocol)
    - IP 주소를 동적으로 할당하고 관리하여, 사용하지 않는 주소를 다른 호스트에 할당할 수 있도록 한다.
- IPv6 도입
    - 128비트 주소체계인 IPv6는 약 2^128개 주소를 할당할 수 있다.
    - 그러나 IPv4와 호환되지 않아서, 두 프로토콜을 모두 지원하는 Dual-Stack Router를 활용하여 IPv4의 Payload 안에 IPv6 데이터그램을 담아 이동하는 터널링 기법을 사용할 수 있다.