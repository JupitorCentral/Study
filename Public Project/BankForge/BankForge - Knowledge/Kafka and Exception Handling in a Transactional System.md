- [x] Kafka and Exception Handling in a Transactional System ➕ 2025-07-27 📅 2025-07-27 ✅ 2025-09-05

### When a transactional method throws an exception, what happens to the Kafka send? Should I use a synchronous or asynchronous send?

When a transactional method annotated with Spring's `@Transactional` throws an exception, the Kafka transaction is automatically rolled back by the transaction interceptor. This ensures that if your business logic fails, the corresponding Kafka messages are not committed and remain invisible to consumers configured with `read_committed` isolation level.

#### Kafka Send Behavior in Transactions

Kafka sends within a transaction are not purely asynchronous. Messages are sent to the broker immediately but are marked as uncommitted. They are only made visible to consumers after the entire transaction successfully completes. If the transaction fails, these messages are aborted.

#### Sync vs. Async Kafka Sends

You can choose how the producer handles the send operation:

- **Asynchronous (Default)**
    
    - Uses `producer.send()` with a callback.
        
    - Offers better throughput by batching messages.
        
    - Carries a risk of data loss if the client application crashes before the message is successfully sent and acknowledged.
        
- **Synchronous**
    
    - Call `Future.get()` on the result of `producer.send()`. This blocks the thread until the send is completed and acknowledged by the broker.
        
    - Guarantees the message is acknowledged before proceeding.
        
    - Results in lower throughput but provides higher reliability.
        

#### Recommended Approach for Banking Systems

For a critical system like banking, it is recommended to:

- **Use synchronous sends** for core transaction data by calling `.get()` on the Future returned by `producer.send()`.
    
- Implement robust **exception handling** with retry logic for transient failures (e.g., network issues).
    
- Configure **dead-letter topics** to handle messages that repeatedly fail processing.
    
- Set producer configuration `acks=all` to ensure the message is replicated across brokers before being acknowledged.
    

#### Exception Handling Strategies

Configure retry behavior based on the exception type:

- **Retryable exceptions**: Network timeouts, temporary broker unavailability.
    
- **Non-retryable exceptions**: Serialization errors, authentication failures.
    
- Spring's `DefaultAfterRollbackProcessor` can be used to define custom logic for handling exceptions after a transaction rollback.
    

---

### What if the failure is not in the application logic but in the database _after_ an asynchronous Kafka send has been made?

If you use an asynchronous Kafka send and the subsequent database transaction fails, you will encounter a data consistency problem.

#### What Happens with Async Kafka + DB Failure

1. The Kafka message is sent to the broker (and potentially acknowledged) because the call is non-blocking.
    
2. The database transaction fails for some reason and is rolled back.
    
3. **Result**: A state of data inconsistency. The transaction information exists in Kafka but is absent from the database. This is a classic distributed transaction problem.
    

#### Solutions for a Kotlin Banking System

1. Synchronous Kafka Sends
    
    Wait for the Kafka acknowledgment before attempting to commit the database transaction. This ensures the message is persisted in Kafka before the state is changed in the database.
    
    Kotlin
    
    ```
    // Wait for Kafka acknowledgment before DB commit
    val future = producer.send(record)
    future.get() // Blocks until Kafka confirms
    
    // Only then proceed with DB operations
    ```
    
2. Transactional Outbox Pattern
    
    This pattern ensures atomicity between database writes and message publishing.
    
    - In a single database transaction, save both the business data (e.g., account update) and the Kafka message payload into a dedicated "outbox" table.
        
    - A separate, independent process or poller reads from this outbox table and reliably sends the messages to Kafka. This decouples the message sending from the core business transaction.
        
3. Saga Pattern
    
    This involves a sequence of local transactions. If a database operation fails after a Kafka message has been sent, you initiate a "compensating transaction" by sending a new message to Kafka to revert or correct the initial action.
    

#### Additional Topics Worth Knowing

