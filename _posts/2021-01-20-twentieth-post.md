---
title: "Observer 패턴"
date: 2021-01-15 18:40:00 -0900
categories: Observer 패턴
---

### Observer 패턴이란?

- 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에게 그 정보를 알려주고 자동으로 내용이 갱신되는 디자인 패턴(one-to-many 의존성)
- Java에서 Observable, Observer를 제공

### Hot Observable

- 생성과 동시에 이벤트 방출
- 구독 시점으로 부터 발행하는 값을 받는 것을 기본
- subscriber의 존재 여부 없이 데이터 발행

### Ice Observable

- Observer가 subscribe 되는 시점부터 이벤트를 생성하여 방출
- subscriber가 구독하기 전까지 데이터 발행 안함

> 해당 포스팅은 '태임쓰의 개발블로그', 'reactivex.io' 블로그를 보면서 정리한 내용입니다.