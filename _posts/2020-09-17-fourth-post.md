---
title: "JPA Auditing이란?"
date: 2020-09-17 16:05:00 -0400
categories: JPA Auditing
---

### JPA Auditing을 이용하여 등록.수정 시간을 자동화하는 방법

- Auditing이란 ? 감사/감시 하다라는 의미입니다.
                         JPA에서 시간에 대해 자동으로 값을 넣어주는 기능입니다.

추상 클래스 BaseTimeEntity를 생성합니다. 이 클래스는 추후에 Entity들의 createDate, modifiedDate를 자동으로 관리하는 역할을 할 것입니다.

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {
    @CreatedDate
    private LocalDateTime createDate;
    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

- `@Getter` : 클래스 내 모든 필드의 Getter 메소드를 자동 생성
- `@MappedSuperclass` : JPA Entity 클래스들이 해당 클래스를 상속할 경우 필드들도 칼럼으로 인식하도록 한다.
- `@EntityListeners(AuditingEntityListener.class)` : 해당 클래스에 auditing 기능을 포함
- `@CreatedDate` : Entity가 생성되어 저장될 때 시간이 자동 저장
- `@LastModifiedDate` : 조회된 Entity의 값을 변경할 때 시간이 자동 저장됩니다.

Posts 클래스는 BaseTimeEntity를 상속 받는다. 위 문장의 의미는 Posts의 column을 명시하지 않고 상속받은 BaseTimeEntity의 createDate, modifiedDate를 column으로 명시하여 사용하겠다라는 의미이다.

```java
@Getter
@NoArgsConstructor
@Entity
public class Posts extends BaseTimeEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT" , nullable = false)
    private String content;

    private String author;

    @Builder
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```

- `@EnableJpaAuditing` : JPA Auditing 어노테이션들을 모두 활성화 한다.

```java
@EnableJpaAuditing
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### JPA Auditing Test Code

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {

    @LocalServerPort
    private int port;
    @Autowired
    private TestRestTemplate restTemplate;
    @Autowired
    private PostsRepository postsRepository;

    @After
    public void tearDown() throws Exception {
        postsRepository.deleteAll();
    }

    @Test
    public void BaseTimeEntity_등록() {
        LocalDateTime now = LocalDateTime.of(2020,9,14,0,0,0);
        postsRepository.save(Posts.builder().title("title").content("content").author("author").build());

        List<Posts> postsList = postsRepository.findAll();

        Posts posts = postsList.get(0);
        System.out.println("create Date : " + posts.getCreateDate() + ", modified Date : " + posts.getModifiedDate());

        assertThat(posts.getCreateDate()).isAfter(now);
        assertThat(posts.getModifiedDate()).isAfter(now);
    }
}
```

| ![3JPA Auditing Test code]({{"https://github.com/JHeeSeong/JHeeSeong.github.io/blob/gh-pages/assets/images/3_img.png"}}) |


*참고 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스

