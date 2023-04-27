공중 네트워크를 이용하여 사설 네트워크가 요구하는 서비스를 제공

> 전용 회선처럼 사용할 수 있게 해주는 기술

종류
---

- Intranet VPN
  - 기업 내부를 LAN을 통해 연결
  - 넓게는 지사까지 연결
- Extranet VPN
  - 자사의 협력업체에게 Intranet 제공
  - Intranet의 확장
- Remote Access VPN
  - 원격으로 `NAS(Network Access Server)`에 접속
  - NAS는 사용자의 접속 인증절차와 터널링 관련 기능 수행

구현 기술
---

### 터널링

송수신자 사이의 전송로에 외부로부터의 `침입을 막기 위해` 일종의 파이프를 구성한 것

- 터널링 되는 데이터를 페이로드라고하며 이것의 내용은 변경되지 않음
- 지원하는 프로토콜은 `PPTP, L2TP, L2F, MPLS, IPSEC` 등이다.

### 암호화

- 정보의 기밀성을 제공하기 위해 `대칭키 암호`를 사용
- 대칭키는 `공개키 암호방식`을 사용하여 키 교환
- 메시지 인증: `MAC`, `해시함수`
- 사용자 인증: 보안서버로부터 인증을 받아야 접속 가능

### 접근제어

- 인증된 사용자에게만 접근 허용
- 암호화 되지 않은 패킷만 접근제어 가능

프로토콜
---

### 2계층

#### PPTP

- MS 개발
- `일대일 통신`만 가능
- IP를 통해 전송
- `TCP` 연결
- Client-Server 구조
- `PPP`기초해 연결 확장, 보호

#### L2F

- CISCO 개발
- NAS 개시 VPN형
  - 별도의 S/W 필요 없음
- `다자간 통신` 지원
- `UDP` 연결

#### L2TP

- `PPTP + L2F`
- 보안성 우수
- IP 외 여러 프로토콜 지원
- 보완을 위해 `IPSec`과 결합

### IPSec

`IP 계층(3계층)`의 보안 프로토콜로 네트워크 계층의 보안을 위해 `AH 프로토콜`과 `ESP 프로토콜`을 함께 사용한다.

- 강력한 암호화, 인증방식
- IPv4와 IPv6를 모두 지원
  - IPv6에서 AH와 ESP는 확장 헤더의 한 부분

#### 동작 모드

- 전송 모드(Transfer Mode): 호스트와 호스트간의 메시지(Payload) 무결성을 제공
- 터널 모드(Tunnel Mode): IP 패킷 전체를 보호하는 모드

#### 헤더

- AH (인증 헤더, Authentication Header)
  - 발신지 인증, 데이터 무결성만을 보장
- ESP (캡슐화된 보안 페이로드, Encapsulating Security Payload)
  - 발신지 인증, 데이터 무결성, `기밀성` 모두를 보장

#### 서비스 기능

- 접근제어
  - 보안 정책(SP, Security Policy)과 보안 연관을 통해 송수신 IP 패킷에 대한 시스템 접근을 제어
- 비연결 무결성
  - MAC을 통해 각 IP 패킷 별로 무결성을 보장
- 데이터 송신처 인증
  - MAC을 통해 제공
- 재전송 공격 방지
  - 보안연관(SA, Security Association, 보안연계)에 순서번호를 유지하여 방지
- 기밀성
  - 대칭 암호화를 통해 지원
- 제한된 트래픽 흐름의 기밀성
  - `ESP/터널모드` 적용 시 새로운 IP 헤더를 통해 보안/터널 게이트웨이 구간 정보는 노출
  - 기존 IP 헤더에 있는 `최초 출발지/목적지` 정보는 암호화되어 노출되지 않음

> `기밀성, 제한된 트래픽 흐름의 기밀성`은 ESP 프로토콜만 지원한다.

#### 보안 연관과 보안 정책

- SA(보안 연관): 두 기기의 논리적인 연결 상태 동안 적용할 보안 정보
  - `단방향 설정`이므로 두 기기 연결에 `2개의 SA`가 필요
- SAD(보안 연관 데이터베이스): 각각의 보안 연관과 관련된 매개 변수 값을 저장하는 데이터베이스
- SP(보안 정책): 수신하는 데이터그램을 어떻게 처리할 지에 대한 규칙
- SPD(보안 정책 데이터베이스): 보안 정책이 저장되어 있는 데이터베이스

#### 인터넷 키 교환(Internet Key Exchange, IKE)

- 내부적 및 외부적 SA을 생성하기 위해 설계된 프로토콜(UDP/500)
- SP들을 생성하고 협상, 관리
- ESP, AH에서 사용하게 될 키를 관리

### MPLS

- 다른 VPN 구조에 비해 서비스 도입과 운영관리가 간단
- 저가의 VPN 서비스 제공 가능
- 짧고 고정된 길이(4byte)의 레이블을 이용한 스위칭

### SSL

- 전송 계층(5계층)에서 동작
- 사용자가 쉽게 사용
- 세분화된 접근 통제
- IPSec VPN에 비해 설치 및 관리 편리, 비용 절감