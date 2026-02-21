## Use Domain-Based Module Structure
## Context
Although we are using a Modular Monolith architecture, poor internal organization could still lead to tight coupling and unclear responsibilities.
To maintain modularity inside a single application, we need a clear strategy for structuring the codebase.

## Decision
We decided to structure the system based on business domains.
Each domain will contain its own models, services, and logic.
Modules will communicate through defined interfaces or service calls only.

## Rational
1. Aligns with Domain-Driven Design (DDD) principles
2. Improves cohesion within modules
3. Reduces coupling between modules
4. Makes the system easier to maintain and test
5. Supports future separation into microservices

## Consequences
Pros
1. Clear ownership per module
2. Easier team collaboration
3. Improved maintainability
4. Better scalability in the future

Cons
1. Requires discipline to maintain boundaries
2. Developers must understand domain separation clearly
3. Risk of accidental cross-module dependency

## Sample code
### orderService.java
``` python
// order/OrderService.java
import payment.PaymentGateway;

public class OrderService {

    private PaymentGateway paymentGateway;

    public OrderService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public void createOrder(double amount) {
        paymentGateway.pay(amount);
    }
}
```
### paymentGateway.java
``` python
// payment/PaymentGateway.java
public interface PaymentGateway {
    void pay(double amount);
}
```
### paymentService.java
``` python
// payment/PaymentService.java
public class PaymentService implements PaymentGateway {
    @Override
    public void pay(double amount) {
        System.out.println("Paid: " + amount);
    }
}
```
The Order function does not directly access the Payment database.
