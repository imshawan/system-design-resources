## **1.4 CAP Theorem and PACELC Theorem**

- **Purpose**: Learn how distributed systems balance trade-offs in consistency, availability, and partition tolerance.
- **Chapters**:
    1. **[CAP Theorem Basics](./1-CAP-Theorem-Basics/README.md)**
        - Definition and history of CAP Theorem.
        - Explaining consistency, availability, and partition tolerance.
        - Why you can only pick two of the three.
    2. **[Real-World Implications of CAP](./2-Real-World-Implications-of-CAP/README.md)**
        - Examples of systems focusing on consistency (e.g., relational databases).
        - Examples of systems focusing on availability (e.g., NoSQL databases).
        - How network partitions affect system behavior.
    3. **[PACELC Theorem](./3-PACELC%20Theorem/README.md)**
        - Introduction to PACELC (Partitioning, Availability, Consistency, Else Latency, Consistency).
        - Real-world examples of latency vs consistency trade-offs.
        - Comparing CAP and PACELC with diagrams.
    4. **[Hands-On Exercises](./4-Hands-On-Exercises/README.md)**
        - Create a partitioned system simulation and test availability vs consistency trade-offs.
        - Discuss PACELC in context of popular systems like DynamoDB or Cassandra.