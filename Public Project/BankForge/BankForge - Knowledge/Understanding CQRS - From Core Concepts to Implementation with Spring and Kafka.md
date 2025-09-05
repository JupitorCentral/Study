- [x] Understanding CQRS - From Core Concepts to Implementation with Spring and Kafka ➕ 2025-07-27 📅 2025-07-27 ✅ 2025-09-05

### I can't understand what CQRS is. Could you explain it compared to a RESTful API with an MVC pattern?

CQRS (Command Query Responsibility Segregation) is an architectural pattern that separates read and write operations into different models, whereas a RESTful API with MVC typically handles both operations through the same unified model.

#### Key Differences

**Traditional MVC + REST**

- A single model handles both read and write operations.
    
- The same database is used for all operations.
    
- Controllers handle both GET (queries) and POST/PUT/DELETE (commands).
    
- It follows a simple, straightforward approach.
    

**CQRS**

- **Commands** (writes) use one model, potentially with its own database.
    
- **Queries** (reads) use a separate, optimized model, often with a dedicated read database.
    
- Command handlers process writes.
    
- Query handlers process reads.
    
- It often uses event sourcing for data synchronization between the write and read sides.
    

#### Practical Example in Banking (Kotlin)

**MVC + REST approach**

```kotlin
@RestController
class AccountController {
    @GetMapping("/accounts/{id}")
    fun getAccount(@PathVariable id: String): Account {
        return accountService.findById(id) // Same model for read
    }
        
    @PostMapping("/accounts/{id}/transfer")
    fun transfer(@PathVariable id: String, @RequestBody request: TransferRequest) {
        accountService.transfer(request) // Same model for write
    }
}
```

**CQRS approach**

```kotlin
// Command side
@Component
class TransferCommandHandler {
    fun handle(command: TransferCommand) {
        // Write to command database
        // Publish event
    }
}

// Query side
@RestController
class AccountQueryController {
    @GetMapping("/accounts/{id}")
    fun getAccount(@PathVariable id: String): AccountView {
        return accountQueryService.getAccountView(id) // Optimized read model
    }
}
```

#### When to Use Each

**Use MVC + REST when:**

- The application involves simple CRUD operations.
    
- You are building small to medium-sized applications.
    
- The team prefers simplicity.
    
- Performance requirements are moderate.
    

**Use CQRS when:**

- The business logic is complex.
    
- High performance for both reads and writes is required.
    
- Reads and writes have different optimization needs.
    
- There is a need for audit trails or a history of events.
    

---

### Is capturing an event the key to CQRS?

Events are **central** to CQRS, but the key is the **event flow** that synchronizes the command and query sides, not just capturing the event itself.

#### Event Role in CQRS

The flow is: **Command Side → Events → Query Side**

1. **Commands** generate **domain events** after a successful write operation.
    
2. **Events** carry the information about "what happened."
    
3. The **Query side** consumes these events to update its separate read models.
    

#### Kotlin Example

```kotlin
// Command side generates events
class TransferCommandHandler {
    fun handle(command: TransferCommand) {
        // Business logic
        val event = MoneyTransferredEvent(
            fromAccount = command.fromAccount,
            toAccount = command.toAccount,
            amount = command.amount
        )
        eventPublisher.publish(event)
    }
}

// Query side consumes events
@EventHandler
class AccountViewUpdater {
    fun on(event: MoneyTransferredEvent) {
        // Update read model
        accountViewRepository.updateBalance(event.fromAccount, -event.amount)
        accountViewRepository.updateBalance(event.toAccount, +event.amount)
    }
}
```

#### Key Points

- Events **synchronize** the separated command and query models.
    
- Without events, the two sides would become inconsistent.
    
- Events enable **eventual consistency** between the write and read models.
    
- **Event Sourcing** (storing events as the source of truth) is an optional but common pattern used with CQRS.
    

---

### Is the flow like this: user request -> command event -> event sourcing -> query event?

Not quite. The flow is more specific. The term isn't "command event," and Event Sourcing is an optional part of the process.

#### Correct CQRS Event Flow

