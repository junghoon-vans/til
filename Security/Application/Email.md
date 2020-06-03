이메일
===

Contents
---

- [구조](#구조)
- [프로토콜](#프로토콜)
  - [SMTP(Simple Mail Transfer Protocol)](#smtpsimple-mail-transfer-protocol)
  - [POP3(Post Office Protocol ver3)](#pop3post-office-protocol-ver3)
  - [IMAP4(Internet Mail Access Protocol ver4)](#imap4internet-mail-access-protocol-ver4)
- [보안 기술](#보안-기술)
  - [PEM(Privacy Enhanced Mail)](#pemprivacy-enhanced-mail)
  - [PGP(Pretty Good Privacy)](#pgppretty-good-privacy)
  - [S/MIME(Secure Multipurpose Internet Mail Extensions)](#smimesecure-multipurpose-internet-mail-extensions)
    - [동작](#동작)
- [스팸메일 보안 대책](#스팸메일-보안-대책)
  - [메일서버 등록제 (SPF, Sender Policy Framework)](#메일서버-등록제-spf-sender-policy-framework)
  - [스팸 필터 솔루션](#스팸-필터-솔루션)

구조
---

- 사용자 대행자
  - MUA(Mail User Agent)
    - 메시지 작성, 읽기, 답장 보내기 등을 수행하는 S/W
- 전송 대행자
  - MTA(Mail Transfer Agent)
    - 전자우편을 `SMTP`을 이용하여 다른 전자우편 서버로 전달하는 S/W
  - MDA(Mail Delivery Agent)
    - 전자우편을 수신자의 사서함으로 전달하는 S/W
- 접근 대행자
  - MAA (Mail Access Agent)
    - 메시지를 검색하고자 할 때 사용
    - `IMAP`, `POP3`

프로토콜
---

![이메일 프로토콜](images/2020-06-03-20-38-14.png)

### SMTP(Simple Mail Transfer Protocol)

- MTA 클라이언트와 서버를 규정하는 공식적인 프로토콜(TCP/25)
- 송신자와 송신자의 메일서버 사이 그리고 두 메일 서버들 사이에서 총 두 번 사용된다.

### POP3(Post Office Protocol ver3)

- 메일을 PC에 다운할수 있도록 해주는 프로토콜(TCP/110)
- 다운시 서버에 있던 메일 내용은 삭제

### IMAP4(Internet Mail Access Protocol ver4)

- 서버서 메일을 읽는 프로토콜(TCP/143)
- 제공 기능
  - 전자우편 내려 받기 전에 헤더 검사
  - 내용 검색, 부분적 다운로드
  - 메일서버에서 편지함을 생성, 삭제, 이름 변경

보안 기술
---

### PEM(Privacy Enhanced Mail)

- IETF서 인터넷 드래프트로 채택
- 기밀성, 인증, 무결성, 부인방지를 지원하는 이메일 보안 기술
- SMTP에 암호화된 정보, 전자서명, 암호화를하여 전송
- 이론 중심, 사양 방대, 구현의 복잡성 때문에 많이 사용되지 않음

### PGP(Pretty Good Privacy)

- 구현이 용이하여 널리 사용
- 다양한 플랫폼 지원
- 인증된 알고리즘 사용
- 기밀성, 메시지 인증, 사용자 인증, 송신 부인방지 지원
- 수신 부인방지는 지원하지 않음

### S/MIME(Secure Multipurpose Internet Mail Extensions)

- PEM의 구현 복잡성, PGP의 낮은 보안성을 보완하기 위해 RSADSI 기술을 기반으로 개발된 기술
- 메시지 기밀성, 무결성, 사용자 인증, 송신 사실 부인 방지 기능 제공
- 보안 메커니즘
  - 전자서명: DSS(디지털 서명 표준), RSA 알고리즘
  - 세션키 암호화: Diffie-Hellman, RSA 알고리즘 

#### 동작

1. 사용자가 메시지를 보내기 이전, 보안 메커니즘(전자 서명, 암호화) 설정
2.  `MIME` 형태로 작성된 메시지를 `S/MIME` 메시지로 변환
3. 송신자는 메일 서버로 전송, 수신자는 S/MIME 클라이언트를 통해 메시지 수신

> MIME: 전자우편을 통해 ASCII가 아닌 데이터가 송신될 수 있도록 허용하는 부가적인 프로토콜

스팸메일 보안 대책
---

<table>
  <thead>
    <tr>
      <th>분류</th>
      <th>대응 방안</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan=5>메일서버 수신차단</td>
      <td>콘텐츠 필터링</td>
      <td>메일의 헤더 정보, 제목, 내용의 특정 단어/문장 패턴을 검색하여 차단</td>
    </tr>
    <tr>
      <td>송신자 필터링</td>
      <td>송신자의 IP, 메일주소, URL 등의 정보를 블랙리스트로 관리하여 차단</td>
    </tr>
    <tr>
      <td>네트워크 레벨 필터링</td>
      <td>패킷 필터링, DNS 변경</td>
    </tr>
    <tr>
      <td>발송량 기준 차단</td>
      <td>기준 시간 동안에 특정 용량 이상의 메일 수신 시 이후 수신 메일 차단</td>
    </tr>
    <tr>
      <td>시간대별 차단</td>
      <td>업무 시간에 업무용 메일 이외의 외부 메일 차단</td>
    </tr>
    <tr>
      <td rowspan=2>메일서버 보안</td>
      <td>릴레이(relay) 스팸 방지</td>
      <td>액세스 DB를 통한 송신자별 허용 거부, 릴레이 허용 여부 설정 가능</td>
    </tr>
    <tr>
      <td>Anti-SPAM 솔루션 도입</td>
      <td>별도의 스팸 차단 전용 보안 솔루션을 도입하여 운영</td>
    </tr>
    <tr>
      <td rowspan=2>메일 클라이언트 보안</td>
      <td>콘텐츠 필터링</td>
      <td>특정 단어가 포함된 필터링 규칙 적용</td>
    </tr>
    <tr>
      <td>송신자 필터링</td>
      <td>수신 거부 등 별도의 블랙리스트 관리를 통해 차단</td>
    </tr>
  </tbody>
</table>

### 메일서버 등록제 (SPF, Sender Policy Framework)

- 메일 헤더에 표시된 IP가 실제로 메일을 발송한 서버 IP와 일치하는지 비교함으로써 발송자 정보의 위변조 여부를 판단
- 발송자의 서버를 DNS에 미리 등록하고 수신자의 서버에 메일이 도착하면 등록된 서버로부터 발신되었는지 확인하여 수신자에게 전달되기 전에 스팸메일을 차단하는 기술 (오픈 소스 기반)
- 필터링 방식에 비해 서버 및 네트워크 자원 소모가 낮고 잘못 탐지할 가능성이 낮음

### 스팸 필터 솔루션

- 메일서버 앞단에 위치하여 프록시 메일서버로서 동작하며, SMTP 프로토콜을 이용한 DoS 공격이나 폭탄 메일, 스팸 메일을 차단
- 주요 기능
  - 메일 헤더 필터링
  - 제목 필터링
  - 본문 필터링
  - 첨부파일 필터링
- 본문 검색 기능으로 내부 정보 유출 차단도 가능