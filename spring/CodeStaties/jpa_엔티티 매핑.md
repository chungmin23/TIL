## 연관관계

1.  FK는 Many가 가진다
2. N:N의 관계는 중간 테이블이 생긴다

|  관계  | Annotation  |
| :----: | :---------: |
| 일대일 |  @OneToOne  |
| 일대다 | @OneToMany  |
| 다대일 | @ManyToOne  |
| 다대다 | @ManyToMany |

~~~java
// 무조건 @Entity와 @id는 써주어어야함
@Entity
public class Item{
	@Id
	@GeneratedValue(strategy= GenerationType.IDENTITY)

}
~~~



FK가 있는곳 

~~~java
@ManyToOne   // (1)
@JoinColumn(name = "MEMBER_ID")  // (2)
private Member member;
~~~

FK없는 연결된 매핑하는곳

~~~java
@OneToMany(mappedBy = "member")
private List<Order> orders = new ArrayList<>();
~~~

### 다대다 매핑 방법

1. 먼저 다대일 매핑을 한다
2. 그다음 일대다 매핑을 해준다