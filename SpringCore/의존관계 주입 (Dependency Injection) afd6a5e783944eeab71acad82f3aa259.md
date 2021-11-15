# 의존관계 주입 (Dependency Injection)

최종 편집 일시: 2021년 11월 14일 오후 10:57
태그: @Autowired, @Primary, @Qualifire, DI, Dependency Injection

# Dependecy Injection

1. 생성자 주입
2. 수정자 주입 (Setter)
3. 필드 주입
4. 일반 메서드 주입

### 생성자 주입

- 파라미터로는 불변, 필수 값만 넣는다.
- 생성자가 단 하나일 때는 `@Autowired` 를 생략할 수 있다.
- 빈 생성과 연관관계 주입이 동시에 일어난다.

### 수정자(setter) 주입

- setter라고 불리는 수정자 메서드를 통해 주입하는 방식
- 선택, 변경 가능성이 있는 의존관계에 사용

### 필드 주입

- 필드에 직접 주입하는 방법이다
    - 외부 변경이 불가능하여 테스트가 힘들다는 치명적 단점이 존재한다.
    - 스프링 설정을 목적으로 하는 `@Configuration` 과 같은 특수한 용도로만 사용
    - 테스트 용도를 제외하고는 사용하지말자.

### 일반 메서드 주입

- 일반 메서드를 통해 의존관계를 주입하는 것을 의미
    - 일반적으로 잘 사용하지 않음

- 스프링은 빈 생성  →  연관관계 생성 을 나누어 실행한다.

## 옵션 처리

- 주입할 빈이 존재하지 않았도 동작해야 할 때가 있는데 `@Autowired` 만 사용하면 필수 값이 되어버려 오류가 발생한다.
- 자동 주입 대상을 옵션처리 하는 방법
    - `@Autowired(required=false)`  : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출이 안됨
    - `org.springframwork.lang.@Nullabe` : 자동 주입할 대상이 없으면 null 입력
    - `Optional<>` : 자동 주입 대상이 없으면 `Optional.empty` 가 입력된다.
    

## 조회할 빈이 2개 이상일 때.

1. `@Autowired` 필드 명 매칭
2. `@Qualifier` → `@Qualifier` 끼리 매칭 → 빈이름 매칭
3. `@Primary` 사용

### 1. `@Autowired` 필드 명 매칭

- `@Autowired` 는 타입 매칭을 먼저시도하고, 이때 다수의 빈이 있으면 필드 명, 파라미터 명으로 빈 이름을 추가 매칭한다

```java

// 필드 주입 
@Autowired
private DiscountPolicy rateDiscountPolicy;

// 생성자 방식
@Autowired
public Constructor(DiscountPolicy rateDiscountPolicy)
{ this discountPolicy = rateDiscountPolicy;}
```

### 2. `@Qualifier` 사용

- 추가 구분자를 붙여주는 방법, 주입시 추가 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.

```java
// 추가 구분자 지정
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy{
}

public Constructor(@Qualifier("mainDiscountPolicy") DiscountPolicy rateDiscountPolicy)
{ this discountPolicy = rateDiscountPolicy;}

```

### 3. `@Primary` 우선순위 지정하는 법

```java
@Component
public class RateDiscountPolicy implements DiscountPolicy{
}

@Component
@Primary
public class FixDiscountPolicy implements DiscountPolicy{
}
```

- `@Primary` 가 붙은 요소를 우선적으로 가져온다.
    
    

## 조회한 빈이 전부 필요할 때 List, Map

```java
@Autowired
public Constructor(Map<String,DiscountPolicy> policyMap, List<DiscountPolicy> policyList)
{ 
	this.policyMap = policyMap;
	this.policyList = policyList;
}
```