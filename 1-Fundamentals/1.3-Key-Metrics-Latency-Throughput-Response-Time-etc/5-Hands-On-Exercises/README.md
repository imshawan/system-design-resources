## Hands-On Exercises: Simulating Latency and Throughput Scenarios

These exercises guide you through simulating latency and throughput in a system using **HTTP requests under load**. You’ll use tools like **Apache JMeter** or **k6** for load testing.

Simulating latency and throughput scenarios involves setting up an HTTP server to handle requests and generating load using tools like Apache JMeter or k6. Below is a step-by-step guide using **Java** for the server and explanations of how to perform load testing.

----------

### **Step 1: Create a Simple HTTP Server in Java**

We'll use **Spring Boot** to create an HTTP server with endpoints that simulate latency and throughput scenarios.

#### **Java Code**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
public class LatencyThroughputSimulationApplication {

    public static void main(String[] args) {
        SpringApplication.run(LatencyThroughputSimulationApplication.class, args);
    }
}

@RestController
class SimulationController {

    // Endpoint to simulate minimal latency
    @GetMapping("/low-latency")
    public String lowLatencyEndpoint() {
        return "Response from low-latency endpoint!";
    }

    // Endpoint to simulate high latency
    @GetMapping("/high-latency")
    public String highLatencyEndpoint() throws InterruptedException {
        // Simulating delay
        Thread.sleep(500); // 500ms artificial delay
        return "Response from high-latency endpoint!";
    }

    // Endpoint to simulate high throughput
    @GetMapping("/high-throughput")
    public String highThroughputEndpoint() {
        return "Response from high-throughput endpoint!";
    }
}
```

#### **Setup Explanation**

1.  `/low-latency`: Quick response to simulate minimal processing delay.
2.  `/high-latency`: Artificial delay (500ms) to simulate network or processing latency.
3.  `/high-throughput`: Lightweight response optimized for high concurrency.

----------

### **Step 2: Load Testing with Apache JMeter**

#### **Install JMeter**

1.  Download [Apache JMeter](https://jmeter.apache.org/).
2.  Extract and run the `jmeter` script.

#### **Configure Load Test**

1.  Open JMeter.
2.  Add a **Thread Group**:
    -   Specify the number of users (threads), ramp-up time, and loop count.
    -   Example: 50 users, ramp-up time 10 seconds.
3.  Add an **HTTP Request Sampler**:
    -   Configure it to target the endpoints (`/low-latency`, `/high-latency`, `/high-throughput`).
4.  Add a **Summary Report** or **Graph Results** listener.
5.  Run the test.

----------

### **Step 3: Load Testing with k6**

#### **Install k6**

1.  Download k6 from [k6.io](https://k6.io/).
2.  Install using package managers like `brew` (Mac) or `choco` (Windows).

#### **Write k6 Test Script**

Create a file `test.js`:

```javascript
import http from 'k6/http';
import { check } from 'k6';

export let options = {
    stages: [
        { duration: '10s', target: 10 },  // Ramp up to 10 users
        { duration: '30s', target: 50 }, // Stay at 50 users
        { duration: '10s', target: 0 },  // Ramp down
    ],
};

export default function () {
    let response = http.get('http://localhost:8080/high-latency'); // Change endpoint here
    check(response, { 'status is 200': (r) => r.status === 200 });
}
```

#### **Run the Test**

Execute the script with:

```bash
k6 run test.js
```


----------

### **Step 4: Analyze Results**

#### **Metrics to Monitor**

1.  **Latency**:
    -   Check average and 95th/99th percentile response times.
2.  **Throughput**:
    -   Requests per second (RPS) handled by the server.

#### **Expected Behavior**

-   **Low-latency endpoint**: Should show minimal response time under load.
-   **High-latency endpoint**: Should demonstrate consistent delays.
-   **High-throughput endpoint**: Should handle more RPS compared to high-latency.

----------

### **Step 5: Extend with Observability**

To monitor your server during testing:

1.  **Add Logging**: Use a logging library like Logback to capture request details.
2.  **Integrate Metrics**: Use a tool like **Micrometer** to expose metrics to Prometheus or a similar system.