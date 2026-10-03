2026-10-03 14:48

Tags: [[software architecture]]

# Anemic vs Rich Domain Models
- An **anemic domain model** holds domain data while services implement the business rules that operate on it.
- A **rich domain model** combines domain state with behaviour that enforces its rules and valid state transitions.
- The distinction concerns where business behaviour lives, rather than the presence of controller, service, and repository layers.

## Responsibility Comparison

| Concern | Anemic model | Rich model |
| --- | --- | --- |
| Domain objects | Primarily hold data | Hold data and related business behaviour |
| Business rules | Implemented in services | Encapsulated in domain objects or appropriate domain services |
| Application services | Perform business calculations and coordinate infrastructure | Coordinate use cases, persistence, and integrations |

- Data-only request objects, response objects, and persistence records can be appropriate in either design.
- Their presence alone does not establish whether the domain model is anemic.

## Example: Virtual Wallet
- With an anemic model, a service reads the balance, checks the amount, calculates the new balance, and updates storage.
- With a rich model, the wallet owns its debit and credit rules and exposes operations instead of unrestricted balance setters, applying [[Encapsulation]].
- In this simplified example, amounts must be positive and the balance cannot become negative:

```java
import java.math.BigDecimal;

class VirtualWallet {
  private BigDecimal balance;

  VirtualWallet(BigDecimal openingBalance) {
    if (openingBalance == null || openingBalance.signum() < 0) {
      throw new IllegalArgumentException("Invalid opening balance");
    }
    balance = openingBalance;
  }

  BigDecimal balance() {
    return balance;
  }

  void debit(BigDecimal amount) {
    requirePositive(amount);
    if (balance.compareTo(amount) < 0) {
      throw new IllegalStateException("Insufficient balance");
    }
    balance = balance.subtract(amount);
  }

  void credit(BigDecimal amount) {
    requirePositive(amount);
    balance = balance.add(amount);
  }

  private static void requirePositive(BigDecimal amount) {
    if (amount == null || amount.signum() <= 0) {
      throw new IllegalArgumentException("Amount must be positive");
    }
  }
}
```

- Grouping balance state with its transition rules supports high [[Cohesion and Coupling|cohesion]].
- The wallet's checks protect the in-memory object. Persistence transactions and concurrency control remain separate responsibilities.

## Application Service Responsibilities
- Load the wallet and arrange any required conversion from persistence records.
- Invoke domain operations, such as `wallet.debit(amount)`.
- Persist the resulting state and coordinate transaction records within the required transaction boundary.
- Coordinate external integrations and operations involving multiple domain objects.
- Keep shared business rules in the domain rather than repeating them across use-case services. Rules spanning several objects may belong in a domain service.

## Choosing a Model
- A simple CRUD application with few business rules may be easier to maintain with a straightforward service-oriented design.
- Complex rules and state transitions can benefit from rich objects that keep invariants and behaviour together.
- Scattered rule implementations can make an anemic model harder to change consistently.
- Rich models require useful boundaries and explicit domain operations; adding methods to every data class does not automatically improve the design.
- Choose according to domain complexity and change patterns. Neither microservices nor a particular web framework determines the choice.

# References
[[2 - Source Materials/Course/设计模式之美/6 - Anemic Domain Model vs Rich Domain Model|6 - Anemic Domain Model vs Rich Domain Model]]
