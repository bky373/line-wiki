## Network

- **네트워크** : 컴퓨터를 **두 대 이상** 연결하여 서로 **데이터를 전송**할 수 있는 **통신망**을 말한다.  ( 출처-1 )

- **인터넷** : **TCP/IP 프로토콜**을 사용하는 **세계 최대 규모의 네트워크**이다. **전 세계**의 컴퓨터를 서로 연결하여 정보를 교환할 수 있도록 만든 하나의 거대한 컴퓨터 통신망이다. ( 출처-1 )

- **패킷** ( `packet` ) : 네트워크 통신을 할 때 사용되는 작게 **분할된 데이터 조각**으로 네트워크에서 전송하는 데이터의 기본 단위를 말한다.  ( 출처-1 )

- **랜** ( `Local Area Network`, `LAN`, **근거리 통신망** ) : 비교적 **가까운 거리**에 위치한 장치들을 서로 연결한 네트워크를 말한다. **집, 사무실, 학교** 등의 건물과 같이 가까운 지역을 연결하는 네트워크이다.  ( 출처-1 )

- **왠** ( `Wide Area Network`, `WAN`, **원거리 통신망** ) : 랜을 하나로 묶는 **거대한 네트워크**다. **특정 도시**, **국가**, **대륙**과 같이 매우 넓은 범위를 연결하는 네트워크를 말한다. 넓은 지역에 설치된 컴퓨터들 간의 정보와 자원을 공유하기에 적합하도록 설계한 컴퓨터 통신망이다.  ( 출처-1 )

- **인터넷 서비스 제공자** ( `Internet Service Provider`, `ISP` ) : 인터넷에 접속하는 수단을 제공하는 주체다. 일반 사용자, 기업체, 기관, 단체 등이 **인터넷에 접속하여 인터넷을 이용할 수 있도록 돕는 사업자**다. 현재는 SK 브로드밴드, KT, U+와 같은 ISP가 인터넷 서비스를 제공한다.  ( 출처-1 )

- **서버** ( `server` ) : 컴퓨터 네트워크에서 다른 컴퓨터에 **서비스를 제공**하기 위한 컴퓨터 또는 프로그램이다. 반대로 서버에서 보내주는 정보 서비스를 받는 측 또는 요구하는 측의 컴퓨터 또는 프로그램을 **클라이언트** ( `client` ) 라고 한다.  ( 출처-1 )

- **DMZ** ( `DeMilitarized Zone` ) : 네트워크 구성 중에서 일반적으로 인터넷인 외부 네트워크와 내부 네트워크 사이에 위치한 **중간 지대(서브넷)** 을 말한다. 네트워크의 보안 영역으로 외부 공격자가 내부 네트워크에 침투하는 것을 막는 역할을 한다. **외부에 공개하기 위한 네트워크**라고 할 수 있다. 주로 **웹 서버, 메일 서버, DNS 서버**를 사용자에게 공개한다.  ( 출처-1 )

- **프로토콜** ( `protocol` ) : 컴퓨터 간에 정보를 주고받을 때의 **통신 방법**에 대한 **규칙이나 표준**이다.  ( 출처-1 )

- **헤더** ( `header` ) : 저장되거나 전송되는 데이터의 맨 앞에 위치하는 추가적인 정보 데이터를 말한다. **데이터의 내용과 성격을 식별 또는 제어** 하는 데 사용한다.  ( 출처-1 ) 

- **트레일러** ( `trailer` ) : **데이터의 맨 뒤** 에 위치하는 정보로 **데이터의 무결성을 보장** 하는 데 사용한다.  ( 출처-1 ) 

