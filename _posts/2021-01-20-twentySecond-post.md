---
title: "리액티브 연산자 - map, filter, reduce, flatMap()"
date: 2021-02-05 21:46:00 -0900
categories: 리액티브 연산자 - map, filter, reduce, flatMap()
---

### 1. map()

- 변환(Transforming)연산자.
- 입력값을 어떤 함수로 넣어서 원하는 값으로변환하는 함수

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd2d62b5-dd9a-4692-81dd-b55171d6f5bb/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd2d62b5-dd9a-4692-81dd-b55171d6f5bb/Untitled.png)

### 2. flatMap()

- 변환(Transforming)연산자.
- return type이 Observable
- one-to-many(1:다)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/724a3efe-3f3e-4f1b-a110-d2b92be1089a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/724a3efe-3f3e-4f1b-a110-d2b92be1089a/Untitled.png)

### 3. filter()

- 필터(filter) 연산자.
- Observable에서 원하는 데이터만 추출하는 역할

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3401673f-05fc-470a-b7a5-f64798937d7d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3401673f-05fc-470a-b7a5-f64798937d7d/Untitled.png)

### 4. reduce()

- 상황에 따라 발행된 데이터를 취합하여 어떤 결과를 만들어 낼 때

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/880acaf3-8d62-4e5e-add7-40e66645f063/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/880acaf3-8d62-4e5e-add7-40e66645f063/Untitled.png)

### 5. interval()

- 생성 연산자.
- 일정시간 간격으로 데이터 흐름 생성
- `interval(long period, TimeUnit unit)` : 일정 시간(period) 쉬었다가 데이터 발행
- `interval(long initialDelay, long period, TimeUnit unit)` : 동작은 같고 최초 지연 시간(initialDelay)을 조절
- `@SchedulerSupport(SchedulerSupport.COMPUTATION)` : 현재 스레드가 아니라 계산을 위한 별도의 스레드(RxJava에서 스케줄러)에서 동작

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07e3a3e0-22f7-41f6-9a71-1c89b39235dd/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07e3a3e0-22f7-41f6-9a71-1c89b39235dd/Untitled.png)

### 6. timer()

- 생성 연산자.
- 일정 시간이 지난 후, 한 개의 데이터를 발행하고 `onComplete()`이벤트 발행

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6a845d9-b1b4-4857-8d79-1ef623bb5c54/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6a845d9-b1b4-4857-8d79-1ef623bb5c54/Untitled.png)