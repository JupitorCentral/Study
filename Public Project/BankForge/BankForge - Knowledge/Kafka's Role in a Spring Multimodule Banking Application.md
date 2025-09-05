- [x] Kafka's Role in a Spring Multimodule Banking Application ➕ 2025-07-27 📅 2025-07-27 ✅ 2025-09-05

### [kafka and multimodules] Is kafka used for load balancing between other modules inside a spring app ? Or, between microservices, or spriing <-> db ?

Kafka is **not** used for load balancing in the traditional sense, but it enables distributed processing and workload distribution.

#### Between Microservices

Kafka is primarily used for **asynchronous communication** between microservices. It acts as a message broker for:

- Event-driven architecture where services publish and consume events.
    
- Decoupling of services through a publish-subscribe model.
    
- Reliable message delivery.
    

However, Kafka provides **consumer load balancing** within consumer groups. When multiple instances of a microservice consume from the same topic, Kafka distributes partitions among them. The number of partitions limits the degree of parallelism, as each partition can only be consumed by one consumer in a group.

#### Within Spring Applications (Multimodule)

For modules within a single Spring application, Kafka is less common for load balancing. More typical methods include:

- Direct method calls between modules.
    
- Spring's application events for internal communication.
    

When used, it's usually for concurrent processing via **consumer concurrency configuration** in Spring Kafka, which allows multiple consumer threads.

#### Spring ↔ Database

Kafka is **not used for load balancing** between Spring applications and databases. Instead, it can be used to:

- Coordinate **transactional consistency** between database operations and message publishing using `KafkaTransactionManager`.
    
- Implement patterns like the **Outbox Pattern** to ensure eventual consistency between database changes and event publishing.
    

#### Key Limitations

- Only one consumer per partition per consumer group.
    
- Load balancing is limited by the number of partitions in a topic.
    
- Consumer rebalancing can temporarily halt message processing.
    

**Additional topics to explore**: Kafka Streams, Schema Registry, Kafka Connect, partition strategies.

### Well, I don't get it - can spring module communicate without kafka ? Why my project uses kafka ? For what ? For implementing CQRS ?

#### Can Spring Modules Communicate Without Kafka?

Yes, absolutely. Spring modules can communicate through:

- Direct method calls.
    
- Spring's internal event publishing (`@EventListener`, `ApplicationEventPublisher`).
    
- Shared beans and dependency injection.
    
- Internal message queues (like RabbitMQ for lighter use cases).
    

#### Why Your Banking Project Uses Kafka

Your project likely uses Kafka for **CQRS implementation** and other banking-specific requirements.

**CQRS Pattern Support**

- **Command-Query separation**: Commands (writes) and queries (reads) use different data models.
    
- **Event sourcing**: Kafka stores all domain events as the immutable source of truth.
    
- **Event replay**: You can rebuild read models from the event history in Kafka.
    

**Banking-Specific Requirements**

- **Audit trail**: Every transaction must be traceable and stored immutably.
    
- **Eventual consistency**: Account balances can be eventually consistent across different read models.
    
- **High throughput**: Necessary to handle thousands of concurrent banking operations.
    
- **Fault tolerance**: Critical for financial data integrity.
    

**Kafka vs Direct Communication in Your Context**

|Direct Communication|Kafka Events|
|---|---|
|Immediate consistency|Eventual consistency|
|Tight coupling|Loose coupling|
|Simple for basic operations|Better for complex workflows|
|Single point of failure|Distributed resilience|

**Additional topics**: Event sourcing patterns, Saga pattern for distributed transactions, Kafka Streams for real-time processing, Outbox pattern implementation in Kotlin.

### But, when saving audit trail - could'nt it be impplemented by just storing db before proceeding transaction ?

Yes, you _could_ implement audit trails by storing database snapshots, but this approach has significant limitations compared to a Kafka-based event sourcing approach, especially for banking.

#### Why Kafka is Better for Banking Audit Trails

- **Storage Efficiency**: Events store only _what changed_ (the delta), not entire records. Database snapshots duplicate massive amounts of unchanged data, which is inefficient for systems with millions of daily transactions.
    
- **Temporal Queries**: Kafka's event log provides a complete, ordered timeline of all changes, making it easy to answer questions like "what happened between 2 PM and 3 PM?". Database snapshots only show "before" and "after" states.
    
