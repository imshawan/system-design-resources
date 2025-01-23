## **Fundamentals of System Design**

#### Purpose:

Build foundational knowledge about what system design is and why it matters.

---

## Chapter list

Here’s a detailed breakdown of the **chapters** for the first section, **Fundamentals of System Design**. Each chapter is designed to provide foundational knowledge and hands-on exercises for practical understanding.

---

### **1. Fundamentals of System Design**

#### **[1.1 Overview of System Design](1-Fundamentals/1.1-Overview-of-System-Design/README.md)**

- **Purpose**: Understand what system design is and its importance in solving real-world engineering problems.
- **Chapters**:
    1. **[Introduction to System Design](1-Fundamentals/1.1-Overview-of-System-Design/README.md)**
        - [What is system design?](1-Fundamentals/1.1-Overview-of-System-Design/1-System-Design/1-what-is-system-design.md)
        - [Difference between high-level and low-level design.](1-Fundamentals/1.1-Overview-of-System-Design/1-System-Design/2-high-and-low-level-designs.md)
        - Examples of real-world systems (e.g., e-commerce, social media platforms).
            1. [E-commerce Platform System Design](1-Fundamentals/1.1-Overview-of-System-Design/1-System-Design/3-example.md)
            2. [System Design for Online Streaming Service](1-Fundamentals/1.1-Overview-of-System-Design/1-System-Design/4-example.md)
            3. [System Design for Social Media Platform](1-Fundamentals/1.1-Overview-of-System-Design/1-System-Design/5-example.md)
            4. [System Design for Online Education Platform](1-Fundamentals/1.1-Overview-of-System-Design/1-System-Design/6-example.md)
    2. **[Why System Design Matters](1-Fundamentals/1.1-Overview-of-System-Design/2-Why-it-Matters/README.md)**
        - Impact on scalability, reliability, and maintainability.
        - Role of system design in interviews and real-world projects.
    3. **[System Design Process](1-Fundamentals/1.1-Overview-of-System-Design/3-The-Process/README.md)**
        - Gathering requirements.
        - Defining key use cases and constraints.
        - Designing high-level architecture and low-level components.
    4. **[Types of System Design](1-Fundamentals/1.1-Overview-of-System-Design/4-The-Types/README.md)**
        - Designing web applications vs distributed systems.
        - Real-time systems vs batch systems.
        - Offline-first systems vs always-online systems.

---

#### **[1.2 Principles of Scalability, Availability, and Reliability](1-Fundamentals/1.2-Principles-of-Scalability-Availability-and-Reliability/README.md)**

- **Purpose**: Learn core principles that guide the design of robust systems.
- **Chapters**:
    1. **[Scalability](1-Fundamentals/1.2-Principles-of-Scalability-Availability-and-Reliability/1-Scalability/README.md)**
        - Vertical scaling vs horizontal scaling.
        - Stateless vs stateful architectures.
        - Examples of scalable designs (e.g., distributed databases).
    2. **[Availability](1-Fundamentals/1.2-Principles-of-Scalability-Availability-and-Reliability/2-Availability/README.md)**
        - Definitions: Availability vs uptime.
        - Designing for high availability (HA).
        - Redundancy and failover mechanisms.
    3. **[Reliability](1-Fundamentals/1.2-Principles-of-Scalability-Availability-and-Reliability/3-Reliability/README.md)**
        - Reliability vs availability.
        - Fault tolerance and graceful degradation.
        - Techniques for improving reliability (e.g., retries, idempotency).
    4. **[Trade-offs Between Scalability, Availability, and Reliability](1-Fundamentals/1.2-Principles-of-Scalability-Availability-and-Reliability/4-Trade-offs/README.md)**
        - How to balance trade-offs based on requirements.
        - Real-world examples of trade-offs.

---

#### **[1.3 Key Metrics: Latency, Throughput, Response Time, etc.](1-Fundamentals/1.3-Key-Metrics-Latency-Throughput-Response-Time-etc/README.md)**

