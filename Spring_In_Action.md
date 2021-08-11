# Spring in Action

- **책 정보**

  - 저자 : 크레이그 월즈
  - 옮긴이 : 심채철
  - 출판사 : 제이펍
  - 출간일 : 2020년 05월 14일
  - 에디션 : 5th Edition
  - 관련링크 :  [실습용 소스 코드](https://github.com/Jpub/SpringInAction5)

- **도메인 객체에 애노테이션 추가하기(p104~)**

  - 특정 클래스를 JPA 개체(entity)로 선언하려면 반드시 @Entity 애노테이션을 ㅜ가해야 한다. 그리고 이것의 id 속성에는 반드시 @Id 를 지정하여 이 속성이 데이터베이스의 개체를 고유하게 식별한다는 것을 나타내야 한다. 

  - JPA에서는 개체가 인자 없는 생성자를 가져야 한다. 이를 위해 Lombok의 @NoArgsConstructor를 지정한다. 하지만 인자 없는 생성자의 사용을 원하지 않을 경우, access 속성을 AccessLevel.PRIVATE으로 설정하여 클래스 외부에서 사용하지 못하게 만들 수 있다. 그리고 초기화가 필요한 final 속성이 있다면 force 속성을 true로 설정한다. 이에 따라 Lombok이 자동 생성한 생성자에서 그 속성들을 null로 설정할 수 있다. 아래 예시처럼 작성하면 된다.

    ```java
    @NoArgsConstructor(access=AccessLevel.PRIVATE, force=true)
    ```

    