The correct sequence is: **User Request → Command → Domain Event → Query Model Update**

1. **User makes a request**: `POST /transfer { fromAccount: "A", toAccount: "B", amount: 100 }`
    
2. **Command processing**: The request is translated into a `TransferCommand` which is handled by a `TransferCommandHandler`.
    
3. **Domain event generated**: After the command succeeds, a **domain event** like `MoneyTransferredEvent` is created.
    
4. **Query side consumes event**: A listener like `AccountViewUpdater` consumes the `MoneyTransferredEvent` and updates the read model.
    

#### Key Corrections

- It's a **domain event**, not a "command event," that is generated _after_ a command is successfully processed.
    
- **Event Sourcing** is optional. You can implement CQRS without storing the full history of events as your source of truth.
    
- The **Query side does not generate events**; it only consumes them to update its data models.
    

#### Event Sourcing vs. CQRS

- **CQRS**: A pattern that separates read and write models. It often uses events for synchronization.
    
- **Event Sourcing**: An implementation strategy where the state of the application is determined by a sequence of events. It can be used with or without CQRS.
    

---

### Is the command side a producer and the query side a consumer, like an event queue in an OS?

Yes, that's an excellent analogy. The command side acts as a **Producer**, and the query side acts as a **Consumer**.

#### CQRS Producer-Consumer Pattern

- **Command Side (Producer)**:
    
    - Processes business commands (e.g., `TransferMoney`).
        
    - Generates domain events after a successful operation.
        
    - Publishes these events to an event stream or queue.
        
- **Query Side (Consumer)**:
    
    - Subscribes to events published by the command side.
        
    - Updates its optimized read models (projections) based on the consumed events.
        
    - Serves these optimized models to users for fast queries.
        

#### Kotlin Example

```kotlin
// Producer (Command Side)
class TransferCommandHandler {
    fun handle(command: TransferCommand) {
        // Business logic
        val event = MoneyTransferredEvent(...)
        eventPublisher.publish(event) // Produces event
    }
}

// Consumer (Query Side)
@EventHandler
class AccountViewUpdater {
    fun on(event: MoneyTransferredEvent) { // Consumes event
        accountViewRepository.updateBalance(...)
    }
}
```

#### Key Differences from OS Event Queues

- **Domain-focused events** vs. system-level events.
    
- **Business context is preserved** in events, unlike low-level OS operations.
    
- The model relies on **eventual consistency** between models, not necessarily immediate processing.
    

---

### The example code for transferring money looks like it's implemented using a Spring event capture handler. Is that right?

Yes, you're exactly right. Spring's built-in event mechanism, particularly `@EventListener`, is a perfect fit for implementing the event handling part of a CQRS architecture.

#### Spring Event Handler for Money Transfer

**Command Side - Publishes events**

```kotlin
@Service
class TransferCommandHandler(
    private val applicationEventPublisher: ApplicationEventPublisher,
    private val accountRepository: AccountRepository
) {
    @Transactional
    fun handle(command: TransferCommand) {
        // Business logic
        val fromAccount = accountRepository.findById(command.fromAccountId)
        val toAccount = accountRepository.findById(command.toAccountId)
                
        fromAccount.withdraw(command.amount)
        toAccount.deposit(command.amount)
                
        // Publish domain event after successful transaction commit
        val event = MoneyTransferredEvent(
            transferId = command.transferId,
            fromAccountId = command.fromAccountId,
            toAccountId = command.toAccountId,
            amount = command.amount,
            occurredAt = Instant.now()
        )
        applicationEventPublisher.publishEvent(event)
    }
}
```

**Query Side - Consumes events**

```kotlin
@Component
class AccountViewProjector(
    private val accountViewRepository: AccountViewRepository
) {
    @EventListener
    @Async // Run this logic in a separate thread
    fun on(event: MoneyTransferredEvent) {
        // Update read models (e.g., account balance view)
        accountViewRepository.updateBalance(
            event.fromAccountId, 
            -event.amount
        )
        accountViewRepository.updateBalance(
            event.toAccountId, 
            +event.amount
        )
                
        // Update another read model (e.g., transaction history view)
        transactionViewRepository.save(
            TransactionView(
                id = event.transferId,
                fromAccount = event.fromAccountId,
                toAccount = event.toAccountId,
                amount = event.amount,
                timestamp = event.occurredAt
            )
        )
    }
}
```

