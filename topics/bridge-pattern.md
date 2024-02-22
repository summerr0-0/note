# bridge-pattern

* 구현 부분과 추상화된 부분을 분리하여, 각각 독립적으로 변형이 가능하도록 만드는 디자인 패턴 
* 추상화와 구현을 분리하여 서로 독립적으로 확장할 수 있게 도와줍니다. 
* 브릿지는 두 구성 요소 간의 '가교' 역할을 함으로써 둘 사이의 결합도를 낮추고 유연성을 향상시킨다

### 구성
추상화 계층 
* Abstraction :추상적인 인터페이스 제공
  * RefinedAbstraction :추상화를 확장하여 새로운 기능 추가
구현 계층
* Implementor :구현에 대한 인터페이스 정의 
  * ConcreteImplementorA : Implementor 인터페이스의 구현
  * ConcreteImplementorB :Implementor 인터페이스의 또 다른 구현
![image_2.png](image_2.png)
### 대표적 예시
* `PlatformTransactionManager` 에서 브릿지 패턴을 사용하고 있다
  * `JdbcTransactionManager`, `JpaTransactionManager` 를 구현체로 사용한다

### 결제
```Java
// 추상화 계층
public interface PaymentProcessor {
    void processPayment(PaymentData data);
}

//추상화 계층 구현
public class PaymentProcessorImpl implements PaymentProcessor {
    private PaymentMethod paymentMethod;

    public PaymentProcessorImpl(PaymentMethod paymentMethod) {
        this.paymentMethod = paymentMethod;
    }

    @Override
    public void processPayment(PaymentData data) {
        paymentMethod.pay(data);
    }
}
    
// 구현 계층 인터페이스
public interface PaymentMethod {
    void pay(PaymentData data);
}

// 신용카드 구현
public class CreditCardPaymentMethod implements PaymentMethod {
    @Override
    public void pay(PaymentData data) {
        // 신용카드 결제 처리 로직
    }
}

// 토스 구현
public class TossPaymentMethod implements PaymentMethod {
    @Override
    public void pay(PaymentData data) {
        // 토스 결제 처리 로직
    }
}

// 결제
public class CheckoutService {
    public void checkout(Cart cart, PaymentData paymentData) {
      //결제수단을 자유롭게 바꿀 수 있음
        PaymentMethod paymentMethod = getPaymentMethodFor(paymentData);
        //새로운 결제방식이 추가되어도 paymentProcessor 수정할 필요 없음
        PaymentProcessor paymentProcessor = new PaymentProcessorImpl(paymentMethod);
        paymentProcessor.processPayment(paymentData);
        
    }
}

```