- **Compliance Requirements**: Banking regulations often require immutable audit logs. Kafka topics are append-only and immutable by design, whereas database records can be modified. Events also capture the _business context_ (the "why").
    
- **System Integration**: Other modules can receive real-time notifications of changes by subscribing to event streams, which is far more efficient than database polling.
    

#### Database Approach Limitations

- **Performance impact**: Snapshotting full records is computationally expensive.
    
- **Storage explosion**: Storage needs grow exponentially with transaction volume.
    
- **Limited reconstruct-ability**: You can't replay the business logic that led to a state change.
    
- **Tight coupling**: All modules must query the same database schema.
    

**Related topics**: Event sourcing patterns, Outbox pattern, CDC (Change Data Capture), Kafka retention policies, Kotlin coroutines for event processing.

### I still can't get about storage efficiency - if I manipulate which info should be stored, it's still efficient for event sourcing.

You're partially right, but the fundamental difference is in _what_ gets stored.

#### Event Sourcing vs Selective Snapshots

- **Event Sourcing (Kafka)**: Stores only the **delta** (what changed).
    
    - Example: `AccountCredited { accountId: "123", amount: 100.00, timestamp: ... }`
        
- **Database Snapshots (even selective)**: Stores the **resulting state** after the change.
    
    - Example: `Account { id: "123", balance: 2500.00, lastModified: ... }`
        

#### Storage Math Example

For 1 million daily transactions on an account:

- **Events**: 1M small change records (~50 bytes each).
    
- **Selective snapshots**: 1M larger account state records (~200+ bytes each).
    
- **Difference**: 4x+ more storage even with "selective" data.
    

#### Beyond Storage Efficiency

Event sourcing gives you more than just storage savings:

- **Business intent**: The event itself tells you _why_ the change happened (`AccountCredited` vs. a simple balance update).
    
- **Exact sequence**: The order of operations is preserved, which is critical in banking.
    
- **Replay capability**: You can reconstruct any point-in-time state by replaying events.
    

Database snapshots lose the "why," the ability to replay business logic, and detailed operational sequencing. For banking, you need the **business context** of each change, not just the resulting state.

**Additional topics**: Event store compaction, snapshot strategies in Kafka, Kotlin data classes for events, space-efficient serialization formats.

### Then, because size of db tables is fixed, storing transactions data needs much more space you mean?

#### Does Storing Transaction Data Require Much More Space With Fixed-Size Tables?

Yes, over time, a traditional database table for transactions will typically require much more space than a Kafka event log. The storage required depends on:

- How much detail is stored per transaction row.
    
- The growth rate of transactions.
    
- Whether you are storing snapshots of state or individual change events.
    

Even with a fixed table schema, each transaction requires a new row. In banking, these rows often contain a lot of metadata for audit purposes, making them large.

#### Comparison: Event Sourcing Versus Table Snapshots

|Approach|What’s Stored|Typical Storage Per Entry|Space Growth|
|---|---|---|---|
|**Event Sourcing (Kafka)**|Only what changed|Small (often <100 bytes)|Linear, very efficient|
|**DB Transaction Table**|Every transaction (full detail)|Medium to large|Linear, can get large|
|**State Snapshots**|Whole object state (full row)|Largest|Rapid growth|

**Bottom line**: If you simply log every transaction in a database table without optimizing, the table grows rapidly. Event sourcing uses less space per entry because it stores only the **change**, not the full object state. Over time, the total storage required for a traditional DB table will typically outpace a well-designed event log.

**Related topics**: Database table partitioning, compression techniques for transaction storage, retention/archive strategies.

### Still can't get it why kafka is efficient for event sourcing. Could you give me any examples for each topics you said above?

#### Why Kafka Is Efficient for Event Sourcing (with Examples)

1. **Append-Only Log: Fast and Reliable Storage**
    
    - Each event is written sequentially and is immutable.
        
    - **Example**: Every account change (e.g., `AccountCredited`, `AccountDebited`) is simply appended to a Kafka topic and remains ordered by time.
        
2. **Partitioning & Parallelism**
    
    - Data is split by a key (e.g., `accountId`) across different partitions.
        
    - **Example**: All changes for `accountId=1001` go to one partition, and `accountId=2002` to another. This allows different consumers to process events for different accounts in parallel, enabling high throughput.
        