**Domain Event**

```kotlin
data class MoneyTransferredEvent(
    val transferId: String,
    val fromAccountId: String,
    val toAccountId: String,
    val amount: BigDecimal,
    val occurredAt: Instant
)
```

#### Key Spring Features Used

- `@EventListener`: Captures published domain events.
    
- `ApplicationEventPublisher`: Injects a publisher to send events.
    
- `@Async`: Makes the event consumer (query side) asynchronous, decoupling it from the command side.
    
- `@Transactional`: Ensures the command's database operations are atomic.
    

---

### So, with this pattern, subsequent code can't be used like a simple RESTful CRUD because it's not synchronous?

Exactly! You've identified the key trade-off and behavioral difference between CQRS and traditional RESTful CRUD. The flow is fundamentally asynchronous.

#### CQRS vs. RESTful CRUD Comparison

**Traditional RESTful CRUD (Synchronous)**

The client sends a request and waits for the entire operation, including database updates, to complete before getting a final response.

```kotlin
@PostMapping("/transfer")
fun transfer(@RequestBody request: TransferRequest): TransferResponse {
    // Everything is synchronous and waits for completion
    val result = accountService.transfer(request)
    // Returns final state after completion
    return TransferResponse(result.balance, "Transfer completed")
}
```

**CQRS Pattern (Asynchronous)**

The client sends a command and gets an immediate acknowledgment that the request has been accepted. The actual processing happens in the background.

```kotlin
@PostMapping("/transfer") 
fun transfer(@RequestBody request: TransferRequest): ResponseEntity<String> {
    transferCommandHandler.handle(command)
    // User doesn't wait for actual completion
    return ResponseEntity.accepted().body("Request accepted for processing")
}
```

#### Key Differences

|Feature|RESTful CRUD|CQRS|
|---|---|---|
|**Execution**|**Synchronous**|**Asynchronous**|
|**Consistency**|**Immediate**|**Eventual**|
|**Flow**|Simple: Request → Process → Response with final state|Complex: Request → Command → Event → Query Update|
|**Threading**|**Blocking**: HTTP thread waits for DB actions|**Non-blocking**: HTTP thread is freed immediately|

#### When Each Makes Sense

- **Use RESTful CRUD** when users need immediate confirmation, business logic is simple, and strong consistency is a requirement.
    
- **Use CQRS** when high throughput is needed, business operations are complex, and the system can tolerate eventual consistency.
    

---

### How does the server tell the user that the transaction is completed?

Since the initial response is just an acknowledgment, you need a separate mechanism to notify the user of the final outcome. There are several common strategies.

#### Strategies for Notifying Users of Transaction Completion

1. **Client Polling**
    
    - **How it works**: After receiving a `202 Accepted` response, the client periodically sends requests to a status endpoint (e.g., `GET /api/transactions/{id}/status`).
        
    - **Pros**: Simple to implement on both client and server.
        
    - **Cons**: Creates extra server load and has latency based on the polling interval.
        
2. **Long Polling (Comet)**
    
    - **How it works**: The client makes a request that the server keeps open until the transaction completes or a timeout is reached. The server responds immediately once the processing is done.
        
    - **Pros**: Near real-time updates without the overhead of WebSockets.
        
    - **Cons**: Ties up server connections and is more complex than simple polling.
        
3. **WebSocket / STOMP Notifications**
    
    - **How it works**: The client establishes a persistent WebSocket connection and subscribes to a specific topic (e.g., `/topic/transactions/{transactionId}`). When the query side projector finishes its update, it sends a message to that topic.
        
    - **Pros**: True real-time, bidirectional communication. Highly efficient.
        
    - **Cons**: Requires WebSocket infrastructure and is more complex to set up.
        
    - **Example**: `simpMessagingTemplate.convertAndSend("/topic/transactions/${event.id}", status)`
        
