## **Key Metrics: Latency, Throughput, Response Time**

1.  **[Latency Measurement](./latency-measurement.md)**:
    -   Write a script to send HTTP requests to a mock server and measure response times.
    -   Visualize latency distribution (e.g., using histograms or percentiles like P99).
2.  **[Throughput Analysis](./throughput-analysis.md)**:
    -   Simulate a workload with multiple concurrent requests using a tool like Apache JMeter or k6.
    -   Measure the maximum requests per second (RPS) the system can handle before latency degrades.
3.  **[Response Time Optimization](./response-time-optimization.md)**:
    -   Introduce caching at the application layer (e.g., with Redis) to reduce response times.
    -   Compare response times with and without caching enabled.