3. **Log Compaction for Latest State**
    
    - Kafka can be configured to keep only the most recent event for each unique key.
        
    - **Example**: For a topic storing customer KYC data, log compaction ensures that only the latest submitted document per user is retained, saving space while keeping the current state available.
        
4. **Scalable Replay and Recovery**
    
    - Consumers can replay events from any point in the log's history to rebuild state.
        
    - **Example**: If a "transaction summary" read model gets corrupted, it can be completely rebuilt by consuming all past transaction events from the Kafka log from the beginning.
        
5. **Configurable Retention**
    
    - You can configure how long events are stored (days, weeks, or indefinitely).
        
    - **Example**: A regulatory requirement to keep all transaction events for 7 years can be met with a simple Kafka configuration setting.
        
6. **Efficient Serialization & Batching**
    
    - Supports space-efficient formats like Avro or Protobuf and batches multiple small messages into a single network request.
        
    - **Example**: When a batch of 100 transfers is processed, they can be sent together as one message to minimize disk and network I/O.
        

**Other Relevant Topics**: Kafka Connect, retention vs. compaction settings, distributed commit log theory.

### Then is kafka only used for event sourcing on my project?

No, Kafka is likely used for more than just event sourcing in your project.

#### Typical Uses of Kafka in a Kotlin Multimodule Bank App

- **Event Sourcing (Primary Use)**: Storing an immutable log of all domain events for auditability, state reconstruction, and compliance.
    
- **Inter-module Communication**: Decoupling modules like Accounts, Transactions, and Notifications, allowing them to react to events asynchronously.
    
- **Asynchronous Processing**: Handling background tasks (e.g., sending alerts, updating analytics) without blocking the main transaction flow.
    
- **Projections/Read Models**: Streaming events to build and update query-optimized views in databases like MongoDB or PostgreSQL.
    

#### Other Potential Use Cases

- Real-time analytics pipelines.
    
- Integration with external services (via Kafka Connect).
    
- Sagas or orchestration of long-running business processes.
    

**Related Topics That Exist**: CQRS, Event-driven microservices patterns, Log compaction and retention in Kafka, Kafka Connect, Kafka Streams.

### When you say inter module communication- but if I run one spring server? Cant one http thread fromtomcat pool all modules subsequently?

#### Inter-Module Communication in a Single Spring Server

Yes, you are correct. If you run only **one Spring server** (a monolith), a single Tomcat thread can call multiple modules (beans/components) sequentially within the same process.

- Each incoming HTTP request can invoke any service or repository bean in your application.
    
- Modules (as Kotlin packages/classes) can call each other directly. No messaging system is needed for this.
    

#### Why Use Kafka for Inter-Module Communication?

Kafka becomes necessary when:

- Modules are **separate microservices** deployed independently.
    
- You need **asynchronous** communication to decouple processing between components.
    
- Business events need to be persisted, replayed, or handled by multiple, possibly external, listeners.
    

For a **single Spring server monolith**, direct method calls are the fastest and simplest approach.

#### Typical Scenarios

|Pattern|How Modules Communicate|
|---|---|
|**Single Spring server**|Direct function/method calls|
|**Multiple microservices**|HTTP REST, Kafka events, etc.|

**Topics That Exist**: Event-driven patterns within monoliths, async processing using Spring's `@Async`, transition strategies from monolith to microservices.

### And, can't I still asynchronous call like notification with out kafka?

Yes, you absolutely can implement asynchronous notifications in a Spring project **without using Kafka**.

#### Native Spring Approaches

- **`@Async` methods**: Annotate a method with `@Async` to have Spring execute it in a background thread pool. This is a simple, "fire-and-forget" approach suitable for tasks where you don't need guaranteed delivery.
    
- **Application Events**: Spring's internal event system (`ApplicationEventPublisher`, `@EventListener`) can handle events asynchronously within the same application, especially when combined with `@Async`.
    

#### Alternative Message Brokers

- **RabbitMQ**: A popular and reliable message broker that is often considered more lightweight than Kafka for traditional asynchronous messaging tasks.
    
- **Others**: ActiveMQ, Redis Streams, Google Pub/Sub, etc., all offer async messaging capabilities with different features.
    

#### When to Use Kafka

Kafka is generally favored when your requirements go beyond simple async calls and you need:

- High throughput and scalability for massive workloads.
    
- Durability, ordering guarantees, and the ability to replay messages.
    
