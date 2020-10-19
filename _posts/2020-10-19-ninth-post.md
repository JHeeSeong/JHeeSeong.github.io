---
title: "IoC, Bean, DI란?"
date: 2020-10-19 13:05:00 -0900
categories: IoC, Bean, DI
---

## IoC(Inversion of Control)이란?

- 제어의 역전이라고 불린다.
- 이전에는 의존관계의 제어를 개발자가 직접하였다.

```java
@Service
public class AService {
    private ARepository aRepository = new ARepository();
}
```

- 제어권이 컨테이너로 넘어갔다.(제어의 역전)

```java
@Service
public class AService {
    private ARepository aRepository;

		// 컨테이너가 제어한다.(객체의 생성부터 생명주기 관리)
		@Autowired
    public AService(ARepository aRepository) {
        this.aRepository= aRepository;
    }

    @Transactional(readOnly = true)
    public List<ADto> findAllDesc() {
        return aRepository.findAllDesc().stream().map(ADto::new).collect(Collectors.toList());
    }
}
```

- 아래와 같이 @RequiredArgsConstructor를 이용하여 IoC를 사용할 수 있다.

```java
@RequiredArgsConstructor
@Service
public class AService {
    private final ARepository aRepository;

    @Transactional(readOnly = true)
    public List<ADto> findAllDesc() {
        return aRepository.findAllDesc().stream().map(ADto::new).collect(Collectors.toList());
    }
}
```

### @RequiredArgsConstructor이란?

- 초기화 되지 않은 `final` 필드나 `@NonNull`이 붙은 필드에 대해 생성자를 생성해준다.

## 빈(Bean)이란?

- 스프링 IoC(Inversion of Control) 컨테이너가 관리하는 객체
  컨테이너란? 인스턴스의 생명주기 관리, 생성된 인스턴스들에게 추가적인 기능 제공

- @SpringBootApplication, @Component, @Configuration, @Repository, @Service, @Controller  어노테이션을 가지고 있으면 빈(Bean)이다.

  ![#8%20IoC,%20Bean,%20DI%E1%84%85%E1%85%A1%E1%86%AB%201cf4aa831ac748a5823c138f8272293e/Untitled.png](#8%20IoC,%20Bean,%20DI%E1%84%85%E1%85%A1%E1%86%AB%201cf4aa831ac748a5823c138f8272293e/Untitled.png)

- 메인 클래스의 패키지를 벗어나면 빈(Bean)관리를 할 수 없다.(다른 방법 사용)

## DI(Dependency Injection)이란?

- 의존성 주입이다.
- @Autowired / @Injection을 사용한다.

### Field Injection(필드)

- Bean에 등록된 객체를 사용하고자 한다.

```java
@Service
public class AService {
		@Autowired
    private ARepository aRepository;

		// 컨테이너가 제어한다.(객체의 생성부터 생명주기 관리)
    public AService(ARepository aRepository) {
        this.aRepository= aRepository;
    }

    @Transactional(readOnly = true)
    public List<ADto> findAllDesc() {
        return aRepository.findAllDesc().stream().map(ADto::new).collect(Collectors.toList());
    }
}
```

### Setter Injection(Setter)

```java
@Service
public class AService {
    private ARepository aRepository;

  	@Autowired
    public void setARepository(ARepository aRepository){
        this.aRepository= aRepository;
    }

    @Transactional(readOnly = true)
    public List<ADto> findAllDesc() {
        return aRepository.findAllDesc().stream().map(ADto::new).collect(Collectors.toList());
    }
}
```

### Constructor Injection(생성자)

- 현재 가장 권장되고 있는 방법

```java
@Service
public class AService {
    private ARepository aRepository;

		// 컨테이너가 제어한다.(객체의 생성부터 생명주기 관리)
  	@Autowired
    public AService(ARepository aRepository) {
        this.aRepository= aRepository;
    }

    @Transactional(readOnly = true)
    public List<ADto> findAllDesc() {
        return aRepository.findAllDesc().stream().map(ADto::new).collect(Collectors.toList());
    }
}
```

- 위의 단점을 모두 극복해낸 패턴

```java
@RequiredArgsConstructor
@Service
public class AService {
    private final ARepository aRepository;

    @Transactional(readOnly = true)
    public List<ADto> findAllDesc() {
        return aRepository.findAllDesc().stream().map(ADto::new).collect(Collectors.toList());
    }
}
```

참고 : 인프런(스프링 프레임워크 입문_백기선님) 강의