### DTO

data tranfer object 

### dto가 필요한 이유

요청 데이터를 하나의 객체로 전달받는 역할을 한다 

Request Body의 데이터 유효성(Validation) 검증이 단순해진다

- JSON 형식의 Request Body를 전달 받기 위해서는 DTO 객체에 `@RequestBody` 애너테이션을 붙여야 한다.



### VO혹은 엔티티 

데이터를 객체라는 단위로 처리한것 



### VO와 DTO 차이

DTO는 각 계층을 오고 가는데 사용되는 택배상자와 비슷하지만 VO는 데이터베이스의 엔티티를 자바 객체로 표현한 것이다 

DTO는 getter/setter 를 이용해서 자유롭게 데이터를 가공할수 있는데 비해 VO는 주로 데이터 자체를 의미하기 때문에 getter만 이용하는 경우가 대부분이다
