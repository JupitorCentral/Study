- [ ] Understanding the `@Transactional` Annotation in Spring (Kotlin) ➕ 2025-07-27 📅 2025-07-27 

### What does the `@Transactional` annotation do, especially concerning exceptions and rollbacks?

Your understanding is correct—the `@Transactional` annotation automatically handles database rollbacks when exceptions occur. Here's a more detailed explanation for your banking project.

#### How @Transactional Works

When you annotate a method with `@Transactional`, Spring creates a proxy that wraps your method with transaction management.


```Kotlin
@Transactional
fun transferMoney(fromAccount: Long, toAccount: Long, amount: BigDecimal) {
    // All database operations here run in a single transaction
    accountService.debit(fromAccount, amount)
    accountService.credit(toAccount, amount)
    // If any exception occurs, everything gets rolled back
}
```

Behind the scenes, this translates to:

1. `connection.setAutoCommit(false)` - Start transaction
    
2. Execute your code
    
3. `connection.commit()` - If successful
    
4. `connection.rollback()` - If an exception occurs
    

#### Exception Handling and Rollback Rules

##### Default Behavior

- **Unchecked exceptions** (`RuntimeException`, `Error`): Automatic rollback
    
- **Checked exceptions**: No rollback by default
    

##### Custom Rollback Configuration

You can customize this behavior.

```Kotlin
@Transactional(rollbackFor = [SQLException::class, Exception::class])
fun riskyOperation() {
    // Will rollback for any exception, including checked ones
}
```

#### Kotlin-Specific Considerations

##### Class Design

Kotlin classes and their methods are `final` by default. For Spring's proxying mechanism to work, they need to be declared as `open`.

```Kotlin
@Service
open class BankService {  // Must be 'open'

    @Transactional
    open fun processTransaction() {  // Must be 'open'
        // Transaction logic
    }
}
```

##### Coroutines Limitation

`@Transactional` does not work with Kotlin coroutines by default. For asynchronous operations, you will need to use reactive transaction management.

#### Key Configuration Options

|Attribute|Purpose|Example|
|---|---|---|
|`rollbackFor`|Specify exceptions that trigger rollback|`rollbackFor = [Exception::class]`|
|`readOnly`|Mark as a read-only transaction|`readOnly = true`|
|`propagation`|Define transaction propagation behavior|`propagation = REQUIRES_NEW`|
|`isolation`|Set isolation level|`isolation = READ_COMMITTED`|

#### For Your Banking Project

Given the critical nature of financial transactions, consider the following:

- Use `rollbackFor = [Exception::class]` for comprehensive error handling.
    
- Apply `@Transactional` at the service layer for methods involving multiple database operations.
    
- Ensure all service classes and transactional methods are `open` for proper proxy creation.
    
- Test rollback scenarios thoroughly.
    

Additional topics to explore include transaction propagation levels, isolation levels for concurrent banking operations, and integration with event sourcing patterns.

---

### Summary

**Core Functionality**

- Wraps a method in a database transaction.
    
- Automatically commits on successful completion and rolls back on specific exceptions.
    

**Rollback Behavior**

- **Default**: Rolls back only for unchecked exceptions (`RuntimeException`, `Error`).
    
- **Custom**: Use `rollbackFor = [Exception::class]` to force rollbacks for all exceptions, including checked ones.
    

**Kotlin Considerations**

- **`open` Keyword**: Classes and methods annotated with `@Transactional` must be `open` to allow Spring to create proxies.
    
- **Coroutines**: `@Transactional` is not compatible with standard Kotlin coroutines; reactive transaction management is required for async operations.
    

**Key Configuration Attributes**

- `rollbackFor`: Specifies which exceptions trigger a rollback.
    
- `readOnly`: Optimizes the transaction for read-only operations.
    
- `propagation`: Defines how the transaction relates to an existing transaction.
    
- `isolation`: Controls the visibility of changes made by one transaction to others.