- **OSI 모델** ( `Open Standards Interconnection model`  ) : 국제표준화기구(`ISO`)가 1977년에 정의한 **국제 통신 표준 규약**이다. 네트워크의 기본 구조를 **일곱 계층**으로 나눠서 표준화한 통신 규약으로 현재 다른 **모든 통신 규약의 기반**이 된다. ( 출처-1 )

  > 7계층은 위에서부터 ( `A|P|S|T|N|D|P` )의 순서로, **응용** 계층, **표현** 계층, **세션** 계층, **전송** 계층, **네트워크** 계층, **데이터 링크** 계층, **물리** 계층이 있다.

  | 계층  | 이름                                                         | 설명                                                         |
  | ----- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | 7계층 | **응용** 계층<br />( `Application Layer`, 애플리케이션 계층 ) | 이메일 & 파일 전송, 웹 사이트 조회 등 애플리케이션에 대한 서비스를 <br> 제공한다. |
  | 6계층 | **표현** 계층<br />( `Presentation Layer`, 프레젠테이션 계층 ) | 문자 코드, 압축, 암호화 등의 데이터를 변환한다.              |
  | 5계층 | **세션** 계층<br />( `Session Layer` )                       | 세션 체결, 통신 방식을 결정한다.                             |
  | 4계층 | **전송** 계층<br />( `Transport Layer`, 트랜스포트 계층 )    | 신뢰할 수 있는 통신을 구현한다.                              |
  | 3계층 | **네트워크** 계층<br />( `Network Layer` )                   | 다른 네트워크와 통신하기 위한 경로 설정 및 논리 주소를 결정한다. |
  | 2계층 | **데이터 링크** 계층<br />( `Data Link Layer` )              | 네트워크 기기 간의 데이터 전송 및 물리 주소를 결정한다.      |
  | 1계층 | **물리** 계층<br />( `Physical Layer` )                      | 시스템 간의 물리적인 연결과 전기 신호를 변환 및 제어한다.    |

- **OSI 모델 계층에 사용되는 프로토콜과 기술**

  | 이름                 | 프로토콜 / 기술                              |
  | -------------------- | -------------------------------------------- |
  | **응용** 계층        | HTTP, DNS, FTP  /  SMTP, POP3, 기타 프로토콜 |
  | **전송** 계층        | TCP  /   UDP                                 |
  | **네트워크** 계층    | IP 등                                        |
  | **데이터 링크** 계층 | 이더넷                                       |
  | **물리** 계층        | 전기 신호 변환                               |

- **TCP/IP 모델** (`Transmission Control Protocol / Internet Protocol model`) : OSI 모델 7계층의 네트워크에서 **데이터를 전송하는 과정**을 **네 개 계층**(`Layer`)으로 단순화시켜 사용하는 모델이다. **인터넷 모델**이라고도 한다. ( 출처-1 )

  > 4계층은 위에서부터 ( `A|T|I|N` )의 순서로, **응용** 계층, **전송** 계층, **인터넷** 계층, **네트워크 접속** 계층이 있다.

- **캡슐화 / 역캡슐화** : **캡슐화**는 컴퓨터 통신에서 **상위 계층의 프로토콜** 정보를 데이터에 추가하여 **하위 계층으로 전송**하는 기술이다. 반대로 **역캡슐화**는 상위 계층의 통신 프로토콜에서 **하위 계층에서 추가한 정보와 데이터를 분리**하는 기술이다. ( 출처-1 )

- **데이터 링크 계층** : 네트워크 장비 간에 **신호를 주고받는 규칙**을 정하는 계층이다. 랜에서 **데이터를 정상적으로** 주고받기 위해 필요한 계층이다. 일반적으로 가장 많이 사용되는 규칙은 **이더넷(`Ethernet`)** 이다.  ( 출처-1 )

- **이더넷** (`Ethernet`) : 랜에서 **데이터를 정상적으로** 주고받기 위한 규칙이다. 여러 컴퓨터가 동시에 데이터를 전송해도 **충돌이 일어나지 않는 구조**로 되어 있다. 이더넷에서 사용하는, **데이터 전송 시점을 늦추는 방법**을 **`CSMA/CD`** 라고 한다. ( 출처-1 )

- **CSMA/CD** : 반송파 감지 다중 접속 및 충돌 탐지( `Carrier Sencse Multiple Access with Collision Detection` )라고 한다. ( 출처-1 )

  - **CS** : 데이터를 보내려고 하는 컴퓨터가 **케이블에 신호가 흐르고 있는지 아닌지**를 확인하는 규칙 
  - **MA** : 케이블에 데이터가 **흐르고 있지 않다면 데이터를 보내도록** 하는 규칙
  - **CD** : **충돌이 발생하고 있는지**를 확인하는 규칙
  - 지금은 효율이 좋지 않다는 이유로 **거의 사용하지 않는다**. 대신 **스위치(switch)** 라는 장비를 사용하여 충돌을 방지한다.

