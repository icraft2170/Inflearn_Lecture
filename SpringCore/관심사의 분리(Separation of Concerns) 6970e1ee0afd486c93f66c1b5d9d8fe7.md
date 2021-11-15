# 관심사의 분리(Separation of Concerns)

최종 편집 일시: 2021년 11월 13일 오후 7:14
태그: SoC, separation of concerns, 관심사의분리

# 관심사의 분리 (SoC : Separation of Concerns)

---

- 좋은 객체지향설계의 원칙중 **단일 책임 원칙(SRP)** 에 의하면 하나의 역할은 하나의 책임만을 가져야 한다. 이 때 너무 많은 책임을 가진 역할 속에서 그런 책임(관심사)들을 분리하는 것을 관심사의 분리 라고 한다.
- 스프링 컨테이너에서 각 Object의 설정(의존결정) 관심사(책임)를 스프링 프레임워크에게 맡기는(분리)하는 것 또한 관심사의 분리라고 할 수 있다.

### 역할의 구현체 결정 책임을 분리한 AppConfig

```java
//Plain Old Java Object
public class AppConfig{
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }

    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }

    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }

    public OrderService orderService(){
        return new OrderServiceImpl(memberRepository(),discountPolicy());
    }
}
/**
* 팩토리 메소드로 구성된 설정파일을 통해 인터페이스의 구현체를 생성하는 책임을 분리했다.
*/
```