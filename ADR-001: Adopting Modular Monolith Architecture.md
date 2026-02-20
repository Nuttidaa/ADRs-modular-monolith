## Adopting Modular Monolith Architecture
## Context
The system we are developing is expected to grow over time, but the current team size is small and the system complexity is still moderate.
Using a traditional monolith may lead to tightly coupled code and poor maintainability. On the other hand, using microservices would introduce unnecessary complexity such as distributed communication, deployment overhead, and infrastructure management.
We need an architecture that keeps deployment simple while maintaining clear structure and separation of concerns.

## Decision
We decided to adopt the Modular Monolith architecture.
The system will be deployed as a single application, but internally divided into well-defined modules based on business domains.

## Rational
1. Keeps deployment simple (single deployable unit)
2. Avoids network overhead and distributed system complexity
3. Provides clear module boundaries
4. Improves maintainability compared to traditional monolith
5. Allows future migration to microservices if needed

## Consequences
Pros
1. Easier deployment
2. Better code organization
3. Clear separation of responsibilities
4. Lower infrastructure complexity

Cons
1. Entire system must be redeployed for changes
2. Scaling is limited compared to microservices
3. Risk of becoming a “big ball of mud” if boundaries are not enforced

## Sample Code
Project structure example:
``` python
src/
 ├── user/
 │    ├── User.java
 │    └── UserService.java
 │
 ├── order/
 │    ├── Order.java
 │    └── OrderService.java
 │
 ├── payment/
 │    ├── PaymentService.java
 │
 └── MainApplication.java
 
 The application runs as a single deployable unit.
```
The application runs as a single deployable unit.
