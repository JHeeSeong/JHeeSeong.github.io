---
title: "Controller, Repository, Service!"
date: 2020-10-12 13:50:00 -0400
categories: Controller, Repository, Service
---

Contoller Layer

- 서비스와 뷰의 중재자
- client가 이용할 End Point
- client 요청을 어떻게 처리할 지 정의하는 곳(client의 request가 서버에 도착했을 때)
- client 요청을 처리하곻 어떻게 응답할 지 결정하는 곳

Repository

- DAO Layer
- 각종 다양한 storage에 데이터를 조회, 저장, 수정, 삭제 하기 위한 모든 객체들의  Layer

Service

- Business Logic이 들어가 있는 가장 중요한  Layer
- 데이터를 받아 비지니스 로직을 처리한다.
- DAO와 1:1 매핑한다.

뷰(View)란?

- 사용자에게 초기화면을 보여준다.
- 사용자가 입력한 데이터를 컨트롤러에게 보낸다.

- Service ↔ Repository (Entity로 전달)
- Controller ↔ Service (DTO로 전달)
- Entity → DTO로 변경하려면 `of()` 
DTO → Entity로 변경하려면 `toEntity()`

참고 : [https://velog.io/@sumusb/Spring-Service-Layer에-대한-고찰](https://velog.io/@sumusb/Spring-Service-Layer%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)