4. **Server-Sent Events (SSE)**
    
    - **How it works**: The client opens a standard HTTP connection to an SSE endpoint that streams updates from the server.
        
    - **Pros**: Simpler than WebSockets and uses standard HTTP. Has built-in reconnection logic.
        
    - **Cons**: Communication is unidirectional (server to client).
        
5. **Push Notifications**
    
    - **How it works**: For mobile or Progressive Web Apps (PWAs), the server can trigger a push notification via a service like Firebase Cloud Messaging (FCM) once the process is complete.
        
    - **Pros**: Reaches users even when they are not actively using the app.
        
    - **Cons**: Relies on external services and adds setup overhead.
        
6. **Email / SMS Alerts**
    
    - **How it works**: A dedicated service listens for completion events and sends an email or SMS to the user.
        
    - **Pros**: Reliable and works even if the user is offline.
        
    - **Cons**: Not real-time and may have associated costs.
        

---

### Now I think I understand Kafka's role. Does it store events from the command side in queues for the query side to process, and manage the status of these events?

Your understanding is correct. In a distributed CQRS system, **Kafka acts as the durable and scalable event backbone** between the command and query sides.

- The **command side** processes business logic and **publishes** events to Kafka topics.
    
- Kafka **durably stores** these events in an ordered, append-only log.
    
- The **query side** subscribes to these topics and **consumes** the events to update its read models.
    

However, there's one important clarification: Kafka **does not track the business-level status** (e.g., "completed," "failed") of an event. Instead, it manages the **delivery status** for consumers.

#### What Kafka Manages

- **Ordering and Durability**: Guarantees that events within a partition are stored in order and are not lost.
    
- **Retention**: Keeps events for a configurable period, allowing for replay or rebuilding of read models.
    
- **Consumer Offsets**: Tracks which events a specific consumer group has successfully processed, preventing lost or duplicate processing.
    

The application itself is responsible for handling the business logic of what "completed" or "failed" means for a given event.

---

### If the system fails after taking an event, since Kafka is sourcing events, will the server continue processing these events from the logs after a reboot?

Yes, exactly. This is one of the most powerful features of using Kafka as an event store. **Kafka's durability and offset tracking guarantee that your system can recover from failures and resume processing.**

#### Failure Recovery Process

When a consumer service (like your query-side projector) crashes and reboots, the following happens:

1. The service reconnects to Kafka as part of the same consumer group.
    
2. Kafka knows the last successfully committed **offset** for that consumer group.
    
3. The consumer automatically resumes fetching messages from that last known offset, ensuring that no messages that were in flight during the crash are lost.
    

#### Example: Spring Kafka Listener

This listener will automatically resume from the last committed offset upon application restart.

```kotlin
// Kafka consumer automatically resumes from last committed offset
@KafkaListener(topics = ["money-transfer-events"])
fun handleTransferEvent(event: MoneyTransferredEvent) {
    // Processes events that weren't handled before the crash
    accountViewRepository.updateBalance(...)
}
```

#### Key Mechanisms for Recovery

- **Persistent Logs**: Kafka stores event data on disk, making it durable.
    
- **Consumer Offsets**: Kafka tracks the progress of each consumer group.
    
- **Automatic Resume**: Kafka clients are designed to automatically resume from where they left off.
    
- **Event Replay**: You can manually reset offsets to reprocess events from any point in time, which is useful for rebuilding read models.
    

---

### Kafka doesn't track business-level status. So, how does Spring know when the query side action is completed?

You're right, Kafka's job is to deliver the message. It's the **Spring application's responsibility** to signal back to Kafka once it has successfully processed that message. This is done through a mechanism called **consumer acknowledgment**.

#### Kafka's Role

Kafka acts as the reliable event storage and delivery system. It hands a message to a consumer and waits to hear back about whether it was processed successfully.

#### Spring's Completion Tracking

By default, Spring Kafka might acknowledge a message automatically as soon as it's received. However, for true transactional safety, you should configure **manual acknowledgment**. This gives your code explicit control.