- **반송파** : 통신에서 정보의 전달을 위해 입력 신호를 변조한 전자기파(일반적으로 사인파)를 의미한다. ( 출처-1 )

- **허브의 특징** : 약해지거나 파형이 **뭉그러진 전기 신호를 복원**하고, 해당 전기 신호를 전달받은 포트를 제외한 **나머지 포트에 이를 전달**한다. ( 출처-1 )

- **MAC 주소** (`Media Access Control Address`) : **랜 카드 제조 시 새겨지는 물리 주소**이다. 전 세계에서 유일한 번호이다. MAC 주소는 **48비트 숫자**로 구성된다. 앞쪽 24비트는 랜 카드를 만든 **제조사 번호**고 뒤쪽 24비트는 제조사가 랜 카드에 붙이는 **일련번호**다.  ( 출처-1 )

- **스위치** (`switch`, **스위칭 허브**) : 랜을 구성할 때 사용하는 **단말기 간 스위칭** 기능이 있는 통신망 **중계 장치**다. 컴퓨터(호스트)에서 특정한 **다른 단말기로 패킷을 보낼 수 있는** 기능이 있어 통신 효율이 향상된다.  ( 출처-1 )

  - 데이터 링크 계층에서 동작하며, **레이어 2 스위치** 또는 **스위칭 허브**라고도 부른다.
  - 스위치에는 **MAC 주소 테이블**이 있다.
  - **MAC 주소 테이블**은 스위치의 **포트 번호**와 그 포트에 연결되어 있는 컴퓨터의 **MAC 주소**가 등록되는 데이터베이스다. 
  - 스위치가 수신 포트 이외의 모든 포트에서 데이터를 송신하는 것을 **플러딩**이라고 한다.
  - 스위치에서 MAC 주소를 기준으로 목적지를 선택하는 것을 **MAC 주소 필터링**이라고 한다.

- **IP** ( `Internet Protocol`, **인터넷 프로토콜** ) : 인터넷에 있는 한 컴퓨터에서 다른 컴퓨터로 데이터를 보내는 데 사용되는 **네트워크 계층 프로토콜**이다.  ( 출처-1 )

- **IP 주소** ( `IP Address` ) : 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신하기 위해 사용하는 주소다.   ( 출처-1 )

  - **다른 네트워크로 데이터를 전달**하려면 IP 주소가 필요하다. 
  - IP 주소는 인터넷 서비스 공급자에게 할당받을 수 있다. 
  - IPv4의 IP 주소는 32비트, IPv6의 IP 주소는 128비트로 구성되어 있다.
  - IP 주소의 종류에는 공인 IP 주소와 사설 IP 주소가 있다.
  - IP 주소는 네트워크 ID와 호스트 ID로 나누어진다. 

- **DHCP** (`Dynamic Host Configuration Protocol`) : IP 주소를 자동으로 할당하는 프로토콜이다. 라우터는 DHCP 기능을 가지고 있다.  ( 출처-1 )

- **네트워크 계층** ( `network layer`) : 네트워크 계층은 **서로 다른 네트워크 간 통신** 이 이루어지는 계층이다. 라우터를 통해 최적의 경로 설정 ( 라우팅 )을 하며 패킷 전송을 담당한다.   ( 출처-1 )

- **라우터** ( `router` ) : **서로 다른 네트워크를 연결해 주는 장치**로 현재의 네트워크에서 다른 네트워크로 **패킷을 전송**할 수 있도록 한다.  ( 출처-1 )

- **라우팅** ( `routing` ) : 네트워크에서 패킷을 목적지로 보낼 때 목적지까지 갈 수 있는 여러 가지 경로 중 **한 가지 경로를 설정**하는 과정이다.  ( 출처-1 )

- **라우팅 테이블** ( `routing table` ) : 컴퓨터 네트워크에서 목적지 주소를, 목적지에 도달하기 위한 네트워크 노선으로 변환할 목적으로 사용된다. 다른 네트워크로 가기 위한 **가장 좋은 라우터의 정보**를 가지고 있다.  ( 출처-1 )

- **서브넷** ( `subnet` ) : 큰 네트워크를 분할하여 만든 **작은 네트워크**이다.  ( 출처-1 )

- **서브넷팅** ( `subneting` ) : 네트워크를 분할하기 위해 **IP 주소의 구성을 변경**하는 작업을 말한다.  ( 출처-1 )

