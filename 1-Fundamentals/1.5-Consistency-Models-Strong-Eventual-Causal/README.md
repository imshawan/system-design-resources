## **1.5 Consistency Models (Strong, Eventual, Causal)**

- **Purpose**: Explore different consistency models used in distributed systems.
- **Chapters**:
    1. **[Introduction to Consistency](./1-Introduction-to-Consistency/README.md)**
        - What is consistency in distributed systems?
        - Why consistency is challenging in distributed environments.
    2. **[Strong Consistency](./2-Strong-Consistency/README.md)**
        - Definition and examples (e.g., RDBMS with ACID properties).
        - Trade-offs and use cases.
    3. **[Eventual Consistency](./3-Eventual-Consistency/README.md)**
        - Definition and examples (e.g., DynamoDB, Cassandra).
        - How eventual consistency works (e.g., anti-entropy, read-repair).
    4. **[Causal Consistency](./4-Causal-Consistency/README.md)**
        - Definition and examples (e.g., Git versioning).
        - Use cases where causal consistency is essential.
    5. **[Hands-On Exercises](./5-Hands-On-Exercises/README.md)**
        - Simulate strong, eventual, and causal consistency in a distributed environment.
        - Implement a simple key-value store with eventual consistency.