# HTTP

- HTTP 응답에서 Content-Length는 웹 애플리케이션 서버가 자동으로 생성해준다.
- **Content-Type** 은 **HTTP 메시지 바디의 데이터 형식** 을 지정한다.
  - **GET/ URL 쿼리 파라미터 데이터 전송** 은 HTTP 메시지 바디를 사용하지 않기 때문에 Content-Type이 존재하지 않는다.
  - 하지만, **POST/ HTML Form 데이터 전송** 은 HTTP 메시지 바디에 데이터를 포함하기 때문에 Content-Type이 반드시 필요하다.
  - 일반적으로 HTML Form에 데이터를 실어 전송하면, 요청 헤더에 `Content-Type: application/x-form-urlencoded`가 함께 전송된다.
    ![image](https://user-images.githubusercontent.com/49539592/135804940-8dc94273-a106-4944-ab2a-dc8d5341a25a.png)

