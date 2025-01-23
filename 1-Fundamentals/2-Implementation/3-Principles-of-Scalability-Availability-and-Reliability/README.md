## **Principles of Scalability, Availability, and Reliability**

1.  **[Scalability](./scalability.md)**:
    -   Implement a load balancer using tools like Nginx or HAProxy to distribute traffic.
    -   Create a script to simulate increasing traffic and observe how horizontal scaling affects performance.
2.  **[Availability](./availability.md)**:
    -   Design and implement a failover mechanism for a database using read replicas.
    -   Perform manual failover testing to ensure availability during primary database downtime.
3.  **[Reliability](./reliability.md)**:
    -   Implement a retry mechanism in an HTTP client to handle transient failures.
    -   Add idempotency logic to an API endpoint to ensure consistent behavior during retries.