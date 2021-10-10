# Postman 소개 및 Mock Server 만들기

## 목차 
  1. [Postman](https://github.com/bky373/line-wiki/blob/main/Postman/Postman_%EC%86%8C%EA%B0%9C_%EB%B0%8F_Mock_Server_%EB%A7%8C%EB%93%A4%EA%B8%B0.md#1-Postman)
  2. [Postman Use Cases](https://github.com/bky373/line-wiki/blob/main/Postman/Postman_%EC%86%8C%EA%B0%9C_%EB%B0%8F_Mock_Server_%EB%A7%8C%EB%93%A4%EA%B8%B0.md#2-Postman-Use-Cases)
  3. [Mock 서버 설정](https://github.com/bky373/line-wiki/blob/main/Postman/Postman_%EC%86%8C%EA%B0%9C_%EB%B0%8F_Mock_Server_%EB%A7%8C%EB%93%A4%EA%B8%B0.md#3-Mock-서버-설정)
  4. [API 문서 만들기](https://github.com/bky373/line-wiki/blob/main/Postman/Postman_%EC%86%8C%EA%B0%9C_%EB%B0%8F_Mock_Server_%EB%A7%8C%EB%93%A4%EA%B8%B0.md#4-API-문서-만들기)
  5. [Postman Mock 서버 장단점](https://github.com/bky373/line-wiki/blob/main/Postman/Postman_%EC%86%8C%EA%B0%9C_%EB%B0%8F_Mock_Server_%EB%A7%8C%EB%93%A4%EA%B8%B0.md#5-Postman-Mock-서버-장단점)
  6. [References](https://github.com/bky373/line-wiki/blob/main/Postman/Postman_%EC%86%8C%EA%B0%9C_%EB%B0%8F_Mock_Server_%EB%A7%8C%EB%93%A4%EA%B8%B0.md#6-References)

# 1. Postman

- 공식 페이지에서는 Postman을 아래와 같이 소개하고 있다.

> The Collaboration Platform for API Development
> **API 개발**을 위한 **협업 플랫폼**

> Postman's features **simplify** **each step** of building an API and **streamline** **collaboration** so you can **create** **better APIs**—**faster**.
> Postman의 기능은 **API 구축**의 각 **단계를 단순화**하고 **협업을 간소화**하여 더 나은 **API를 더 빠르게 만들** 수 있습니다.

- 간단히 소개하면, **Postman**은 **빠른 API 빌딩을 위한 협업 툴**이다.

# 2. Postman Use Cases

- 공식 페이지에서 Postman 이용 사례를 소개하고 있다. 애플리케이션을 개발할 때 Postman이 어떻게 도움을 주는지 짐작할 수 있다. 
- 아래에서는 일부 내용만 정리하였다. 더 많은 내용을 알고 싶다면 [공식 페이지](https://www.postman.com/)를 참조하자.

## API-First Development

- **코드를 작성하기 전에 먼저 API를 빌드**하여 애플리케이션을 빠르게 제공하는 방식이다.
- Postman을 사용해 어떤 방향으로 개발 플로우를 진행할 수 있는지 아래 그림에서 살펴보자.

![](https://images.velog.io/images/bky373/post/2cdc5483-bdaf-4ad5-91e9-6a53abae4be8/image.png)

> [이미지 출처](https://www.postman.com/use-cases/api-first-development/)

1. **명세서 단계 (항목화 단계)**

   - 새로운 API를 생성한다.
   - 새로운 명세(항목)을 작성하거나 import 한다. (선택사항)

2. **개발 단계**

   - `mock` 서버를 생성한다.
   - 문서를 생성한다.
   - API를 디버그한다.

3. **테스트 단계**

   - API를 탐구한다.

   - `Javascript`를 사용해 테스트를 작성한다.

   - `Collection Runner`를 활용해 테스트를 돌리며 결과물을 검증한다.

     > [Collection Runner](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)를 사용하면 지정된 순서로 일련의 요청을 실행할 수 있다.

4. **개발 단계**

   - `Newman`을 통해 빌드 시스템 안에서 테스트들을 통합한다.

   - 준비된 `CI/CD` 파이프라인을 통해 `production` (운영 모드)로 배포한다.

     > [Newman](https://support.postman.com/hc/en-us/articles/115003710329-What-is-Newman-)은 Postman을 위한 명령줄 컬렉션이다. 명령줄에서 직접 Postman 컬렉션을 실행하고 테스트 할 수 있다.

   - 알림을 받기 위해 모니터를 설정한다.

> 2단계와 3단계에서 팀원들을 workspace에 초대하여 업데이트된 내용, 테스트 결과물, 디버깅 정보를 공유할 수 있다.

 ## Application Development

- Postman을 사용하면 **프론트엔드 및 백엔드 팀이 병렬로 작업**할 수 있다. 이로써 **종속성이 제거**되고 **생산 시간을 단축**시켜 결국 **애플리케이션 개발을 빠르게** 진행할 수 있다. 
- 아래 이미지에서 내용을 좀 더 살펴보자.

![](https://images.velog.io/images/bky373/post/20b56026-d8f0-4240-9123-c288b9d97e55/image.png)

> [이미지 출처](https://www.postman.com/use-cases/api-first-development/)

1. **명세서 단계 (항목화 단계)**

   - product 명세서(항목 문서)를 만든다.

2. **프론트엔드 개발 단계**

   - `mock` 서버를 이용해 API 호출을 시뮬레이션해본다.

   - UI 코드와 `mock` 서버를 통합시킨다.

   - `mock` 과 `staging` 서버를 교체하여 사용한다.

     > **staging (스테이징 환경)**: **운영 환경과 거의 동일한 환경**을 만들어 놓고, 운영환경으로 이전하기 전에, 여러 가지 비 기능적인 부분 (Security, 성능, 장애 등)을 검증하는 환경이다.
     >
     > [참고](https://bcho.tistory.com/759)

   - `mock`서버를  `production` 서버로 교체한다. 

3. **백엔드 개발 단계**

   - API 명세 또는 `mock` 서버를 작성한다.
   - 코드를 구현하고 `staging` 단계에서 배포해본다.
   - API 테스트를 작성한다.
   - `production` (운영 모드)로 배포한다.

4. **통합 단계**

   - API를 디버그해서 `production`까지 넘어간다.
   - `Postman Proxy`를 사용하여 API 호출을 테스트한다.
   - 빌드 시스템과 API 테스트를 통합한다.

# 3. Mock 서버 설정

- **`production` API가 준비되지 않았거나, 아직 실제 데이터에 대해 요청을 실행하고 싶지 않은 경우** `mock` 서버를 사용할 수 있다.

- Postman의 `mock` 서버를 사용하면, **가짜(`mock`) 데이터를 반환하는 요청**을 할 수 있다.

- **컬렉션(`collections`)에 `mock` 서버를 추가**하고, **요청에 예제를 추가**하면 실제 API의 동작을 시뮬레이션 할 수 있다.

- `mock` 서버에 요청을 보낼 때 Postman은 미리 정의된 **예제와 요청 구성(`Configuration`)를 맞춰보고**, 일치할 경우 이에 **맞는 데이터를 응답**해준다.

- Postman 왼쪽에 있는 `mock` 서버에서 `workspace`의 가짜 예제 등을 볼 수 있다.
  ![](https://images.velog.io/images/bky373/post/df5c8122-e501-4c2b-bb05-d3d5e09775e7/image.png)

  > [이미지 출처](https://learning.postman.com/docs/designing-and-developing-your-api/mocking-data/setting-up-mock/#creating-mock-servers)

## Mock 서버 생성

> - (참고) Postman에서 Mock 서버를 생성하는 방법은 여러 가지가 있다. 여기에서는 아예 무에서 시작해 Mock 서버를 만드는 과정을 서술하였다.
>
> - 이미 만들어둔 컬렉션 또는 API를 활용해 Mock 서버를 만들고 싶다면 [공식 페이지](https://learning.postman.com/docs/designing-and-developing-your-api/mocking-data/setting-up-mock/#creating-mock-servers)을 참조하자.

- Postman 왼쪽의 `Mock Servers`에서 `+` 버튼을 클릭한다.

  ![](https://images.velog.io/images/bky373/post/a828ce86-2415-422e-944e-fa80863c6e32/image.png)

- 새로운 컬렉션을 추가하거나(`Create a new collection`) 기존 컬렉션을 선택한다(`Select an existing collection`). 여기에서는 새로운 컬렉션을 추가하도록 한다.

- 컬렉션을 새롭게 추가할 경우, 초기 요청을 작성해주어야 한다. `Request URL`에 임시 `URL`를 작성해준다 (추후 수정 가능). 여기에서는 `test` 라고 입력해주었다.

  ![](https://images.velog.io/images/bky373/post/f67067b3-de8b-4954-a56a-4ea5ccf37408/image.png)

- 다음으로 `Next` 버튼을 누르면 `Configuration` 을 작성해야 한다. `Mock server name` 에 `Mock Server Test`라고 입력해주었다. (추후 수정 가능)

- `Mock 서버 URL`을 환경변수로 사용하려면,  `Save the mock server URL as an environment variable` 항목 체크를 유지한다.

- 다음으로 `Create Mock Server` 버튼을 누르면 Mock 서버가 생성된 화면이 나온다. (Mock 서버 생성 끝!)


  ![](https://images.velog.io/images/bky373/post/7fc70ad8-6123-40c0-8c09-e6b08b1382ef/image.png)

## API 예제 작성

![](https://images.velog.io/images/bky373/post/5258b789-dd74-4776-b938-003c1726794c/image.png)

- Mock 서버가 생성되었다면 Postman 왼쪽의 `Collections`를 클릭한다. Mock 서버의 이름과 동일한 이름의 컬렉션이 하나 생성되어 있고, 그 안에는 위에서 Mock 서버를 만들 때 작성한 초기 요청(`GET test`) 템플릿이 생성되어 있다.

- 또한 초기 요청 아래  `Default` 가 하나 있는데, Postman에서는 이를 `example`이라 부른다. API에 대해 Params나 Response 등을 다르게 하여 여러 개의 예제를 생성할 수 있다. `Default`라는 이름은 변경 가능하다.

- 오른쪽 상단에는 API 경로가 나와 있다. `{{url}}/test` 에서 `{{url}}`은 Postman이 관리하는 환경 변수로 왼쪽의 `Environments` 탭에서 확인할 수 있다. 

- 예리한 사람들은 눈치챘겠지만, Mock 서버를 처음 만들 때 사실 환경 세팅을 해주지 않았다. 그래서 오른쪽 우측 상단을 보면 현재 API는 `No Environment`인 상태다. 아래 화살표를 클릭한 후 `Mock Server Test` 환경을 선택한다. (이 설정을 하지 않으면 API 경로에서 `{{url}}`을 인식하지 못해 올바른 API 응답을 받을 수 없다.)

  - 환경 변경 전: `No Environment`

    ![](https://images.velog.io/images/bky373/post/977efe71-10c4-45c4-b217-8e100c9ee294/image.png)

  - 환경 변경 후: `Mock Server Test` 

    ![](https://images.velog.io/images/bky373/post/e0483ab3-9b61-4c74-8398-2437da9c53c7/image.png)

  - 이제 `Default`를 클릭 후,  오른쪽 하단의 `Response Body`를 아래와 같이 간단하게 작성한다.

  - > {
    >
    > 	"menu" : "Americano",
    > 	
    > 	"price" : 4000
    >
    > } 

- 오른쪽 상단의 `Save` 버튼을 눌러 저장 후, 다시 `GET test` API로 돌아간다.
  ![](https://images.velog.io/images/bky373/post/202b2f3a-5cb4-4a5e-812e-93283285d92a/image.png)

## API 호출

- 오른쪽 상단의 환경이 `Mock Server Test`인 상태에서 그 아래 `Send` 버튼을 누른다.

  ![](https://images.velog.io/images/bky373/post/dd7b38b2-eeea-4c8e-91a6-4f0731614809/image.png)

- 제대로 설정이 되어 있다면, 아래와 같이 미리 저장해놓은 응답을 받을 수 있다.

  ![](https://images.velog.io/images/bky373/post/2cfd4e2b-4e22-4e93-9087-0eeba15ce8ce/image.png)

# 4. API 문서 만들기

- Postman은 컬렉션을 기반으로 문서를 생성, 호스팅하며, 실시간으로 동기화하고 브라우저를 통해 접근할 수 있다. 

- 문서를 사용하여 팀원들과 공동 작업을 수행할 수 있다.

  ![](https://images.velog.io/images/bky373/post/53f9db6e-6df6-4a42-a47b-ce6784b95869/image.png)

  > [이미지 출처](https://learning.postman.com/docs/publishing-your-api/documenting-your-api/)

## 문서 생성

- Postman에서 기존 컬렉션에 대한 문서를 생성하려면 왼쪽 `Collections` 탭에서 문서화하기 원하는 컬렉션을 선택한다. 그 후 오른쪽 `Documentation` 탭을 클릭하면 문서를 작성할 수 있다.

  - `Documentation` 탭 클릭 전
    ![](https://images.velog.io/images/bky373/post/d6cc637a-cf21-4832-9d79-cbea3601289f/image.png)

  - `Documentation` 탭 클릭 후
    ![](https://images.velog.io/images/bky373/post/d74cf760-590b-49eb-bf20-da45156bd650/image.png)

- 또는 문서화하기 원하는 컬렉션의 설정 버튼에서  `View documentation`를 클릭하여 넓은 화면에서 문서를 만들고 편집할 수 있다.
  ![](https://images.velog.io/images/bky373/post/19811ab0-c69b-40d1-9d57-73835672dafd/image.png)

- 마지막으로, 왼쪽 메뉴의 `New` 버튼 클릭 후, `API Documentation` 을 클릭하면 새로운 컬렉션 또는 기존 컬렉션으로 문서를 생성할 수 있다.
  ![](https://images.velog.io/images/bky373/post/69be8ffb-e8f1-470c-a81e-8849308cb867/image.png)

  ![](https://images.velog.io/images/bky373/post/c74f8926-1a08-4ac8-b4bf-bf3bd758aed3/image.png)

  ![](https://images.velog.io/images/bky373/post/4a223357-96a9-4cb7-b914-0d0691f50976/image.png)

  ![](https://images.velog.io/images/bky373/post/8daf38e8-6d34-442c-bd95-fc439c39afb0/image.png)

## 문서 편집

- 문서 페이지가 보인다면, 여기저기 마우스를 올려보자. 편집 가능한 영역에 **연필 표시**가 나타날 것이다. 클릭해서 내용을 변경하고, 저장하여 텍스트를 업데이트한다.

  > 참고로 Postman의 문서는 **마크다운 양식**을 지원한다. 

  - 연필 표시 클릭 전

    ![](https://images.velog.io/images/bky373/post/bc614e81-1029-4442-aeda-f8ac62b0cc0c/image.png)

  - 연필 모양 클릭 후

    ![](https://images.velog.io/images/bky373/post/42e0dd1f-2f5f-4cdf-9cae-a6d11bab857c/image.png)

## 브라우저에서 문서 공유

- 문서는 기본적으로 `private` 이기 때문에 컬렉션을 공유한 사람만 문서를 볼 수 있다. 

- 문서를 브라우저 상에 공개적으로 공유하고 싶다면, 문서를 확장한 화면 우측의 `Publish` 버튼을 클릭한다.

  ![](https://images.velog.io/images/bky373/post/a1d43ff8-a21b-4f7f-a1df-ee617850df71/image.png)

- 브라우저 상에 아래 화면이 나타나면, 위에서 설정한 `Mock Server Test`  환경을 클릭하고, 아래 URL, 스타일링 등의 항목을 설정한 후 맨 아래 `Save` 버튼을 누른다. (추후 변경이 가능하므로 여기서는 기본값으로 설정함)

  ![](https://images.velog.io/images/bky373/post/bb9bdc78-be32-4ede-b5b2-cf3a787eca4f/image.png)

- `Publish`를 한 번 했다가 취소하고 다시 `Publish`를 해서 이미지에서는 `Save and republish` 버튼이 나타났다.

  ![](https://images.velog.io/images/bky373/post/a28c3908-abfc-47f9-855a-f4f95e3b38e4/image.png)

- 이제 아래와 같은 화면이 나오면 `Publish`에 성공한 것이다!

  ![](https://images.velog.io/images/bky373/post/cef6a793-795a-49e2-b878-c1a18366c9b7/image.png)

- 화면 중앙의 URL로 접속하면 브라우저 상에서 작성한 API 문서를 확인할 수 있다.

  ![](https://images.velog.io/images/bky373/post/40d853a7-e03f-4ecd-9081-b4211e76dee5/image.png)



## 문서 세부 정보

- 문서에는 다양한 클라이언트 언어로 된 샘플 코드와 요청에 대한 세부 정보가 포함된다. 
- 각 컬렉션 및 요청 목록에서
  - `method`
  - `Authorization Type`
  - `URL`
  - `description`
  - `headers`
  - `request` 및 `response` 구조 정보를 확인할 수 있다.

# 5. Postman Mock 서버 장단점

## 장점

- GUI 방식으로 Mock Server를 손쉽게 생성할 수 있다. 
  (서버를 직접 구성할 필요가 없음)
- 동일한 플랫폼 내에서 API 문서를 쉽게 만들고 공유할 수 있다.
- CORS가 설정되어 있다.
- 환경마다 환경 변수의 값을 다르게 설정할 수 있다.

## 단점

- 호출 횟수에 제한이 있다. (1분당 60회)
- API 예제 중 `Path`,  `Params`,  `Headers`, `Body`가 같은 것이 있으면 최근에 추가된 예제가 호출된다.
  - 하지만,  `Path`,  `Params`,  `Headers`, `Body`를 다르게 설정할 수 있는 장점을 가진다.  



# 6. References

- [POSTMAN](https://www.postman.com/)
- [POSTMAN Learning Center](https://learning.postman.com/docs/designing-and-developing-your-api/mocking-data/setting-up-mock/)
- [[Postman] Mock Server / API Documentation 만들기 - 불곰님](https://brownbears.tistory.com/448)
