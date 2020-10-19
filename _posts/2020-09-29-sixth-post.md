---
title: "JUnit 이란?"
date: 2020-09-29 09:29:00 -0600
categories: JUnit
---


Java의 단위 테스팅(Unit Testing) 도구이다.

### 특징

- 라이브러리이자 프레임워크이다.
- 단정문으로 테스트 케이스의 수행 결과를 판별한다.
- annotation으로 간결하게 지원한다.
- 결과는 녹색(성공), 노란색(실패) 중 하나로 표시된다.

```java
@Test
void test() { 
	assertTrue(calculator.plusCount(calcData)==101); //asser~:단정문
}
```

## Test code 생성하는 방법

### 1. maven project를 생성한다.

- maven project를 생성하면 test package가 생성된다.(idea로 인하여 test package가 생성됨)

### 2. maven repository에 dependency추가

```xml
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.5.1</version>
    <scope>test</scope>
</dependency>

<!-- Test코드가 성공하여야만, 빌드에 성공할 수 있다. -->
<build>
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
        </plugin>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
    </plugins>
</build>
```

### 3. ctrl + shift + t를 이용하여 Create Test를 실행한다.

| ![5. create test]({{"https://github.com/JHeeSeong/JHeeSeong.github.io/blob/gh-pages/assets/images/5_TestCodePlay.png"}}) |

### 4. 테스트 코드를 작성한다.

```java
import org.junit.jupiter.api.*;
import org.junit.jupiter.api.MethodOrderer.OrderAnnotation;
import static org.junit.jupiter.api.Assertions.*;

@TestMethodOrder(OrderAnnotation.class)
class CounterTest {
    @BeforeEach
    @DisplayName("단위 테스트 시작")
    @Order(1)
    void jUnitStart(){
        System.out.println(">> JUnit method start <<");
    }

    @Test
    @DisplayName("더하기")
    @Order(2)
    void plusCount() {
        CalcData calcData = new CalcData();
        calcData.setNum(10);

        Counter calculator = new Counter();

        assertTrue(calculator.plusCount(calcData)==101);
        assertEquals(13,calculator.plusCount(calcData));
        assertFalse(calculator.plusCount(calcData)==2);
    }

    @Test
    @DisplayName("빼기")
    @Order(3)
    void minusCount() {
        CalcData calcData = new CalcData();
        calcData.setNum(1);

        Counter calculator = new Counter();

        assertThrows(NumberFormatException.class, () -> {
            calculator.minusCount(calcData);
        });
    }
}
```

### 5. 테스트 코드를 실행한다.

| ![5. test code play]({{"https://github.com/JHeeSeong/JHeeSeong.github.io/blob/gh-pages/assets/images/5_TestCodeResult.png"}}) |

### 6. 테스트 코드 결과를 확인한다.

| ![5. test code result]({{"https://github.com/JHeeSeong/JHeeSeong.github.io/blob/gh-pages/assets/images/5_TestCodeResult.png"}}) |

### * Test코드 실패시, 빌드 Fail

| ![5. test code fail]({{"https://github.com/JHeeSeong/JHeeSeong.github.io/blob/gh-pages/assets/images/5_TestCodeFail.png"}}) |

### * Test코드 성공시, 빌드 Success

| ![5. test code succ]({{"https://github.com/JHeeSeong/JHeeSeong.github.io/blob/gh-pages/assets/images/5_TestCodeSucc.png"}}) |

## JUnit Jupiter

### JUnit4이후에 추가된 annotation

- `@TestFactory` : 동적 테스드들을 위한 테스트 팩토리인 메서드를 나타냅니다.
- `@DisplayName` : 테스트 클래스 나 테스트 메서드을 위한 커스텀 디스플레이 네임을 정의합니다.
- `@Nested` : 이 표시된 클래스는 nested, non-static 테스트 클래스이다 라는 것을 나타냅니다.
- `@Tag` : 필터링 테스트들을 위한 테그들을 정의합니다.
- `@ExtendWith` : custom extensions을 등록할때 사용됩니다.
- `@BeforeEach` : 이 표시된 메서드는 각각의 테스트 메서드 전에 실행됩니다.
- `@AfterEach` : 이 표시된 메서드는 각각의 테스트 메서드 후에 실행됩니다.
- `@BeforeAll` : 이 표시된 메서드는 현재 클래스에서 모든 메스트 메서드들 전에 실행됩니다.
- `@AfterAll` : 이 표시된 메서드는 현재 클래스에서 모든 테스트 메서드들 후에 실행됩니다.
- `@Disable` : 테스트 클래스 나 메서드 를 비활성화 할때 사용됩니다.

## 참고사항

Maven Lifecycle이란?
clean, default, site의 세가지 Lifecycle을 제공하고 있다.

### clean : 빌드 시 생성되었던 산출물을 삭제

- `clean` : 이전 빌드에서 생성된 모든 파일 삭제

### default : 프로젝트 배포 절차, 패키지 타입별로 다르게 정의

- `validate` : 프로젝트 상태 점검, 빌드에 필요한 정보 존재 유무 확인
- `compile` : 프로젝트 소스를 컴파일
- `test` : 단위 테스트 프레임워크를 이용해 테스트 수행
- `package` : 개발자가 선택한 war, jar 등의 패키징 수행
- `verify` : 패키지가 품질 기준에 적합한지 검사
- `install` : 패키지를 로컬 저장소에 설치
- `delpoy` : 패키지를 원격 저장소에 배포

### site : 프로젝트 문서화 절차

- `site` : 사이트 문서 생성
