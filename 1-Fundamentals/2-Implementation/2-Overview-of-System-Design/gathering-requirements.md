## **Gathering Requirements**

When building a system like a URL shortener, it is essential to define clear functional and non-functional requirements to guide the development process. This step ensures the system addresses the needs of its users while adhering to performance, scalability, and reliability constraints.

----------

#### **Functional Requirements**

These define what the system is supposed to do from the user's perspective. For a URL shortener, key functional requirements include:

1.  **Shortening URLs**
    
    -   The system should take a long URL (e.g., `https://example.com/very/long/url`) and generate a short, unique identifier (e.g., `https://short.ly/abc123`).
2.  **Redirecting to Original URLs**
    
    -   When a user accesses the shortened URL, the system should redirect them to the original URL.
3.  **Custom Shortened URLs** (Optional)
    
    -   Users may want to create custom short links like `https://short.ly/my-link`.
4.  **Analytics Tracking** (Optional)
    
    -   The system can record data such as how many times a shortened URL was accessed, the location of users, and the device used.
5.  **User Management** (Optional)
    
    -   Users can create accounts to view and manage their shortened URLs.

----------

#### **Non-Functional Requirements**

These define the performance, scalability, and reliability characteristics of the system.

1.  **Scalability**
    
    -   The system should handle high traffic, such as millions of URL redirections per day.
    -   It should support horizontal scaling, allowing additional servers to handle increased traffic.
2.  **High Availability**
    
    -   The system should be available 99.9% of the time, ensuring users can access their URLs without disruptions.
3.  **Low Latency**
    
    -   URL redirection should occur in less than 100ms for a seamless user experience.
4.  **Data Durability**
    
    -   Once a URL is shortened, it should persist in the database even if a server crashes.
5.  **Security**
    
    -   Prevent malicious activities like spamming or phishing by implementing validation and rate-limiting.
6.  **Global Accessibility**
    
    -   The service should work seamlessly across multiple geographic locations, with minimal latency for global users.

----------

#### **Identifying Constraints**

Understanding the constraints early ensures the system design remains practical and efficient.

1.  **Data Storage**
    
    -   URLs and associated metadata (e.g., click counts, timestamps) must be stored efficiently. Using key-value databases like Redis or DynamoDB is common.
2.  **Uniqueness of Short URLs**
    
    -   Shortened URLs must be unique. Collision-resistant algorithms or hashing functions should ensure this.
3.  **Scalability Challenges**
    
    -   As the system grows, managing a massive number of records (URLs) while maintaining fast lookups becomes challenging.
4.  **Concurrency**
    
    -   Multiple users may shorten URLs simultaneously, requiring mechanisms to prevent conflicts.
5.  **Handling Expiry (Optional)**
    
    -   If URLs are allowed to expire, the system should efficiently remove or archive them.
6.  **Budgetary Constraints**
    
    -   The system should operate within the allocated budget for infrastructure and maintenance.

----------

#### **Example Metrics for Constraints**
| Metric              | Requirement                     | Approach                                    |
|----------------------|---------------------------------|--------------------------------------------|
| Response Time        | < 100ms                        | Use caching (e.g., CDN, Redis).            |
| Throughput           | 10 million redirects/day       | Scale horizontally with replicas.          |
| Storage              | 10 TB for metadata storage     | Use distributed databases.                 |
| Downtime Allowance   | < 44 minutes/month (99.9% uptime) | Implement redundancy and failover.         |
| Security             | Block spam and prevent phishing | Implement validation and CAPTCHA.          |
