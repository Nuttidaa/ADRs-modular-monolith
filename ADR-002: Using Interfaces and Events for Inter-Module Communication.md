## Using Interfaces and Events for Inter-Module Communication
## Context
In our Modular Monolith architecture (ADR-001), modules need to communicate with each other while maintaining loose coupling. For example:
- Order module needs to check inventory availability
- Payment module needs to notify Order module of payment status
- User module needs to validate user credentials for orders

We need a communication strategy that:
- Prevents tight coupling between modules
- Maintains clear module boundaries
- Allows for future extraction to microservices
- Supports both synchronous and asynchronous interactions

Options considered:
1. Direct method calls: Simple but creates tight coupling
2. Shared database: Easy but violates encapsulation
3. Interface-based contracts: Provides abstraction layer
4. Event-driven communication: Enables loose coupling for async operations

## Decision
We will implement a hybrid approach using:

1. Synchronous Communication: Interface-Based Contracts
   - For immediate responses (queries and commands that need instant feedback)
   - Modules expose public interfaces only
   - Use dependency injection for implementations

2. Asynchronous Communication: Domain Events
   - For operations that don't need immediate response
   - Events for notifications and state changes
   - Event handlers process events within interested modules

Communication Rules:
- Modules expose public interfaces only
- No direct access to other module's internal classes
- Use dependency injection for interface implementations
- Events for notifications and async operations
- No shared database tables between modules
- API contracts versioned and documented

## Consequences

### Positive
- Loose coupling: Modules depend on abstractions, not implementations
- Testability: Easy to mock interfaces for unit testing
- Flexibility: Can swap implementations without affecting consumers
- Clear contracts: Public APIs are explicit and documented
- Async support: Events enable non-blocking operations
- Migration ready: Interfaces can become REST/gRPC calls when extracting to microservices
- Event sourcing option: Event-based communication supports audit trails

### Negative
- Complexity: More code than direct method calls
- Learning curve: Team needs to understand event-driven patterns
- Debugging difficulty: Event flows harder to trace than direct calls
- Performance overhead: Slight latency from event bus (though minimal in-process)
- Eventual consistency: Async events may create temporary inconsistencies

### Neutral
- Framework dependency: Need event bus library (e.g., MediatR, Spring Events)
- Documentation overhead: Must maintain interface contracts and event schemas
- Governance required: Need architectural guidelines to prevent misuse
