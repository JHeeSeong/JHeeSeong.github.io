---
title: "RX Java란?"
date: 2020-11-04 15:45:00 -0900
categories: RXJava, Reactive Programming
---

## 리액티브 프로그래밍과 RxJava

### 1.1 리액티브 프로그래밍이란?

- 기존 pull방식의 프로그래밍 개념을 push 방식의 프로그래밍 개념으로 변경한다.
- 함수형 프로그래밍의 지원을 받는다.
→ 함수형 프로그래밍은 부수 효과가 없는 순수 함수를 지향한다. 따라서, 멀티스레드 환경에서도 안전하다. 
→ 함수형 프로그래밍 도구를 활용한 비동기 프로그래밍

### 1.2 RxJava를 만든 이유

- 동시성을 적극적으로 끌어안을 필요가 있다. (Embrance Concurrency)
→ 클라이언트의 요청을 처리할 때 다수의 비동기 실행 흐름(스레드 등)을 생성하고 그것의 결과를 취합하여 최종 리턴하는 방식
- 자바 Future를 조합하기 어렵다는 점을 해결해야 한다. (Java Futures are Expensive to Compose)
→ 비동기 흐름을 조합할 수 있는 방법 제공(조합하는 실행 단위 : 리액티브 연산자)
→ 리액티브 연산자를 활용하면 목적을 달성할 수 있는 도구인 ‘함수형 프로그래밍’ 방식으로 ‘스레드에 안전한 비동기 프로그램’을 작성할 수 있음
- 콜백 방식의 문제점을 개선해야 한다. (Callbacks Have Their Own Problems)
→ 콜백을 사용하지 않는 방향으로 설계하여 RxJava는 이러한 문제점을 개선하였다.

### 1.3 예시

```java
Observable.just("Hello", "RxJava2", "World", "!")
                .subscribe(System.out::println);
// Observable : 리액티브 프로그래밍의 시작
// subscribe() : 변화한 데이터를 구독자에게 발행
// just() : 가장 단순한 Observable의 선언 방식이며, 인자의 타입은 동일해야 하며 최대 10개
//        : 데이터를 넣으면 자동으로 이벤트 발생
```

- subscribe() 인자의 개수 별로 달라진다.
    - `[인자 없음] Disposable subscribe()` : `onNext`와 `onComplete` 이벤트를 무시하고 `onError` 이벤트가 발생했을 때만 `onErrorNotImplementedException`을 던집니다. 테스트하거나 디버깅할 때 이용
    - `[인자 1개] Disposable subscribe(Consumer<? super T> onNext)` : `onNext`를 처리합니다.
    - `[인자 2개] Disposable subscribe (Consumer<? super T> onNext, Consumer<? super java.lang.Throwable> onError)` : `onNext`와 `onError` 이벤트를 처리합니다.
    - `[인자 3개] Disposable subscibe(Consumer<? super T> onNext, Consumer<? super java.lang.Throwable> onError, Action OnComplete)` : `onNext`와 `onError`, `onComplete` 이벤트를 처리합니다.
    

### Observable 연산자
-  Observable 생성
> Create, Defer, Empty/Never/Throw, From, Interval, Just, Range, Repeat, Start, 그리고 Timer
-  Observable 항목 변환
> Buffer, FlatMap, GroupBy, Map, Scan, 그리고 Window
-  Observable 필터
> Debounce, Distinct, ElementAt, Filter, First, IgnoreElements, Last, Sample, Skip, SkipLast, Take, 그리고 TakeLast
-  Observable 결합
> And/Then/When, CombineLatest, Join, Merge, StartWith, Switch, 그리고 Zip
-  오류 처리 연산자
> Catch 그리고 Retry
-  유틸리티 연산자
> Delay, Do, Materialize/Dematerialize, ObserveOn, Serialize, Subscribe, SubscribeOn, TimeInterval, Timeout, Timestamp, 그리고 Using
-  조건 및 불린(Boolean) 연산자
> All, Amb, Contains, DefaultIfEmpty, SequenceEqual, SkipUntil, SkipWhile, TakeUntil, 그리고 TakeWhile
-  수학과 조합 연산자
> Average, Concat, Count, Max, Min, Reduce, 그리고 Sum
-  변환 Observable
> To
-  연결 가능한 Observable 연산자
> Connect, Publish, RefCount, 그리고 Replay
-  역압(backpressure) 연산자
> 특정 제어흐름 원칙들을 적용하는 다양한 연산자들

    
> 해당 포스팅은 '태임쓰의 개발블로그', 'reactivex.io' 블로그를 보면서 정리한 내용입니다.