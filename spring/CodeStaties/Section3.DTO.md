### DTO

data tranfer object 

데이터 전송 객체

계층간 데이터 교환을 위해서 만든 객체

도메인 또는 VO라고도 부름

- 스프링 부트STS툴에서 패키지 폴더를 domain, dto, vo라고 생성함



### dto가 필요한 이유

요청 데이터를 하나의 객체로 전달받는 역할을 한다 

- Request Body의 데이터 유효성(Validation) 검증이 단순해진다

- JSON 형식의 Request Body를 전달 받기 위해서는 DTO 객체에 `@RequestBody` 애너테이션을 붙여야 한다.











### VO혹은 엔티티 

데이터를 객체라는 단위로 처리한것 



### VO와 DTO 차이

DTO는 각 계층을 오고 가는데 사용되는 택배상자와 비슷하지만 VO는 데이터베이스의 엔티티를 자바 객체로 표현한 것이다 

DTO는 getter/setter 를 이용해서 자유롭게 데이터를 가공할수 있는데 비해 VO는 주로 데이터 자체를 의미하기 때문에 getter만 이용하는 경우가 대부분이다





## DAO(Data Access Object)

- 데이터 접근 객체라고함
  1. mybatis - dao , mapper 구현
  2. jpa - jpa repository 이용
- Service 계층과 DB를 연결해주는 역할