- **서브넷 ID** : IP 주소의 네트워크 부분을 늘리기 위해 서브넷 마스크로 사용되는 비트로 서브넷 비트 ( `subnet bits` ) 라고도 한다.  ( 출처-1 )

- **서브넷 마스크** ( `subnet mask` ) : IP 주소의 네트워크 부분만 나타나게 하여 같은 네트워크인지를 판별하게 하는 마스크이다.  ( 출처-1 )

- **멀티캐스트** ( `multicast` ) : 한 컴퓨터(호스트)에서 **패킷을 여러 컴퓨터로** 동시에 전송한느 것을 말한다.  ( 출처-1 )

- **브로드캐스트** ( `broadcast` ) : IP 네트워크에 있는 **모든 컴퓨터(호스트)로 데이터를 전송**하는 방식을 말한다.  ( 출처-1 )

- **전송 계층**  ( `transport layer`, 트랜스포트 계층 ) : **신뢰할 수 있는 데이터를 순차적으로 전달**하는 역할을 하는 계층이다. 이를 통해 상위 계층들이 데이터 전달의 유효성이나 효율성을 신경 쓰지 않도록 한다. 데이터가 중복되거나 누락되지 않고 오류 없이 순서에 맞게 전송되도록 관리한다.  ( 출처-1 )

- **연결형** ( `connection-oriented` ) : 데이터를 교환하기 전에 **연결을 맺고** 데이터를 교환하는 동안 **계속 연결을 관리**하는 프로토콜의 한 형태이다.   ( 출처-1 )

- **비연결형** ( `connectionless` ) : 연결( `connection` )에 대한 초기화 과정이 없는 통신을 말한다.  ( 출처-1 )

- **TCP** ( `Transmission Control Protocol`, 전송 제어 프로토콜 ) : 전송 계층의 프로토콜은 **연결형**( `connection-oriented` ) 통신 방식이며 **신뢰할 수 있는 데이터 전송**을 보장한다.  ( 출처-1 )

- **대역폭** ( `bandwidth` ) : 정해진 시간 동안 전송될 수 있는 데이터의 양 ( 주로 **속도**를 의미한다 ) 을 말한다. 대역폭은 제한적이다.  ( 출처-1 )

- **UDP** ( `User Datagram Protocol` ) : 정보를 서로 주고받을 때 보내는 쪽에서 **일방적으로** 데이터를 전달하는 통신 프로토콜이다. 연결을 맺을 필요가 없고 정보를 보내거나 받는다는 신호도 필요하지 않다.  ( 출처-1 )

- **3-way 핸드셰이크** ( `three-way handshake` ) : TCP 통신에서 사용하는 **신뢰성**을 제공하기 위한 통신 방식이다. 컴퓨터 간에 연결을 맺기 위한 초기화 과정이며 세 단계로 되어 있어서 `three-way` 라고 부른다.  ( 출처-1 )  

- **잘 알려진 포트** ( `well-known prots` ) : 특정 애플리케이션이 사용할 수 있도록 예약되어 있는 포트로, 1~1023번 포트를 말한다.  ( 출처-1 )

- **일련번호** ( `sequence number` ) : TCP에서는 데이터를 보낼 때마다 **각 데이터에 고유한 번호**를 부여해서 전송을 시도한다. 이 번호를 이용하여 **TCP 패킷의 순서를 제어**할 수 있다.  ( 출처-1 )

- **응용 계층** ( `application layer`, 애플리케이션 계층 ) : OSI 모델의 최상위 계층으로, 다양하게 존재하는 응용 환경에서 공통적으로 필요한 기능을 다룬다. 이메일, 파일 전송, 웹 사이트 조회 등 애플리케이션에 대한 서비스를 제공하는 계층이다.  ( 출처-1 )

- **WWW** ( `World Wide Web`, 월드 와이드 웹 )   ( 출처-1 )

  - 통신 회선이 거미줄처럼 서로 연결 되어 있어서 언제 어디서든 필요한 곳에 접근하여 정보를 주고 받을 수 있는 **멀티미디어 인터넷 서버** 이다.
  - WWW 에는  **HTML**, **URL**, **HTTP** 의 세 가지 기술이 사용된다.

