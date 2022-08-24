### DTO

data tranfer object 

### dto가 필요한 이유

요청 데이터를 하나의 객체로 전달받는 역할을 한다 

Request Body의 데이터 유효성(Validation) 검증이 단순해진다

- JSON 형식의 Request Body를 전달 받기 위해서는 DTO 객체에 `@RequestBody` 애너테이션을 붙여야 한다.