- **Purpose**: Understand key performance metrics for evaluating system efficiency.
- **Chapters**:
    1. **[Latency](1-Fundamentals/1.3-Key-Metrics-Latency-Throughput-Response-Time-etc/1-Latency/README.md)**
        - Network latency vs application latency.
        - Sources of latency in distributed systems.
        - Techniques for reducing latency (e.g., caching, CDN).
    2. **[Throughput](1-Fundamentals/1.3-Key-Metrics-Latency-Throughput-Response-Time-etc/2-Throughput/README.md)**
        - Definition and measurement.
        - Maximizing throughput with parallel processing and batching.
    3. **[Response Time](1-Fundamentals/1.3-Key-Metrics-Latency-Throughput-Response-Time-etc/3-Response-Time/README.md)**
        - Average response time vs percentile response times (e.g., P99).
        - How to optimize response times.
    4. **[Connections Between Metrics](1-Fundamentals/1.3-Key-Metrics-Latency-Throughput-Response-Time-etc/4-Connections-Between-Metrics/README.md)**
        - How latency and throughput interact.
        - Choosing the right metrics based on system requirements.
    5. **[Hands-On Exercises](1-Fundamentals/1.3-Key-Metrics-Latency-Throughput-Response-Time-etc/5-Hands-On-Exercises/README.md)**
        - Simulate latency and throughput scenarios (e.g., HTTP requests under load).
        - Use tools like Apache JMeter or k6 for load testing.

---

#### **[1.4 CAP Theorem and PACELC Theorem](1-Fundamentals/1.4-CAP-Theorem-and-PACELC-Theorem/README.md)**

- **Purpose**: Learn how distributed systems balance trade-offs in consistency, availability, and partition tolerance.
- **Chapters**:
    1. **[CAP Theorem Basics](1-Fundamentals/1.4-CAP-Theorem-and-PACELC-Theorem/1-CAP-Theorem-Basics/README.md)**
        - Definition and history of CAP Theorem.
        - Explaining consistency, availability, and partition tolerance.
        - Why you can only pick two of the three.
    2. **[Real-World Implications of CAP](1-Fundamentals/1.4-CAP-Theorem-and-PACELC-Theorem/2-Real-World-Implications-of-CAP/README.md)**
        - Examples of systems focusing on consistency (e.g., relational databases).
        - Examples of systems focusing on availability (e.g., NoSQL databases).
        - How network partitions affect system behavior.
    3. **[PACELC Theorem](1-Fundamentals/1.4-CAP-Theorem-and-PACELC-Theorem/3-PACELC%20Theorem/README.md)**
        - Introduction to PACELC (Partitioning, Availability, Consistency, Else Latency, Consistency).
        - Real-world examples of latency vs consistency trade-offs.
        - Comparing CAP and PACELC with diagrams.
    4. **[Hands-On Exercises](1-Fundamentals/1.4-CAP-Theorem-and-PACELC-Theorem/4-Hands-On-Exercises/README.md)**
        - Create a partitioned system simulation and test availability vs consistency trade-offs.
        - Discuss PACELC in context of popular systems like DynamoDB or Cassandra.

---

#### **[1.5 Consistency Models (Strong, Eventual, Causal)](1-Fundamentals/1.5-Consistency-Models-Strong-Eventual-Causal/README.md)**

- **Purpose**: Explore different consistency models used in distributed systems.
- **Chapters**:
    1. **[Introduction to Consistency](1-Fundamentals/1.5-Consistency-Models-Strong-Eventual-Causal/1-Introduction-to-Consistency/README.md)**
        - What is consistency in distributed systems?
        - Why consistency is challenging in distributed environments.
    2. **[Strong Consistency](1-Fundamentals/1.5-Consistency-Models-Strong-Eventual-Causal/2-Strong-Consistency/README.md)**
        - Definition and examples (e.g., RDBMS with ACID properties).
        - Trade-offs and use cases.
    3. **[Eventual Consistency](1-Fundamentals/1.5-Consistency-Models-Strong-Eventual-Causal/3-Eventual-Consistency/README.md)**
        - Definition and examples (e.g., DynamoDB, Cassandra).
        - How eventual consistency works (e.g., anti-entropy, read-repair).
    4. **[Causal Consistency](1-Fundamentals/1.5-Consistency-Models-Strong-Eventual-Causal/4-Causal-Consistency/README.md)**
        - Definition and examples (e.g., Git versioning).
        - Use cases where causal consistency is essential.
    5. **[Hands-On Exercises](1-Fundamentals/1.5-Consistency-Models-Strong-Eventual-Causal/5-Hands-On-Exercises/README.md)**
        - Simulate strong, eventual, and causal consistency in a distributed environment.
        - Implement a simple key-value store with eventual consistency.