- **[HTTP](https://velog.io/@bky373/Web-HTTP%EC%99%80-HTTPS-%EC%B4%88%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC)** ( `HyperText Transfer Protocol` ) : 웹 서비스에서 클라이언트(웹 브라우저)와 웹 서버 간에 정보를 주고 받기 위해 사용하는 네트워크 프로토콜을 말한다.  ( 출처-1 )

  - `Keep-Alive` : 클라이언트와 서버가 연결을 한 번 수립하면 데이터 교환을 마칠 때까지 연결을 유지하고, 데이터 교환을 모두 끝내면 연결을 끊는 구조 를 지원한다. HTTP/1.0 버전에서는 요청을 보낼 때마다 연결이 수립되었다 끊어지는 작업을 반복했다. 이런 느린 작업의 단점을 보완하기 위해  HTTP/1.1 버전에서 `Keep-Alive` 기능이 추가되었다. 
  - 하지만 `Keep-Alive`는 **요청 순서대로 응답** 값을 보내줘야 하는 단점이 있었다. 이전 요청을 처리하는 데 시간이 길어지면 다음 요청에 대한 처리가 늦어졌다. 
  - 이후 등장한 HTTP/2 버전은 이를 보완하였다. 요청을 보낸 순서대로 꼭 응답 값을 반환하지 않아도 되었다. 이에 따라 HTTP/2 버전에서는 콘텐츠 표시를 보다 빠르게 할 수 있게 되었다.    

- **DNS** ( `Domain Name System`,  도메인 이름 시스템 ) : 네트워크에서 **호스트 이름을 IP 주소로 변환** 하는 데 사용하는 시스템(서비스)이다. DNS 서비스가 동작하는 컴퓨터(서버)를 DNS 서버라고 한다.  ( 출처-1 )

- **FTP** ( `File Transfer Protocol`,  파일 전송 프로토콜 ) : 서버와 클라이언트 간에 **파일을 전송** 하기 위한 프로토콜이다. 일반적으로 통신 포트는 데이터 전송 용도로 20번 포트, 제어 용도로 21번을 사용한다.  ( 출처-1 )

- **SMTP** ( `Simple Mail Transfer Protocol`,  단순 메일 전달 프로토콜 ) : **인터넷에서 메일을 송신** 하는 데 사용하는 프로토콜이다. 통신 포트는 일반적으로 25번을 사용한다. SMTP를 지원하는 서버를 SMTP 서버라고 한다.  ( 출처-1 )

- **POP3** ( `Post Office Protocol` ) : **인터넷에서 메일을 수신** 하는 데 사용하는 프로토콜이다. 통신 포트는 일반적으로 110번을 사용한다. POP3를 지원하는 서버를 POP3 서버라고 한다. ( 출처-1 )

- **HTML** ( `HyperText Markup Language` )   ( 출처-1 ) 

  - 웹 페이지에서 **문장 구조나 문자를 꾸미는 태그** 를 사용하여 작성하는 **마크업 언어** 이다. 예를 들어, 제목이나 목록과 같은 문장 구조를 지정하거나 이미지 파일을 보여 줄 때 태그를 사용한다.
  - WWW 를 통해 볼 수 있는 문서를 만들기 위해 사용하는 프로그래밍 언어이다. 하이퍼텍스트를 작성하기 위해 개발되었다. 
  - 하이퍼텍스트로는 문자와 이미지를 표시하거나 **하이퍼링크** ( `hyperlink` ) 를 사용할 수 있다. 하이퍼링크는 보통 **링크** ( `link` ) 라고 부르는데 웹 사이트에서 아이콘이나 버튼 등에 있는 링크를 클릭하면 다른 사이트로 이동할 수 있다. 이동한 사이트에서는 **html 파일** 이나 이미지 파일이 웹 서버에서 전송된다.

- **URL** ( `Uniform Resource Locator` ) : 인터넷에서 **파일의 위치** 를 지정하기 위해 기술된 주소이다. 웹 사이트 주소를 지정하기 위해 사용한다.   ( 출처-1 )

  - URL의 문법은 다음과 같다.
    - **scheme://{userinfo@}host[:port][/path][?query][#fragment]**
    - 예) **https://www.google.com:443/search?q=hello&hl=ko**
    - 위 예시의 구성 요소는 다음과 같다.
      - **프로토콜**(https) 
      - **호스트명**(www.google.com) 
      - **포트 번호**(443)
      - **패스**(/search)
      - **쿼리 파라미터**(q=hello&hl=ko)


