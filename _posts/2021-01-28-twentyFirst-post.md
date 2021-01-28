---
title: "Observable 연산자"
date: 2021-01-28 20:10:00 -0900
categories: Observable 연산자
---

### 1. **Observable 생성**

> `Create`, `Defer`, `Empty`/`Never`/`Throw`, `From`, `Interval`, `Just`, `Range`, `Repeat`, `Start`, 그리고 `Timer`

- **`[Create](http://reactivex.io/documentation/operators/create.html)`** — 직접적인 코드 구현을 통해 옵저버 메서드를 호출하여 Observable을 생성한다
- **`[Defer](http://reactivex.io/documentation/operators/defer.html)`** — 옵저버가 구독하기 전까지는 Observable 생성을 지연하고 구독이 시작되면 옵저버 별로 새로운 Observable을 생성한다
- **`[Empty`/`Never`/`Throw`](http://reactivex.io/documentation/operators/empty-never-throw.html)** — 아주 정확하고 제한된 행동을 하는 Observable을 생성한다
- **`[From](http://reactivex.io/documentation/operators/from.html)`** — 다른 객체나 자료 구조를 Observable로 변환한다
- **`[Interval](http://reactivex.io/documentation/operators/interval.html)`** — 특정 시간별로 연속된 정수형을 배출하는 Observable을 생성한다
- **`[Just](http://reactivex.io/documentation/operators/just.html)`** — 객체 하나 또는 객채집합을 Observable로 변환한다. 변환된 Observable은 원본 객체들을 발행한다
- **`[Range](http://reactivex.io/documentation/operators/range.html)`** — 연속된 범위(Range)의 정수를 발행하는 Observable을 생성한다
- **`[Repeat](http://reactivex.io/documentation/operators/repeat.html)`** — 특정 항목이나 연속된 항목들을 반복적으로 배출하는 Observable을 생성한다
- **`[Start](http://reactivex.io/documentation/operators/start.html)`** — 함수의 실행 결과를 배출하는 Observable을 생성한다
- **`[Timer](http://reactivex.io/documentation/operators/timer.html)`** — 지정된 시간이 지나고 난 후 항목을 하나 배출하는 Observable을 생성한다

### 2. **Observable 항목 변환**

> `Buffer`, `FlatMap`, `GroupBy`, `Map`, `Scan`, 그리고 `Window`

- **`[Buffer](http://reactivex.io/documentation/operators/buffer.html)`** — Observable로부터 정기적으로 항목들을 수집하고 묶음으로 만든 후에 묶음 안에 있는 항목들을 한번에 하나씩 배출하지 않고 수집된 묶음 단위로 배출한다
- **`[FlatMap](http://reactivex.io/documentation/operators/flatmap.html)`** — 하나의 Observable이 발행하는 항목들을 여러개의 Observable로 변환하고, 항목들의 배출을 차례차례 줄 세워 하나의 Observable로 전달한다
- **`[GroupBy](http://reactivex.io/documentation/operators/groupby.html)`** — 원본 Observable이 배출하는 항목들을 키(Key) 별로 묶은 후 Observable에 담는다. 이렇게 키 별로 만들어진 Observable들은 자기가 담고 있는 묶음의 항목들을 배출한다
- **`[Map](http://reactivex.io/documentation/operators/map.html)`** — Observable이 배출한 항목에 함수를 적용한다
- **`[Scan](http://reactivex.io/documentation/operators/scan.html)`** — Observable이 배출한 항목에 연속적으로 함수를 적용하고 실행한 후 성공적으로 실행된 함수의 리턴 값을 발행한다
- **`[Window](http://reactivex.io/documentation/operators/window.html)`** — 정기적으로 Observable의 항목들을 더 작은 단위의 Observable 윈도우로 나눈 후에, 한번에 하나씩 항목들을 발행하는 대신 작게 나눠진 윈도우 단위로 항목들을 배출한다

### 3. **Observable 필터**

> `Debounce`, `Distinct`, `ElementAt`, `Filter`, `First`, `IgnoreElements`, `Last`, `Sample`, `Skip`, `SkipLast`, `Take`, 그리고 `TakeLast`

- **`[Debounce](http://reactivex.io/documentation/operators/debounce.html)`** — Observable의 시간 흐름이 지속되는 상태에서 다른 항목들은 배출하지 않고 특정 시간 마다 그 시점에 존재하는 항목 하나를 Observable로부터 배출한다
- **`[Distinct](http://reactivex.io/documentation/operators/distinct.html)`** — Observable이 배출하는 항목들 중 중복을 제거한 항목들을 배출한다
- **`[ElementAt](http://reactivex.io/documentation/operators/elementat.html)`** — Obserable에서 번째 항목만 배출한다
- **`[Filter](http://reactivex.io/documentation/operators/filter.html)`** — 테스트 조건을 만족하는 항목들만 배출한다
- **`[First](http://reactivex.io/documentation/operators/first.html)`** — 맨 첫 번째 항목 또는 조건을 만족하는 첫 번째 항목만 배출한다
- **`[IgnoreElements](http://reactivex.io/documentation/operators/ignoreelements.html)`** — 항목들을 배출하지는 않고 종료 알림은 보낸다
- **`[Last](http://reactivex.io/documentation/operators/last.html)`** — Observable의 마지막 항목만 배출한다
- **`[Sample](http://reactivex.io/documentation/operators/sample.html)`** — 특정 시간 간격으로 최근에 Observable이 배출한 항목들을 배출한다
- **`[Skip](http://reactivex.io/documentation/operators/skip.html)`** — Observable이 배출한 처음 개의 항목들을 숨긴다
- **`[SkipLast](http://reactivex.io/documentation/operators/skiplast.html)`** — Observable이 배출한 마지막 개의 항목들을 숨긴다
- **`[Take](http://reactivex.io/documentation/operators/take.html)`** — Observable이 배츨한 처음 개의 항목들만 배출한다
- **`[TakeLast](http://reactivex.io/documentation/operators/takelast.html)`** — Observable이 배출한 마지막 개의 항목들만 배출한다

### 4. O**bservable 결합**

> `And`/`Then`/`When`, `CombineLatest`, `Join`, `Merge`, `StartWith`, `Switch`, 그리고 `Zip`

- **`[And`/`Then`/`When`](http://reactivex.io/documentation/operators/and-then-when.html)** — 두 개 이상의 Observable들이 배출한 항목들을 'Pattern'과 'Plan' 중계자를 이용해서 결합한다
- **`[CombineLatest](http://reactivex.io/documentation/operators/combinelatest.html)`** — 두 개의 Observable 중 하나가 항목을 배출할 때 배출된 마지막 항목과 다른 한 Observable이 배출한 항목을 결합한 후 함수를 적용하여 실행 후 실행된 결과를 배출한다
- **`[Join](http://reactivex.io/documentation/operators/join.html)`** — A Observable과 B Observable이 배출한 항목들을 결합하는데, 이때 B Observable은 배출한 항목이 타임 윈도우를 가지고 있고 이 타임 윈도우가 열린 동안 A Observable은 항목의 배출을 계속한다. Join 연산자는 B Observable의 항목을 배출하고 배출된 항목은 타임 윈도우를 시작시킨다. 타임 윈도우가 열려 있는 동안 A Observable은 자신의 항목들을 계속 배출하여 이 두 항목들을 결합한다
- **`[Merge](http://reactivex.io/documentation/operators/merge.html)`** — 복수 개의 Observable들이 배출하는 항목들을 머지시켜 하나의 Observable로 만든다
- **`[StartWith](http://reactivex.io/documentation/operators/startwith.html)`** — 소스 Observable이 항목을 배출하기 전에 다른 항목들을 앞에 추가한 후 배출한다
- **`[Switch](http://reactivex.io/documentation/operators/switch.html)`** — Observable들을 배출하는 Observable을 싱글 Observable로 변환하다. 변환된 싱글 Observable은 변환 전 소스 Observable들이 배출한 항목들을 배출한다
- **`[Zip](http://reactivex.io/documentation/operators/zip.html)`** — 명시한 함수를 통해 여러 Observable들이 배출한 항목들을 결합하고 함수의 실행 결과를 배출한다

### 5. **오류 처리 연산자**

> `Catch` 그리고 `Retry`

- **`[Catch](http://reactivex.io/documentation/operators/catch.html)`** — 오류를 무시하고 배출되는 항목들을 계속 진행시켜 'onError'로부터 전달된 오류를 복구한다
- **`[Retry](http://reactivex.io/documentation/operators/retry.html)`** — 만약 소스 Observable이 'onError' 알림을 보낼 경우, 오류 없이 실행이 완료되기를 기대하며 재구독을 시도한다

### 6. **유틸리티 연산자**

> `Delay`, `Do`, `Materialize`/`Dematerialize`, `ObserveOn`, `Serialize`, `Subscribe`, `SubscribeOn`, `TimeInterval`, `Timeout`, `Timestamp`, 그리고 `Using`

- **`[Delay](http://reactivex.io/documentation/operators/delay.html)`** — Observable의 배출을 특정 시간동안 미룬다
- **`[Do](http://reactivex.io/documentation/operators/do.html)`** — Observable의 생명주기 동안 발생하는 여러 이벤트에서 실행 될 액션을 등록한다
- **`[Materialize`/`Dematerialize`](http://reactivex.io/documentation/operators/materialize-dematerialize.html)** — 배출된 항목이 어떤 알림을 통해 옵저버에게 전달 됐는지를 표현하며, 그 반대 과정을 수행할 수 있다
- **`[ObserveOn](http://reactivex.io/documentation/operators/observeon.html)`** — 옵저버가 어느 스케줄러 상에서 Observable을 관찰할지 명시한다
- **`[Serialize](http://reactivex.io/documentation/operators/serialize.html)`** — Observable이 직렬화된 호출을 생성하고 제대로 동작하도록 강제한다
- **`[Subscribe](http://reactivex.io/documentation/operators/subscribe.html)`** — Observable이 배출하는 항목과 알림을 기반으로 동작한다
- **`[SubscribeOn](http://reactivex.io/documentation/operators/subscribeon.html)`** — Observable을 구독할 때 사용할 스케줄러를 명시한다
- **`[TimeInterval](http://reactivex.io/documentation/operators/timeinterval.html)`** — 항목들을 배출하는 Observable을, 항목을 배출하는데 걸린 시간이 얼마인지를 가리키는 Observable로 변환한다
- **`[Timeout](http://reactivex.io/documentation/operators/timeout.html)`** — 소스 Obvservable을 그대로 전달하지만, 대신 특정 시간 동안 배출된 항목이 없으면 오류 알림을 보낸다
- **`[Timestamp](http://reactivex.io/documentation/operators/timestamp.html)`** — Observable이 배출한 항목에 타임 스탬프를 추가한다
- **`[Using](http://reactivex.io/documentation/operators/using.html)`** — 소스 Observable과 동일한 생명주기를 갖는 Observable을 생성하는데, 이 Observable은 생명주기가 완료되면 리소스를 종료하고 반환한다

### 7. **조건 및 불린(Boolean) 연산자**

> `All`, `Amb`, `Contains`, `DefaultIfEmpty`, `SequenceEqual`, `SkipUntil`, `SkipWhile`, `TakeUntil`, 그리고 `TakeWhile`

- **`[All](http://reactivex.io/documentation/operators/all.html)`** — Observable이 배출한 전체 항목들이 어떤 조건을 만족시키는지 판단한다
- **`[Amb](http://reactivex.io/documentation/operators/amb.html)`** — 두 개 이상의 소스 Observable이 주어 질때, 그 중 첫 번째로 항목을 배출한 Observable이 배출하는 항목들을 전달한다
- **`[Contains](http://reactivex.io/documentation/operators/contains.html)`** — Observable이 특정 항목을 배출하는지 아닌지를 판단한다
- **`[DefaultIfEmpty](http://reactivex.io/documentation/operators/defaultifempty.html)`** — 소스 Observable이 배출하는 항목을 전달한다. 만약 배출되는 항목이 없으면 기본 항목을 배출한다
- **`[SequenceEqual](http://reactivex.io/documentation/operators/sequenceequal.html)`** — 두 개의 Observable이 항목을 같은 순서로 배출하는지 판단한다
- **`[SkipUntil](http://reactivex.io/documentation/operators/skipuntil.html)`** — 두 번째 Observable이 항목을 배출하기 전까지 배출된 항목들을 버린다
- **`[SkipWhile](http://reactivex.io/documentation/operators/skipwhile.html)`** — 특정 조건이 false를 리턴하기 전까지 Observable이 배출한 항목들을 버린다
- **`[TakeUntil](http://reactivex.io/documentation/operators/takeuntil.html)`** — 두 번째 Observable이 항목을 발행하기 시작햤거나 두 번째 Observable이 종료되면 그 때부터 발행되는 항목들은 버린다
- **`[TakeWhile](http://reactivex.io/documentation/operators/takewhile.html)`** — 특정 조건이 false를 리턴하기 시작하면 그 이후에 배출되는 항목들을 버린다

### 8. **수학과 조합 연산자**

> `Average`, `Concat`, `Count`, `Max`, `Min`, `Reduce`, 그리고 `Sum`

- **`[Average](http://reactivex.io/documentation/operators/average.html)`** — Observable이 발행한 항목의 평균 값을 발행한다
- **`[Concat](http://reactivex.io/documentation/operators/concat.html)`** — 두 개 이상의 Observable들이 항목을 발행할 때 Observable 순서대로 배출하는 항목들을 하나의 Observable 배출로 연이어 배출한다
- **`[Count](http://reactivex.io/documentation/operators/count.html)`** — 소스 Observable이 발행한 항목의 개수를 배출한다
- **`[Max](http://reactivex.io/documentation/operators/max.html)`** — Observable이 발행한 항목 중 값이 가장 큰 항목을 배출한다
- **`[Min](http://reactivex.io/documentation/operators/min.html)`** — Observable이 발행한 항목 중 값이 가장 작은 항목을 배출한다
- **`[Reduce](http://reactivex.io/documentation/operators/reduce.html)`** — Observable이 배출한 항목에 함수를 순서대로 적용하고 함수를 연산한 후 최종 결과를 발행한다
- **`[Sum](http://reactivex.io/documentation/operators/sum.html)`** — Observable이 배출한 항목의 합계를 배출한다

### 9. **변환 Observable**

> `To`

- `To` — Observable을 다른 객체나 자료 구조로 변환한다

### 10. **연결 가능한 Observable 연산자**

> `Connect`, `Publish`, `RefCount`, 그리고 `Replay`

- **`[Connect](http://reactivex.io/documentation/operators/connect.html)`** — 구독자가 항목 배출을 시작할 수 있도록 연결 가능한 Observable에게 명령을 내린다
- **`[Publish](http://reactivex.io/documentation/operators/publish.html)`** — 일반 Observable을 연결 가능한 Observable로 변환한다
- **`[RefCount](http://reactivex.io/documentation/operators/refcount.html)`** — 일반 Observable처럼 동작하는 연결 가능한 Observable을 만든다
- **`[Replay](http://reactivex.io/documentation/operators/replay.html)`** — 비록 옵저버가 Observable이 항목 배출을 시작한 후에 구독을 했다 하더라도 배출된 모든 항목들을 볼 수 있도록 한다

### 11. **역압(backpressure) 연산자**

> 특정 제어흐름 원칙들을 적용하는 다양한 연산자들

- `backpressure operators` — 옵저버가 소비하는 것보다 더 빠르게 항목들을 생산하는 Observable을 - 복재하는 전략

> 해당 포스팅은 '태임쓰의 개발블로그', 'reactivex.io' 블로그를 보면서 정리한 내용입니다.