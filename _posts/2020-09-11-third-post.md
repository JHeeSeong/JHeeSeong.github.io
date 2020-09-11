---
title: "Spring Web Layer"
date: 2020-09-11 10:30:00 -0400
categories: Spring
---

### Web Layer
- 컨트롤러(@Controller) 와 JSP/Freemaker 등의 뷰 템플릿 영역

- 필터(@Filter), 인터셉터, 컨트롤러 어드바이스(@ControllerAdvice) 등 외부 요청과 응답에 대한 전반적인 영역

### Service Layer
- @Service에서 사용되는 서비스 영역

- @Transactional이 사용되어야 하는 영역

### Repository Layer
- Database와 같이 데이터 저장소에 접근하는 영역

### Dtos
- Dto(Data Transfer Object)는 계층 간에 데이터 교환을 위한 객체를 이야기하며 Dtos는 이들의 영역

### Domain Model
- @Entity가 사용된 영역

- 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이야기 할 수 있고 공유할 수 있도록 단순화시킨 것을 도메인 모델이라 한다.
