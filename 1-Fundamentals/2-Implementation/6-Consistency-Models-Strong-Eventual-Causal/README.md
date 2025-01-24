## **5. Consistency Models (Strong, Eventual, Causal)**

1.  **[Strong Consistency Implementation](./strong-consistency-implementation.md)**:
    -   Create a relational database setup with ACID properties (e.g., PostgreSQL).
    -   Write a script to test transactional consistency by simulating concurrent writes.
2.  **[Eventual Consistency Simulation](./eventual-consistency-simulation.md)**:
    -   Build a simple distributed key-value store where nodes asynchronously replicate data.
    -   Test consistency by performing writes and observing when all nodes eventually converge.
3.  **[Causal Consistency Experiment](./causal-consistency-experiment.md)**:
    -   Implement a versioning system (e.g., using vector clocks) to simulate causal consistency.
    -   Create scenarios demonstrating causal relationships, such as a collaborative editing tool.