- **ping  명령**   ( 출처-1 )

  - 목적지 컴퓨터와의 통신을 확인하기 위해 사용한다.

  - ICMP ( `Internet Control Message Protocol` ) 라는 프로토콜을 사용하여 목적지 컴퓨터에 ICMP 패킷을 전송하고 **패킷에 대한 응답이 제대로 오는지 확인하는 명령** 이다.

  - `ping` 명령이 정상으로 실행되면 네트워크 연결이 정상이라고 판단할 수 있으므로 문제를 확인할 때 자주 사용한다.

  - `ping` 사용법

    > (윈도우의 경우)  cmd 창을 열고, 아래 명령어들을 실행한다.
    >
    > ```bash
    > ping [목적지 IP 주소]
    > ping [목적지 호스트 이름]
    > ```
    >
    > <br>
    >
    > (실행 결과)
    >
    > ![image](https://user-images.githubusercontent.com/49539592/127004841-5a6d6fc6-7362-487e-884b-77fb8873ca4c.png)

- **액세스 포인트** ( `access point` ) : 무선 인터넷 사용자가 인터넷 서비스를 이용할 수 있도록 무선 인터넷 접속을 도와주는 중계 장치다.  ( 출처-1 )

- **IEEE802.11n** : 무선 랜이나 와이파이( `Wi-Fi` ) 라고 부르는 랜을 위한, 컴퓨터 **무선 네트워크에서 사용되는 표준 기술** 을 말한다. ( 출처-1 )

- **SSID** ( `Service Set IDentifier`, 서비스 세트 식별자 ) : 무선 랜을 통해 전송되는 **패킷의 각 헤더에 붙는 고유 식별자**로 **하나의 무선 랜을 다른 무선 랜과 구분** 하는 역할을 한다.  ( 출처-1 )

- **채널** ( `channel` ) : 무선 통신에서 하나의 통화 신호나 기타 **정보가 전송되는 분리된 전송로** 를 뜻한다. **통신 채널** 이라고도 한다.  ( 출처-1 )

- **전파 간섭** ( `interference` ) : 같은 주파수의 파동이 **합성**되거나 **상쇄**될 때 나타나는 현상이다. 무선 랜과 무선 랜 또는 무선 랜과 다른 기기 사이의 전파 간섭 때문에 통신 속도가 떨어질 수 있다.

- **Content-Type: multipart/form-data**
  - **파일 업로드와 같이 바이너리 데이터를 전송할 때 사용한다.** 쉽게 HTML Form으로 데이터를 전송하는 경우를 생각하면 된다. 
  - 아래와 같이 이미지 파일을 폼에 업로드 하면,

    ![image](https://user-images.githubusercontent.com/49539592/135484955-c723761c-77a4-4aea-9156-3b86f9222c59.png)
    
  - 요청 헤더 안에 다음 줄이 포함된다.
    > Content-Type: multipart/form-data; boundary=----WebKitFormBoundary9MlEpz4jOhCVCA1i
    
    ![image](https://user-images.githubusercontent.com/49539592/135482627-df4d51dc-2aec-42ab-8720-0050956734f5.png)
  - boundary는 이름 그대로 경계선을 의미한다. 다음 사진에서처럼 폼의 데이터를 구분하는 경계선 으로 사용된다.
  - 만약 폼 안에 필드가 더 있었다면 경계선 표시(`------WebKitFormBoundary9MlEpz4jOhCVCA1i`)의 위(또는 아래)에 다른 필드 정보가 함께 표현되었을 것이다.
    
    ![image](https://user-images.githubusercontent.com/49539592/135482751-73edf571-093f-4ddd-9751-71580e7d2514.png)
    
  - 결국, `multipart/form-data` 라는 이름은 **폼에 있는 여러 타입의 데이터**를 한꺼번에 전송할 수 있기 때문에 붙여진 이름이다. 예를 들어 하나의 폼을 사용하여 title(문자열 필드)과 image(이미지 파일)을 한번에 전송하는 경우를 생각해볼 수 있다.


## References

- [출처-1 : 모두의 네트워크 ( 미즈구치 카츠야 저, 이승룡 옮김 )](http://www.yes24.com/Product/Goods/61794014)
