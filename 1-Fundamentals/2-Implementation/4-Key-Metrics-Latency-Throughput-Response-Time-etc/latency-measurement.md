## **Latency Measurement in Distributed Systems**

Measuring latency is crucial for understanding the performance of a system, identifying bottlenecks, and ensuring a good user experience. Latency refers to the time taken for a request to travel from the client to the server and back.

----------

### **1. Sending HTTP Requests and Measuring Response Times**

To measure response times, we can send multiple HTTP requests to a mock server and record the latency for each request. Here's a Python script using the `requests` and `matplotlib` libraries:

#### **Python Script for Measuring Latency**

```python
import requests
import time
import matplotlib.pyplot as plt

# Mock server endpoint
URL = "https://httpbin.org/delay/1"  # A mock endpoint with a 1-second delay

# Function to measure latency
def measure_latency(url, num_requests=100):
    latencies = []
    for i in range(num_requests):
        start_time = time.time()
        try:
            response = requests.get(url, timeout=5)  # Send request
            response.raise_for_status()
        except requests.exceptions.RequestException as e:
            print(f"Request {i + 1} failed: {e}")
            latencies.append(None)  # Mark failed request
        else:
            end_time = time.time()
            latencies.append(end_time - start_time)
    return latencies

# Measure latencies
latency_results = measure_latency(URL, num_requests=100)

# Filter out failed requests (None values)
valid_latencies = [lat for lat in latency_results if lat is not None]

# Output basic metrics
print(f"Number of requests: {len(latency_results)}")
print(f"Successful requests: {len(valid_latencies)}")
print(f"Average latency: {sum(valid_latencies) / len(valid_latencies):.3f} seconds")
print(f"P99 latency: {sorted(valid_latencies)[int(len(valid_latencies) * 0.99) - 1]:.3f} seconds")

# Save raw latencies to a file (optional)
with open("latency_data.txt", "w") as f:
    for lat in valid_latencies:
        f.write(f"{lat}\n")
```

----------

### **2. Visualizing Latency Distribution**

Once we have the latency data, we can visualize the distribution using a histogram or percentiles to understand the system's performance under varying conditions.

#### **Plotting the Latency Distribution**

```python
# Plot histogram of latencies
plt.figure(figsize=(10, 6))
plt.hist(valid_latencies, bins=20, color='blue', alpha=0.7, edgecolor='black')
plt.title("Latency Distribution")
plt.xlabel("Latency (seconds)")
plt.ylabel("Frequency")
plt.grid(axis="y", linestyle="--", alpha=0.7)
plt.show()
```

#### **Percentiles for Performance Metrics**

Percentiles (e.g., P50, P90, P99) are critical for understanding performance under different loads:

-   **P50**: Median latency (50% of requests are faster than this value).
-   **P90**: Indicates high-load performance (90% of requests are faster).
-   **P99**: Shows the latency for nearly all requests (99% are faster).

**Example Code for Percentiles**:

```python
def calculate_percentiles(latencies, percentiles=[50, 90, 99]):
    sorted_latencies = sorted(latencies)
    results = {}
    for p in percentiles:
        index = int(len(sorted_latencies) * (p / 100)) - 1
        results[f"P{p}"] = sorted_latencies[index]
    return results

percentile_results = calculate_percentiles(valid_latencies)
print("Latency Percentiles:", percentile_results)
```

----------

### **3. Observations and Insights**

-   **Average Latency**: Gives a general idea of the system’s performance under normal conditions.
-   **P99 Latency**: Crucial for understanding the worst-case scenarios.
-   **Histogram**: Helps identify anomalies or irregular patterns (e.g., a spike in high latencies).

----------

### **Sample Output and Visualization**

After running the script:

-   **Average latency**: 1.05 seconds
-   **P99 latency**: 1.21 seconds

**Histogram Visualization**: The histogram shows most requests complete within 1-1.2 seconds, with occasional outliers.

----------

### **Use Cases of Latency Measurement**

-   **Performance Testing**: Identify bottlenecks before deployment.
-   **Capacity Planning**: Prepare for expected traffic patterns.
-   **SLAs and SLOs**: Ensure compliance with latency-based agreements.

This process allows teams to proactively identify performance issues and optimize system reliability.