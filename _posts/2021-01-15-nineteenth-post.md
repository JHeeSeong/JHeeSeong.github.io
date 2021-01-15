title: "List의 객체 목록 중복 제거"
date: 2021-01-15 18:40:00 -0900
categories: JAVA, List, 목록 중복 제거
---

### 중복 제거 코드
```java

public static void main(String[] args) {
    List<Deduplication> overLapList = new ArrayList<>();
    overLapList.add(new Deduplication(1, "dupli1"));
    overLapList.add(new Deduplication(2, "dupli2"));
    overLapList.add(new Deduplication(3, "dupli3"));
    overLapList.add(new Deduplication(1, "dupli1"));

    System.out.println("======= Deduplication Before =======");
    for (Deduplication deduplication : overLapList){
        System.out.println(String.format("Sequence : %d, Name : %s", deduplication.getSeq(), deduplication.getName()));
    }

    List<Deduplication> dupliationList = new ArrayList<>();
    dupliationList.addAll(overLapList.stream().collect(Collectors.toConcurrentMap(Deduplication::getSeq, Function.identity(), (p, q) -> p)).values());

    System.out.println("======= Deduplication After =======");
    for (Deduplication deduplication : dupliationList){
        System.out.println(String.format("Sequence : %d, Name : %s", deduplication.getSeq(), deduplication.getName()));
    }
}
```

### 결과

```java
======= Deduplication Before =======
Sequence : 1, Name : dupli1
Sequence : 2, Name : dupli2
Sequence : 3, Name : dupli3
Sequence : 1, Name : dupli1
======= Deduplication After =======
Sequence : 1, Name : dupli1
Sequence : 2, Name : dupli2
Sequence : 3, Name : dupli3
```

- List의 타입 : Dulpication 정의

```
public class Deduplication {
    private long seq;
    private String name;

    public Deduplication(long seq, String name) {
        this.seq = seq;
        this.name = name;
    }

    public long getSeq() {
        return seq;
    }

    public void setSeq(long seq) {
        this.seq = seq;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