- **Two-Phase Commit (2PC)**: A protocol for achieving atomic transactions across distributed systems, though it can be complex and impact performance.
    
- **Event Sourcing**: An architectural pattern where all changes to application state are stored as a sequence of events, avoiding dual writes.
    
- **Kafka Connect**: A tool for reliably streaming data between Apache Kafka and other systems, like databases.
    
- **Spring Cloud Sleuth**: For distributed tracing to monitor requests across multiple services.
    
- **Circuit Breaker Patterns**: To prevent cascading failures in a distributed environment.
    
- **Idempotent Consumers**: To safely handle duplicate messages that might result from retries.
    

---

### Are there any use cases where an asynchronous Kafka call would be appropriate for my banking project?

Yes, while synchronous sends are crucial for core operations, asynchronous calls have their place for less critical tasks.

#### Recommended Approach: Synchronous First

For a Kotlin banking system, **synchronous Kafka calls should be the default** for core business logic due to the strict requirements for:

- **Data Consistency**: Ensuring financial transactions are not lost.
    
- **Regulatory Compliance**: Maintaining accurate and complete audit trails.
    
- **Error Handling**: Detecting and handling failures immediately.
    

#### Limited Use Cases for Async Kafka

Asynchronous Kafka calls are beneficial in scenarios where immediate consistency is not paramount and high throughput is desired:

- **Non-Critical Events**
    
    - User login/logout history.
        
    - Application metrics and performance data collection.
        
    - System health monitoring pings.
        
- **High-Volume, Non-Transactional Data**
    
    - User behavior analytics (e.g., clicks, page views).
        
    - Real-time performance metric streams.
        
    - Queuing non-essential notifications.
        
- **Background Processing Triggers**
    
    - Initiating report generation.
        
    - Sending notifications for batch job completion.
        
    - Broadcasting cache invalidation events.
        

#### Implementation Strategy

Adopt a hybrid approach: use **synchronous** sends for all core banking operations like transfers, deposits, and withdrawals. Reserve **asynchronous** sends for auxiliary systems that support monitoring, analytics, and background tasks where eventual consistency is acceptable.

---

### Summary

**Core Transactional Behavior**

- When a Spring `@Transactional` method fails, an associated Kafka transaction is automatically rolled back, preventing uncommitted messages from being read.
    

**Synchronous vs. Asynchronous Sends**

- **Synchronous**: Use `Future.get()` to block until the broker acknowledges the message. This provides high reliability and is essential for critical data, though it lowers throughput.
    
- **Asynchronous**: The default behavior. It offers high throughput but risks data inconsistency if a subsequent database commit fails. The message might be in Kafka while the database state is rolled back.
    

**Handling Distributed State (DB + Kafka)**

- **Problem**: An async Kafka send followed by a DB failure leads to inconsistent data.
    
- **Solutions**:
    
    - **Synchronous Sends**: Ensure Kafka has the message before committing to the DB.
        
    - **Transactional Outbox Pattern**: Atomically write business data and the message to an "outbox" table in the same DB transaction. A separate poller sends messages from this table to Kafka.
        
    - **Saga Pattern**: Use compensating transactions to revert actions if a step in a distributed process fails.
        

**Recommended Strategy for Banking Systems**

- **Synchronous for Core Operations**: All financial transactions (transfers, deposits) must use synchronous sends to guarantee data integrity.
    
- **Asynchronous for Non-Critical Data**: Use for auxiliary tasks like analytics, logging, system metrics, and triggering background jobs where eventual consistency is acceptable.
    

**Key Configurations & Patterns**

- **`acks=all`**: Ensure the producer only receives an acknowledgment after the message is fully replicated.
    
- **Idempotent Consumers**: Design consumers to handle duplicate messages without causing incorrect state changes.
    
- **Dead Letter Queues (DLQ)**: Create a separate topic to route messages that repeatedly fail processing for later analysis.
    
- **Retry Logic**: Implement retries for transient errors like network failures.