---

### **[Implementation Tasks for Fundamentals](1-Fundamentals/2-Implementation/README.md)**

- **[Drawing and Planning](1-Fundamentals/2-Implementation/1-Drawing-and-Planning/README.md)**:
    - Create diagrams to explain CAP and PACELC trade-offs.
    - Map out a flow of metrics (latency, throughput) for a sample architecture.


#### **1. Overview of System Design**

1. **Gathering Requirements**:
    - Define functional and non-functional requirements for a simple system like a URL shortener.
    - Identify constraints such as data storage, scalability, and high availability.
2. **Design High-Level Architecture**:
    - Use tools like draw.io or Lucidchart to create a high-level architecture diagram for the system.
    - Include components such as frontend, backend, database, and caching layer.
3. **Explore Trade-offs**:
    - Discuss trade-offs in choosing a relational database vs a NoSQL database for the system.
    - Create a document explaining decisions made based on scalability and consistency requirements.

---

#### **2. Principles of Scalability, Availability, and Reliability**

1. **Scalability**:
    - Implement a load balancer using tools like Nginx or HAProxy to distribute traffic.
    - Create a script to simulate increasing traffic and observe how horizontal scaling affects performance.
2. **Availability**:
    - Design and implement a failover mechanism for a database using read replicas.
    - Perform manual failover testing to ensure availability during primary database downtime.
3. **Reliability**:
    - Implement a retry mechanism in an HTTP client to handle transient failures.
    - Add idempotency logic to an API endpoint to ensure consistent behavior during retries.

---

#### **3. Key Metrics: Latency, Throughput, Response Time**

1. **Latency Measurement**:
    - Write a script to send HTTP requests to a mock server and measure response times.
    - Visualize latency distribution (e.g., using histograms or percentiles like P99).
2. **Throughput Analysis**:
    - Simulate a workload with multiple concurrent requests using a tool like Apache JMeter or k6.
    - Measure the maximum requests per second (RPS) the system can handle before latency degrades.
3. **Response Time Optimization**:
    - Introduce caching at the application layer (e.g., with Redis) to reduce response times.
    - Compare response times with and without caching enabled.

---

#### **4. CAP Theorem and PACELC Theorem**

1. **CAP Trade-offs**:
    - Set up a distributed key-value store (e.g., Consul or Etcd).
    - Simulate network partitions and observe behavior when prioritizing consistency vs availability.
2. **PACELC Exploration**:
    - Use a NoSQL database like DynamoDB or MongoDB to demonstrate latency vs consistency trade-offs.
    - Write a report comparing latency in strongly consistent and eventually consistent reads.
3. **Visualization**:
    - Create diagrams illustrating scenarios where CAP and PACELC apply.
    - Include real-world examples of systems (e.g., DynamoDB for AP, Spanner for CP).

---

#### **5. Consistency Models (Strong, Eventual, Causal)**

1. **Strong Consistency Implementation**:
    - Create a relational database setup with ACID properties (e.g., PostgreSQL).
    - Write a script to test transactional consistency by simulating concurrent writes.
2. **Eventual Consistency Simulation**:
    - Build a simple distributed key-value store where nodes asynchronously replicate data.
    - Test consistency by performing writes and observing when all nodes eventually converge.
3. **Causal Consistency Experiment**:
    - Implement a versioning system (e.g., using vector clocks) to simulate causal consistency.
    - Create scenarios demonstrating causal relationships, such as a collaborative editing tool.

---

#### **Final Project for Fundamentals**

- **System Design Case Study**:
    - Design a simple, distributed chat application with the following requirements:
        - Low latency for message delivery.
        - High availability during network partitions.
        - Eventual consistency for message order.
    - Deliverables:
        - High-level architecture diagram.
        - CAP and PACELC trade-off decisions.
        - Explanation of chosen consistency model.
        - Implementation of core features with a focus on latency, scalability, and reliability.

---