- Long-term storage of event/notification history.
    

For straightforward asynchronous notifications, Kafka is often overkill.

**Related Topics**: How to choose between `@Async`, RabbitMQ, or HTTP callbacks; trade-offs between different async architectures.

### Projections/read models: Streams events to build query-optimized views in databases like MongoDB or PostgreSQL. I can't get it

#### What Are Projections/Read Models in Event Sourcing?

**Projections** (or **read models**) are specialized, query-optimized database tables or collections. They are built and kept up-to-date by continuously processing a stream of events from a source like Kafka. Their sole purpose is to make data fast and easy to query for your application's UI, reports, or APIs.

#### How It Works

1. **Events are Emitted**: When something happens in your system, an event (e.g., `"MoneyDeposited"`, `"AccountOpened"`) is published to a Kafka topic.
    
2. **A Consumer Listens**: A background service (a "projector" or consumer) subscribes to these events.
    
3. **The Read Model is Updated**: The service processes each event and updates a separate, query-friendly database (like PostgreSQL or MongoDB). It transforms the event data into a structure that is easy to read.
    

#### Why Do This?

Raw event logs in Kafka are designed for writing and replaying sequences, not for complex queries like "get the current balance for account 123" or "list all transactions for a user last month." Projections create optimized views of the data for these specific read operations.

#### Simple Example (Banking)

Suppose a Kafka topic receives these events in order:

1. `AccountCreated(accountId=1, name="Sam")`
    
2. `MoneyDeposited(accountId=1, amount=500)`
    
3. `MoneyWithdrawn(accountId=1, amount=200)`
    

A consumer could create two different read models from these events:

- **A balance summary in PostgreSQL** (for quick balance checks):
    
    SQL
    
    ```
    -- account_balances table
    account_id | current_balance
    --------------------------
    1          | 300
    ```
    
- **A transaction history in MongoDB** (for a UI page):
    
    JSON
    
    ```
    {
      "accountId": 1,
      "transactions": [
        {"type": "Deposited", "amount": 500},
        {"type": "Withdrawn", "amount": 200}
      ]
    }
    ```
    

When a new event arrives (e.g., another deposit), the consumer automatically updates these views.

#### Key Points

- Read models are disposable. If one gets corrupted, you can delete it and rebuild it by replaying all the events from Kafka.
    
- They are a core part of the CQRS (Command Query Responsibility Segregation) pattern.
    
- They allow you to have multiple, different views of the same data, each tailored to a specific query need.
    

**Related topics**: Event handlers and updaters, eventual consistency in read models, tools for automated projections.

---

### Summary

**Kafka's Core Role in Distributed Systems**

- **Primary Use**: Acts as a message broker for asynchronous, event-driven communication between microservices, not for traditional load balancing.
    
- **Workload Distribution**: Achieves parallelism through consumer groups, where partitions of a topic are distributed among consumer instances.
    
- **Decoupling**: Enables services to communicate without being directly aware of each other, increasing resilience and scalability.
    

**Kafka for Event Sourcing & Audit Trails**

- **Immutable Log**: Serves as an append-only, immutable log, making it ideal for storing a permanent audit trail of all business events, a key requirement in banking.
    
- **Storage Efficiency**: Stores small "delta" events (what changed) rather than full state snapshots, making it more space-efficient over time compared to traditional database audit tables.
    
- **State Reconstruction**: Allows for complete system state reconstruction at any point in time by replaying events from the log.
    

**Asynchronous Communication: Kafka vs. Alternatives**

- **In-Monolith**: For async tasks within a single Spring application, native features like `@Async` or Spring Events are simpler and often sufficient.
    
- **When to use Kafka**: Choose Kafka for high-throughput, durable, and scalable messaging, especially when event history and replay capabilities are needed.
    
- **Alternatives**: For simpler inter-service messaging, lightweight brokers like RabbitMQ are a common alternative.
    

**CQRS Implementation with Projections/Read Models**

- **CQRS Enabler**: Kafka is a foundational technology for implementing the Command Query Responsibility Segregation (CQRS) pattern.
    
- **Command/Write Side**: The "write" side of the application publishes events to Kafka.
    
- **Query/Read Side (Projections)**: The "read" side consists of consumers that process these events to build and maintain query-optimized `read models` (or `projections`) in separate databases (e.g., PostgreSQL, MongoDB), tailored for specific UI or API queries.