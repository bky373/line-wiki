# 1. HTTP

## 개념

- **HTTP**는 `HyperText Transfer Protocol`의 약자이다.

- **HyperText**와  **Transfer Protocol** 이 뭔지 각각 알아보자. 

  > 1. **HyperText**
  >
  > - 직역: **초월적인 텍스트**
  > - 의역: **텍스트 간 연결(이동)** 할 때, 순차적 접근 방식이 아닌 **비순차적(초월적) 접근 방식**을 따르는 텍스트를 의미한다. 쉽게 하이퍼링크를 생각하면 된다. 

  > 2. **Transfer Protocol**: **통신 장비 간 데이터 교환 방식에 대해 합의한 내용**이다. 통신을 원하는 두 개체가 **무엇을, 어떻게 통신할 것인가**에 대해 **약속**하고 이를 **규칙**으로 정의해놓은 것이라 보면 된다. 데이터의 형식(아날로그 or 디지털), 부호화(Unicode, ASCII), 신호 순서, 인증, 오류수정 등을 포함한다.
  >
  > - (참고) **Hyper Link**: 페이지에서 다른 페이지 혹은 문서 내부에서 다른 문서로 연결되는 참조를 하이퍼링크라고 한다.

- 결국, **HTTP**는 **HyperText를 전송하기 위한 통신 규약**을 의미한다.

  > **HTML** (HyperText Markup Language)
  >
  > - Hyper Text의 의미는 위와 동일하다.
  > - Markup은 표시하다는 뜻으로, 제목, 단락, 목록 등 태그를 사용해 구조적 의미를 표현한다. 또한 링크, 인용과 그 밖의 항목으로 [구조적 문서](https://ko.wikipedia.org/wiki/구조적_문서)를 만들 수 있는 방법을 제공한다. 
  > - (예, 특정 상품 표기를 [Apple, iPhone 12]로 할 수 있지만, [**Company** : Apple, **Model** : iPhone 12]로 할 수도 있다. Mark Up은 간단히 태그를 달아 놓는 방식이라 할 수 있다. 
  > - 결론적으로 태그를 이용해 정보와 문서를 구조적으로 표현하는 웹 문서 작성 기술을 의미한다.

## 특징

1. HTTP는 **웹에서 이루어지는 모든 데이터 교환의 기초**이다. (HTTP가 웹의 구성 요소이다 )

2. HTTP는 **클라이언트-서버 프로토콜**이다. 

   > - **클라이언트-서버 프로토콜**:  (보통 웹브라우저인) 클라이언트 **요청을 생성하기 위해 연결을 연 다음 응답을 받을때 까지 기다리는 모델**, 각각의 요청은 클라이언트에 의해 초기화된다.
   > - 이 둘은 데이터 스트림이 아닌 **개별적인 메시지 교환**을 통해 통신한다.
   > - 클라이언트에 의해 전송되는 메시지를 **요청(requests)**이라 하고, 요청에 대해 서버에서 응답으로 전송하는 메시지를 **응답(responses)**이라고 한다.

3. HTTP는 **상태가 없고, 세션이 있다**.

   - HTTP는 **상태를 저장하지 않는다.** (Stateless)

   - 예를 들어, `www.sample.com/page1` 요청 후 `www.sample.com/page2` 를 요청하는 경우, **이 둘의 요청은 서로 연관성을 가지지 않고 독립적**이다. 즉, `page1`에서 만들어진 데이터는 `page2`를 요청할 때 유지되지 않는다.

   - 이는 e-커머스 장바구니처럼, 사용자가 일관된 방식으로 페이지와 상호작용하길 원할 때 문제가 된다. 하지만, HTTP는 상태가 없음에도 **HTTP 쿠키**를 사용하면 상태를 저장하는 세션을 사용할 수 있다. 

     > [HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies): 서버가 웹 브라우저에 전송하는 작은 데이터 조각이다. 브라우저는 데이터 조각들을 저장해 놓았다가, 동일한 서버에 재요청 시 저장된 데이터를 함께 전송한다. 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용하며, 이를 통해 사용자의 로그인 상태를 유지할 수 있다.

4. HTTP는 **확장 가능**하다.

   - [HTTP 헤더](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)를 사용하면 HTTP를 확장하기 쉽다. 클라이언트와 서버가 새로운 헤더에 대해 합의한다면, 새로운 기능을 언제든지 추가할 수 있다.

## HTTP의 흐름

- 클라이언트가 서버와 통신하고자 할 때, 최종 서버가 됐든 중간 프록시가 됐든, 다음 단계의 과정을 수행한다.

1. **TCP 연결**을 연다: TCP 연결은 (단일 또는 여러 개의) **요청을 보내거나 응답을 받는 데 사용**된다. 클라이언트는 새 연결을 열거나, 기존 연결을 재사용하거나, 서버로 가는 여러 개의 TCP 연결을 열 수 있다.

   > **TCP**(전송 제어 프로토콜)
   >
   > - **두 개의 호스트를 연결**하고 **데이터를 교환**하게 해주는 중요한 네트워크 프로토콜이다. 
   > - TCP 통신을 할 때 **서버가 통신할 준비가 되어 있는지**, 서로 연결해서 **데이터를 주고 받을 수 있는지** 확인한다. 여기서 준비가 되었다면 통신을 하게 되고, 준비가 안 되었다면 서버가 준비가 안 됐다는 메시지 혹은 에러를 사용자에게 보여줄 수 있다. 
   > - TCP는 데이터와 패킷이 보내진 **순서대로 전달되는 것을 보장**해준다. TCP의 역할은 에러가 없이 패킷이 **신뢰할 수 있게 전달**되었는지 보증해주는 것이다.

   ![](https://images.velog.io/images/bky373/post/71087631-74e0-450d-afee-7cc46610706c/image.png)

   > [이미지 출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)

   ![](https://images.velog.io/images/bky373/post/ee09eac0-a44c-4086-ab40-b7d5c7de4248/image.png)

   > [이미지 출처](https://networkinterview.com/http-vs-tcp-know-the-difference/)

2. **[HTTP 메시지](#2-HTTP-메시지)를 전송**한다**(요청)**: 전송되는 HTTP 메시지 형태는 아래와 같다.

   > GET / HTTP/1.1
   > Host: developer.mozilla.org
   > Accept-Language: fr

3. 요청에 맞게 **서버가 보내준 응답을 읽는다**.

   > HTTP/1.1 200 OK
   > Date: Sat, 09 Oct 2010 14:28:02 GMT
   > Server: Apache
   > Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
   > ETag: "51142bc1-7449-479b075b2891b"
   > Accept-Ranges: bytes
   > Content-Length: 29769
   > Content-Type: text/html
   >
   > 
   >
   > `<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)`

4. 연결을 **닫거나** 다른 요청을 위해 연결을 **재사용**한다.

# 2. HTTP 메시지

## 개념

![http request message](https://t1.daumcdn.net/cfile/tistory/21282E3B554A0A1B2C)

> [이미지 출처](https://webdir.tistory.com/263)

- HTTP 메시지는 **서버와 클라이언트 간에 데이터가 교환되는 방식**을 말한다. **ASCII로 인코딩된 텍스트 정보**이며 여러 줄로 되어 있다.
- **요청(request)**과 **응답(response)** 두 가지 타입이 존재하며, 각각 특정한 포맷을 가지고 있다. 아래에서 각각의 내용을 살펴볼 수 있다.
- 개발자가 HTTP 메시지를 텍스트로 작성하는 경우는 잘 없다. 대신 **소프트웨어, 브라우저, 프록시, 또는 웹 서버가 그 일을 대신**한다. 
- HTTP 메시지는 설정 파일(프록시 혹은 서버의 경우), API(브라우저의 경우), 혹은 다른 인터페이스를 통해 제공된다.
- **단순한 메시지 구조**를 이루고 있고, **확장성이 좋다**는 특징이 있다.

## 공통 구조

- HTTP 요청과 응답의 구조는 서로 유사한 면이 있는데, 구체적으로는 아래와 같다.

1. **시작 줄(start-line, Request-line)**: **HTTP 요청** / 또는 요청에 대한 **성공 또는 실패**가 기록된다. 항상 한 줄로 끝난다.
2. **HTTP 헤더**: 시작 줄 다음으로 **요청에 대한 설명** / 또는 **메시지 본문에 대한 설명**이 들어간다.
3. **빈 줄**: 요청에 대한 모든 메타 정보가 전송되었음을 알리는 빈 줄이 삽입된다. (헤드와 본문 사이)
4. **본문**(optional): **요청과 관련된 데이터**(HTML form 콘텐츠 등) / 또는 **응답과 관련된 문서**(document)가 **선택적**으로 들어간다. 본문의 존재와 크기는 시작 줄 및 HTTP 헤더에 명시된다.

- HTTP 메시지의 시작 줄과 HTTP 헤더를 묶어서 **요청 헤드(head)** 라고 부르며, 이와 반대로 HTTP 메시지의 페이로드는 **본문(body)** 이라고 한다.

![image-20210517003419560](C:\Users\bobo\AppData\Roaming\Typora\typora-user-images\image-20210517003419560.png)

> [이미지 출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)

# 3. Request

- **요청(request)**은 클라이언트가 **서버로 전달**하는 메시지로, **서버 측 액션을 유도**한다.![image-20210517010606565](C:\Users\bobo\AppData\Roaming\Typora\typora-user-images\image-20210517010606565.png)

  > [이미지 출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)

## 시작 줄

- 시작 줄(start-line, Request-line)은 아래 세 가지 요소로 이루어져 있다.

  > **HTTP 메소드** + **URI** + **HTTP 버전**

  1. **[HTTP 메소드](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)**: 영어 동사([`GET`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/GET), [`PUT`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT),[`POST`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/POST)) 혹은 명사([`HEAD`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD), [`OPTIONS`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS))를 사용해 서버가 수행해야 할 동작을 나타낸다.

  2. **URI**: [URL](https://developer.mozilla.org/ko/docs/Glossary/URL), 또는 프로토콜, 포트, 도메인의 절대 경로가 온다. URI 포맷은 아래와 같이 나눌 수 있다.

     - `origin 형식`: 끝에 `'?'`와 쿼리 문자열이 붙는 절대 경로이다. 가장 일반적인 형식으로, `GET`, `POST`, `HEAD`, `OPTIONS` 메소드와 함께 쓰인다.

       > **POST**  /  HTTP 1.1
       > **GET**  / background.png  HTTP/1.0 
       > **HEAD**  /test.html?query=alibaba HTTP/1.1
       > **OPTIONS**  /anypage.html  HTTP/1.0
       >
       > (참고) 
       >
       > - **HEAD**: 웹 서버에서 **헤더 정보** 이외에는 어떤 데이터도 보내지 않는다. 웹 서버의 다운 여부 점검(Health Check)이나 웹 서버 정보(버전 등)등을 얻기 위해 사용될 수 있다. 
       >
       > - **OPTIONS** 메소드: 특정 엔드포인트가 **어떤 메소드를 허용하는지** 알고자 할 때 사용한다

     - `absolute` 형식: 완전한 URL 형식이다. 프록시에 연결하는 경우 대부분 `GET`과 함께 쓰인다.

       > **GET**  http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages  HTTP/1.1

     - `authority` 형식: 도메인 이름 및 옵션 포트(`':'`가 앞에 붙는 형태)로 이루어진 URL의 authority component 이다. HTTP 터널을 구축하는 경우에만 `CONNECT`와 함께 사용할 수 있다.

       > **CONNECT**  developer.mozilla.org:80  HTTP/1.1

     - `asterisk` 형식:  `OPTIONS`와 함께 별표(`'*'`) 하나로 간단하게 **서버 전체**를 나타낸다. 

       > **OPTIONS**  *  HTTP/1.1

  3. **HTTP 버전**: 메시지의 남은 구조는 명시된 버전에 따라 달라진다. 또한 응답 메시지에서 써야 할 HTTP 버전을 알려주는 역할을 한다.

## 헤더

- HTTP 헤더는 대소문자를 구분하지 않는 **이름, 콜론(`:`), 값**으로 구성된다. 값 앞의 공백은 무시된다.

- 헤더는 값까지 포함해 한 줄로 구성되지만 꽤 길어질 수 있다.

- 헤더는 아래와 같이 몇몇 그룹으로 나눌 수 있다.

  ![image-20210517010606565](C:\Users\bobo\AppData\Roaming\Typora\typora-user-images\image-20210517010606565.png)

  > [이미지 출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)

  - **Request 헤더**: 요청의 내용을 좀 더 **구체화**하고, **컨텍스트를 제공**한다.  조건부로 제한하여 요청 내용을 수정하기도 한다. 

    > 각 헤더 필드 설명
    >
    > - **Host**:  서버의 **도메인 이름**(포트는 Optional)
    >
    > - **User-Agent**: 사용자 에이전트(클라이언트)의 **브라우저, 운영 체제, 플랫폼** 및 버전
    >
    > - **Accept**: 클라이언트가 이해 가능한 (허용하는) **파일 형식** (MIME TYPE)
    >
    > - **Accept-Encoding**: 클라이언트가 해석할 수 있는 **인코딩**, 콘텐츠 압축 방식
    >
    > - **Authorization**: **인증 토큰**(JWT나 Bearer 토큰)을 서버로 보낼 때 사용하는 헤더, API 요청을 할 때 토큰이 없으면 거절을 당하기 때문에 이 때, Authorization을 사용
    >
    > - **Origin**: POST와 같은 요청을 보낼 때, 요청이 **어느 주소에서 시작되었는지**를 나타냄. 여기서 요청을 보낸 주소와 받는 주소가 다르면 [CORS 문제](https://www.zerocho.com/category/NodeJS/post/5a6c347382ee09001b91fb6a)가 발생하기도 함
    >
    > - **Referer**: 이 페이지 **이전의 페이지 주소**가 담겨 있음. 이 헤더를 사용하면 어떤 페이지에서 지금 페이지로 들어왔는지 알 수 있음
    >
    > - **IF-Modified-Since**: 최신 버전 페이지 요청을 위한 필드, 요청의 부하를 줄임. 서버는 지정된 날짜 이후에 마지막으로 수정된 경우에만 200 상태 코드로 요청된 리소스를 다시 보냄, 만약 이후에 리소스가 수정되지 않았다면 서버는 본문 없이 304 상태 코드로 응답을 보냄
    >
    >   ![image-20210517102005568](C:\Users\bobo\AppData\Roaming\Typora\typora-user-images\image-20210517102005568.png)
    >
    >   [이미지 출처](https://goddaehee.tistory.com/169)

  - **General 헤더**: **메시지 전체에 적용**되는 헤더이다.

    > - **Connection**: 현재의 전송이 완료된 후 네트워크 접속을 유지할지 말지를 제어함.  `keep-alive`면, 연결은 지속되고, 동일한 서버에 대한 후속 요청을 수행할 수 있음. 일반적으로 `HTTP/1.1`을 사용하고 Connection은 기본적으로 `keep-alive`로 되어있음.
    > - **[Via](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Via)**:  요청헤더와 응답헤더에 포워드 프록시와 리버스 프록시에 의해서 추가됨. 포워드 메시지를 추적하거나, 요청 루프 방지, 요청과 응답 체인에 따라 송신자의 프로토콜 정보를 식별함

  - **Entity 헤더**: **요청 본문에 적용**되는 헤더이다. 요청 내에 본문이 없는 경우에는 당연히 전송되지 않는다.

    > - **Content-Length**: 요청과 응답 메시지의 **본문 크기**를 **바이트 단위**로 표시해줌. 메시지 크기에 따라 자동으로 만들어짐
    >
    >   예) Content-Length: 345

## 본문

- 본문은 요청의 마지막 부분에 들어가며, 모든 요청에 본문이 들어가지는 않는다. `GET`, `HEAD`, `DELETE` , `OPTIONS`처럼 리소스를 가져오는 요청에서는 보통 본문이 필요없다. 

- 일부 요청은 업데이트를 하기 위해 서버에 데이터를 전송한다. 보통 (HTML Form 데이터를 포함하는) `POST` 요청일 경우 서버에 데이터를 전송한다.

- 넓게 보면 본문은 두 종류로 나뉜다.

  - **단일-리소스 본문**(single-resource bodies): 헤더 두 개([`Content-Type`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type)와 [`Content-Length`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Length))로 정의된 단일 파일로 구성된다.

  - **다중-리소스 본문**(multiple-resource bodies): `multipart` 본문으로 구성되며, 파트마다 다른 정보를 포함한다. 보통 [HTML Form](https://developer.mozilla.org/en-US/docs/Learn/Forms)과 관련이 있습니다.

# 4. Response

- **응답(response)**은 요청에 대한 서버의 답변이다.![image-20210517101019797](C:\Users\bobo\AppData\Roaming\Typora\typora-user-images\image-20210517101019797.png)

  >  [이미지 출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)

## 상태 줄

- HTTP 응답의 시작 줄은 **상태 줄(status line)**이라고 말한다.

- 상태 줄은 아래 세 가지 요소로 구성되어 있다.

  > **프로토콜 버전** + **상태 코드** + **상태 텍스트**

  1. **프로토콜 버전**: 보통 `HTTP/1.1` 이다.
  2. **상태 코드**: 요청의 **성공 또는 실패**를 숫자 코드로 나타낸다. [`200`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/200), [`404`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/404) 혹은 [`302`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/302) 등이 있다.
  3. **상태 텍스트**: 짧고 간결하게 상태 코드에 대한 설명을 글로 나타내어 사람들이 HTTP 메시지를 이해할 때 도움이 됩니다.

- 상태 줄은 일반적으로 `HTTP/1.1 200 OK`,  `HTTP/1.1 404 Not Found.` 와 같이 생겼다.

## 헤더

- 설명은 요청의 헤더와 동일하지만, 복습 차원에서 다시 작성해보면,

- HTTP 헤더는 대소문자를 구분하지 않는 **이름, 콜론(`:`), 값**으로 구성된다. 값 앞의 공백은 무시된다.

- 헤더는 값까지 포함해 한 줄로 구성되지만 꽤 길어질 수 있다.

- 헤더는 아래와 같이 몇몇 그룹으로 나눌 수 있다.

  ![image-20210517101019797](C:\Users\bobo\AppData\Roaming\Typora\typora-user-images\image-20210517101019797.png)

  >  [이미지 출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)

  - **Response 헤더**: 상태 줄에 미처 들어가지 못했던 서버에 대한 추가 정보를 제공한다.

    > - **Access-Control-Allow-Origin**: 요청 Host와 응답 Host가 다르면 CORS 에러가 발생하는데 서버에서 응답 메시지 Access-Control-Allow-Origin 헤더에 프론트 주소를 적어주면 에러가 발생하지 않는다.
    >
    >   예) `Access-Control-Allow-Origin: www.sample.com ` /  `Access-Control-Allow-Origin: *`
    >
    > - **Set-Cookie**: 서버 측에서 클라이언트에게 세션 쿠키 정보를 설정할 때 사용하는 항목 (RFC 2965에서 규정)
    >
    >   예) `Set-Cookie: zerocho=babo; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly`
    >
    >   > **HttpOnly**: 
    >   >
    >   > - 자바스크립트에서 쿠키에 접근할 수 없다.
    >   >
    >   > - XSS 요청을 막으려면 활성화해두는 것이 좋다.
    >
    >   쿠키는 XSS 공격과 CSRF 공격 등에 취약하기 때문에 HttpOnly 옵션을 켜두고, 쿠키를 사용하는 요청은 서버 단에서 검증하는 로직을 마련해두는 것이 좋다.

  - **General 헤더**: **메시지 전체에 적용**되는 헤더이다.

    > - **Server**:  웹 서버 (소프트웨어) 정보
    > - **[Via](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Via)**:   요청헤더와 응답헤더에 포워드 프록시와 리버스 프록시에 의해서 추가된다. 포워드 메시지를 추적하거나, 요청 루프 방지, 요청과 응답 체인에 따라 송신자의 프로토콜 정보를 식별함.

  - **Entity 헤더**: **요청 본문에 적용**되는 헤더이다. 요청 내에 본문이 없는 경우에는 당연히 전송되지 않는다.

    > - **Content-Encoding**: 응답 콘텐츠 압축하는 방식, `br`, `gzip`, `deflate` 등의 알고리즘으로 압축해서 보내면, 브라우저가 알아서 해제해서 사용함, 요청이나 응답 전송 속도도 빨라지고, 데이터 소모량도 줄어들기 때문에 사용함
    >
    > - **Content-Type**: 콘텐츠의 미디어 타입(MIME Type). 반환된 콘텐츠 타입이 실제로 무엇인지 클라이언트에게 알려줌.
    > - **Date**: 현재 날짜
    > - **Last-Modified**: 요청한 파일의 최종 수정일

## 본문

- 본문은 요청의 마지막 부분에 들어가며, 모든 응답에 본문이 들어가지는 않는다. [`201`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/201), [`204`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/204)과 같은 상태 코드를 가진 응답에는 보통 본문이 없다.
- 넓게 보면 본문은 세 종류로 나뉜다.
  - 이미 **길이가 알려진** 단일 파일로 구성된, **단일-리소스 본문**: 헤더 두개([`Content-Type`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type)와 [`Content-Length`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Length))로 정의한다. 
  - **길이를 모르는** 단일 파일로 구성된, **단일-리소스 본문**: [`Transfer-Encoding`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Transfer-Encoding)가 `chunked`로 설정되어 있으며, 파일은 청크로 나뉘어 인코딩 되어 있다. 
  - 서로 다른 정보를 담고 있는 `multipart`로 이루어진 [다중 리소스 본문](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#multipartform-data): 이 경우는 위의 두 경우에 비해 상대적으로 보기 힘들다.

![](https://images.velog.io/images/bky373/post/61945040-f15f-4f9e-8b3d-bf0409f981ab/image.png)

> [이미지 출처](https://www.notion.so/2-Protocol-HTTP-6c75c49407264b87a4c2da4040fd8a88)

# 5. References

- [인터넷과 웹의 개념](https://easy-h.tistory.com/9)
- [SSL(Secure Sockets Layer) 개념 및 동작 원리 알아보기](https://goodgid.github.io/TLS-SSL/)
- [HTTP 개요 - MDN Web Docs](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)
- [[HTTP] HTTP 쿠키란(Cookie)? 쿠키 등장 배경 그리고 쿠키와 세션의 차이점.](https://dololak.tistory.com/?page=262)
- [프로토콜 - IT 위키](https://itwiki.kr/w/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
- [스터디 2주차 : Protocol과 HTTP - 유지님 노션](https://www.notion.so/2-Protocol-HTTP-6c75c49407264b87a4c2da4040fd8a88)
- [HTTP 응답 코드 종류 && HTTP 메소드 종류](https://gyrfalcon.tistory.com/entry/HTTP-%EC%9D%91%EB%8B%B5-%EC%BD%94%EB%93%9C-%EC%A2%85%EB%A5%98-HTTP-%EB%A9%94%EC%86%8C%EB%93%9C-%EC%A2%85%EB%A5%98)
- [알아둬야 할 HTTP 공통 & 요청 헤더](https://www.zerocho.com/category/HTTP/post/5b3ba2d0b3dabd001b53b9db)
- [[HTTP 기초_1] 헤더 (요청(Request) 헤더, 응답(Response)헤더) - 갓대희의 작은공간](https://goddaehee.tistory.com/169) 
- [[Network] HTTP 헤더의 종류 및 항목](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)
