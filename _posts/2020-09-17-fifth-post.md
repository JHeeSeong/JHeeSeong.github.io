---
title: "Dirty Checking이란?"
date: 2020-09-25 09:26:00 -0400
categories: Dirty Checking
---


# #4. Dirty Checking

- JPA 더티 체킹을 이용하면  Update 쿼리 없이 테이블 수정이 가능하다는 것

```java
@RequiredArgsConstructor
@Service
public class PostsService {
    private final PostsRepository postsRepository;

    @Transactional
    public Long update(Long id, PostsUpdateRequestDto requestDto) {
        Posts posts = postsRepository.findById(id).orElseThrow(() -> new IllegalArgumentException("해당 사용자가 없습니다. id=" + id));

        posts.update(requestDto.getTitle(), requestDto.getContent());
        return id;
    }
}
```

- 위의 예제의 update 메소드에서 데이터 베이스로 쿼리를 날리는 부분이 없습니다.
(`posts.update(requestDto.getTitle(), requestDto.getContent())`의 경우는 Post 객체의 update 메소드이다.)
- 그 이유는 JPA의 영속성 컨텍스트 때문입니다.
- 해당 데이터의 값을 변경하면 데이터 베이스에 저장하는 것이 아닌 엔티티 매니저를 이용하여 영속성 컨텍스트에 저장하는 것이다.
- 즉, 해당 데이터의 값을 변경하면 트랜잭션이 끝나는 시점에 해당 테이블에 변경분을 반영하는 것이다.

### 엔티티 매니저란?

- Entity의 CRUD 등 Entitiy와 관련된 모든 일을 처리한다.

### 영속성 컨텍스트란?

- Entity를 영구 저장하는 환경입니다.





참고 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스