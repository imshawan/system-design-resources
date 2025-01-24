
## **Implementation Tasks for Fundamentals**

### **[Drawing and Planning](./1-Drawing-and-Planning/README.md)**:
- Create diagrams to explain CAP and PACELC trade-offs.
- Map out a flow of metrics (latency, throughput) for a sample architecture.

### **[1. Overview of System Design](./2-Overview-of-System-Design/README.md)**

1.  **[Gathering Requirements](./2-Overview-of-System-Design/gathering-requirements.md)**:
    -   Define functional and non-functional requirements for a simple system like a URL shortener.
    -   Identify constraints such as data storage, scalability, and high availability.
2.  **[Design High-Level Architecture](./2-Overview-of-System-Design/design-high-level-architecture.md)**:
    -   Use tools like [draw.io](http://draw.io) or Lucidchart to create a high-level architecture diagram for the system.
    -   Include components such as frontend, backend, database, and caching layer.
3.  **[Explore Trade-offs](./2-Overview-of-System-Design/explore-trade-offs.md)**:
    -   Discuss trade-offs in choosing a relational database vs a NoSQL database for the system.
    -   Create a document explaining decisions made based on scalability and consistency requirements.

----------

### **[2. Principles of Scalability, Availability, and Reliability](./3-Principles-of-Scalability-Availability-and-Reliability/README.md)**

1.  **[Scalability](./3-Principles-of-Scalability-Availability-and-Reliability/scalability.md)**:
    -   Implement a load balancer using tools like Nginx or HAProxy to distribute traffic.
    -   Create a script to simulate increasing traffic and observe how horizontal scaling affects performance.
2.  **[Availability](./3-Principles-of-Scalability-Availability-and-Reliability/availability.md)**:
    -   Design and implement a failover mechanism for a database using read replicas.
    -   Perform manual failover testing to ensure availability during primary database downtime.
3.  **[Reliability](./3-Principles-of-Scalability-Availability-and-Reliability/reliability.md)**:
    -   Implement a retry mechanism in an HTTP client to handle transient failures.
    -   Add idempotency logic to an API endpoint to ensure consistent behavior during retries.

----------

### **[3. Key Metrics: Latency, Throughput, Response Time](./4-Key-Metrics-Latency-Throughput-Response-Time-etc/README.md)**

1.  **[Latency Measurement](./4-Key-Metrics-Latency-Throughput-Response-Time-etc/latency-measurement.md)**:
    -   Write a script to send HTTP requests to a mock server and measure response times.
    -   Visualize latency distribution (e.g., using histograms or percentiles like P99).
2.  **[Throughput Analysis](./4-Key-Metrics-Latency-Throughput-Response-Time-etc/throughput-analysis.md)**:
    -   Simulate a workload with multiple concurrent requests using a tool like Apache JMeter or k6.
    -   Measure the maximum requests per second (RPS) the system can handle before latency degrades.
3.  **[Response Time Optimization](./4-Key-Metrics-Latency-Throughput-Response-Time-etc/response-time-optimization.md)**:
    -   Introduce caching at the application layer (e.g., with Redis) to reduce response times.
    -   Compare response times with and without caching enabled.

----------

### **[4. CAP Theorem and PACELC Theorem](./5-CAP-Theorem-and-PACELC-Theorem/README.md)**

1.  **[CAP Trade-offs](./5-CAP-Theorem-and-PACELC-Theorem/cap-trade-offs.md)**:
    -   Set up a distributed key-value store (e.g., Consul or Etcd).
    -   Simulate network partitions and observe behavior when prioritizing consistency vs availability.
2.  **[PACELC Exploration](./5-CAP-Theorem-and-PACELC-Theorem/pacelc-exploration.md)**:
    -   Use a NoSQL database like DynamoDB or MongoDB to demonstrate latency vs consistency trade-offs.
    -   Write a report comparing latency in strongly consistent and eventually consistent reads.
3.  **[Visualization](./5-CAP-Theorem-and-PACELC-Theorem/visualization.md)**:
    -   Create diagrams illustrating scenarios where CAP and PACELC apply.
    -   Include real-world examples of systems (e.g., DynamoDB for AP, Spanner for CP).

----------

### **[5. Consistency Models (Strong, Eventual, Causal)](./6-Consistency-Models-Strong-Eventual-Causal/README.md)**

1.  **[Strong Consistency Implementation](./6-Consistency-Models-Strong-Eventual-Causal/strong-consistency-implementation.md)**:
    -   Create a relational database setup with ACID properties (e.g., PostgreSQL).
    -   Write a script to test transactional consistency by simulating concurrent writes.
2.  **[Eventual Consistency Simulation](./6-Consistency-Models-Strong-Eventual-Causal/eventual-consistency-simulation.md)**:
    -   Build a simple distributed key-value store where nodes asynchronously replicate data.
    -   Test consistency by performing writes and observing when all nodes eventually converge.
3.  **[Causal Consistency Experiment](./6-Consistency-Models-Strong-Eventual-Causal/causal-consistency-experiment.md)**:
    -   Implement a versioning system (e.g., using vector clocks) to simulate causal consistency.
    -   Create scenarios demonstrating causal relationships, such as a collaborative editing tool.