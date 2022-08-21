### Spring DI

---

1. 변경에 유리한 코드 - 다형성을 이용

2. map과 외부파일 



### 분리

1. 변하는것, 변하지 않는것 
2. 관심사
3. 중복코드 (AOP)



## 2. 객체컨테이너 만들기

![image-20220811172836857](image/SpringDI/image-20220811172836857.png)

ApplicationContext에 map형식으로 key와 value형식으로 값을 추가를 해준다

getBean("car"); 형식으로 값을 가져온다



### 자동객체등록 - Component scanning

- 패캐지내의 모든 클래스를 읽어서 어노테이션이 붙은 클래스를 객체를 생성해서  map에 저장한다

~~~java
@Component class Car{}
@Component class Engine{}
~~~



### 객체찾기 - by name, by type

~~~java
AppContext ac = new AppContext();
Car car = (Car) ac.getBean("car"); //이름으로 찾기
Car car2 = (Car) ac.getBean(Car.class); // 타입으로 찾기

//이름으로 찾기
Object getBean(String key) {
    return map.get(key);
} // byName

// 타입으로 찾기
Object getBean(Class clazz) { // byType
    for(Object obj : map.values()){
        if(clazz.isInstance(obj))
            return obj;
    }
    return null;
}
~~~



### 객체 자동연결

@Autowired - by type

@Resource - by name

---

## 이론

ApplicationContext의 메서드

![image-20220820204427106](image/SpringDI/image-20220820204427106.png)

### IOC와 DI

![image-20220820204612800](image/SpringDI/image-20220820204612800.png)

### @Autowired

![image-20220820204839144](image/SpringDI/image-20220820204839144.png)

생성자의 @Autowired 생략가능 

![image-20220820204952219](image/SpringDI/image-20220820204952219.png)

@Resource

![image-20220820205048617](image/SpringDI/image-20220820205048617.png)

@Component

![image-20220820205215837](image/SpringDI/image-20220820205215837.png)

@Compoent 어노테이션이 달려있는곳에 빈으로 등록 

다른 @Controller, @Serice등을 써주어도 안에 @Component가 포함이 되어있어서 똑같이 스캔이 되어 빈으로 등록이 된다

### 빈 초기화

![image-20220820205521511](image/SpringDI/image-20220820205521511.png)

![image-20220820205533880](image/SpringDI/image-20220820205533880.png)

![image-20220820205542169](image/SpringDI/image-20220820205542169.png)