Here's how it works:

1. You configure the listener to use manual acknowledgment.
    
2. Your code performs its business logic (e.g., updating the read database).
    
3. If the logic succeeds, you call `acknowledgment.acknowledge()`. This tells Kafka, "I'm done with this message; you can advance my offset."
    
4. If the logic fails, you can either do nothing (so the message is re-delivered later) or explicitly call `acknowledgment.nack()` to signal a failure and potentially trigger a retry or send the message to a dead-letter topic.
    

#### Code Example with Manual Acknowledgment

```kotlin
// Manual acknowledgment gives precise control
@KafkaListener(topics = ["money-transfer-events"])
fun handleTransferEvent(
    event: MoneyTransferredEvent,
    acknowledgment: Acknowledgment // Injected by Spring
) {
    try {
        // 1. Update the read model
        accountViewRepository.updateBalance(...)
                
        // 2. Acknowledge ONLY on successful processing
        acknowledgment.acknowledge()

    } catch (exception: Exception) {
        // 3. On failure, reject the message and retry after 5 seconds
        acknowledgment.nack(Duration.ofSeconds(5))
    }
}
```

#### Configuration for Manual Control

You need to disable auto-commit and set the ack-mode in your `application.yml`.

```yaml
# application.yml
spring:
  kafka:
    consumer:
      properties:
        # Disable Kafka's auto-commit feature
        enable.auto.commit: false 
    listener:
      # Tell Spring we will acknowledge messages manually
      ack-mode: MANUAL
```

---

### Summary

#### CQRS Fundamentals: CQRS vs. Traditional MVC

- **Core Principle**: CQRS separates write operations (**Commands**) from read operations (**Queries**).
    
- **Models**: It uses separate, distinct models for commands (often normalized, for transactional integrity) and queries (often denormalized, for read performance).
    
- **Traditional MVC**: Uses a single model for both reading and writing, which can lead to compromises in complexity and performance.
    
- **Use Cases**: CQRS is ideal for complex domains with high-performance requirements, while traditional patterns are better for simpler CRUD applications.
    

#### Event-Driven Flow and the Producer-Consumer Pattern

- **Event Flow**: The standard flow is `User Request` → `Command` → `Domain Event` → `Query Model Update`.
    
- **Role of Events**: Domain events are generated by the command side after a successful operation. They act as the mechanism to synchronize the query side.
    
- **Producer-Consumer**: The command side is the **Producer** of events, and the query side is the **Consumer**.
    
- **Event Sourcing**: An optional but related pattern where the sequence of events itself is the source of truth, not the current state of data.
    

#### Asynchronous Nature and User Notification

- **Synchronicity**: Unlike synchronous CRUD operations, CQRS commands are typically processed **asynchronously**. The client receives an immediate acknowledgment (`202 Accepted`) rather than waiting for the full process to complete.
    
- **Eventual Consistency**: The read model is not updated instantly, leading to a state of eventual consistency.
    
- **User Notification**: Because processing is asynchronous, patterns are needed to inform the user of completion. Common strategies include:
    
    - Client-side polling
        
    - WebSockets or Server-Sent Events (SSE) for real-time updates
        
    - Push notifications or email/SMS alerts
        

#### Implementation with Spring and Kafka

- **Spring Events**: For simple, in-process CQRS, Spring's `@EventListener` and `ApplicationEventPublisher` can manage the event flow between command and query components. `@Async` is used to prevent blocking.
    
- **Kafka as Event Backbone**: For distributed or more robust systems, Kafka acts as the durable, scalable event store.
    
    - **Role**: It decouples the command and query sides, persists events, and guarantees message delivery.
        
- **Failure Recovery**: Kafka's persistent logs and consumer offsets ensure that if a consumer service fails, it can restart and resume processing events from where it left off, preventing data loss.
    
- **Completion Tracking**: The application, not Kafka, is responsible for business-level completion. In Spring Kafka, this is managed via **manual acknowledgment**. The consumer code explicitly calls `acknowledgment.acknowledge()` after successfully processing an event, telling Kafka it can advance the offset.