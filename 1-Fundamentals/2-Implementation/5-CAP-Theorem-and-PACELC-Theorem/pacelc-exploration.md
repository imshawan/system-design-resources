## **PACELC Exploration: Latency vs Consistency Trade-offs**

The PACELC theorem extends the CAP theorem by considering latency trade-offs in distributed systems during normal operations. It states that even when there is no network partition, a system must choose between **consistency** and **latency**. This exercise demonstrates the latency-consistency trade-offs in a NoSQL database like **DynamoDB** or **MongoDB** using both **strongly consistent** and **eventually consistent** reads.

----------

## **1. Setting Up the Environment**

### **Using DynamoDB**

#### **Step 1: Install AWS CLI and SDK**

1.  Install AWS CLI:
    
    ```bash
	sudo apt update
    sudo apt install awscli -y
	```
    
2.  Configure AWS credentials:
    
    ```bash
    aws configure
    ```
    
    -   Provide your access key, secret key, region, and output format.
3.  Install the AWS SDK for Python (Boto3):
    
    ```bash
    pip install boto3
    ```
    

#### **Step 2: Create a DynamoDB Table**

1.  Create a table with a primary key (`id`):
    
    ```bash
    aws dynamodb create-table \
        --table-name TestTable \
        --attribute-definitions AttributeName=id,AttributeType=S \
        --key-schema AttributeName=id,KeyType=HASH \
        --billing-mode PAY_PER_REQUEST
    ```
    
2.  Insert test data:
    
    ```bash
    aws dynamodb put-item \
        --table-name TestTable \
        --item '{"id": {"S": "1"}, "value": {"S": "Hello, World!"}}'
    ```

----------

## **2. Demonstrate Latency vs Consistency Trade-offs**

### **Testing Strongly Consistent Reads**

A strongly consistent read ensures that the data returned reflects all previous write operations, guaranteeing the most up-to-date value.

#### **Python Code for Strongly Consistent Read**

```python
import boto3
import time

dynamodb = boto3.client('dynamodb')

def strong_consistent_read():
    start_time = time.time()
    response = dynamodb.get_item(
        TableName='TestTable',
        Key={'id': {'S': '1'}},
        ConsistentRead=True  # Strong consistency
    )
    latency = time.time() - start_time
    print(f"Strong Read Latency: {latency:.4f}s")
    print("Data:", response.get('Item'))

strong_consistent_read()
```

----------

### **Testing Eventually Consistent Reads**

An eventually consistent read may return stale data, as it doesn’t guarantee the latest write is reflected.

#### **Python Code for Eventually Consistent Read**

```python
def eventual_consistent_read():
    start_time = time.time()
    response = dynamodb.get_item(
        TableName='TestTable',
        Key={'id': {'S': '1'}},
        ConsistentRead=False  # Eventual consistency
    )
    latency = time.time() - start_time
    print(f"Eventual Read Latency: {latency:.4f}s")
    print("Data:", response.get('Item'))

eventual_consistent_read()
```

----------

## **3. Experiment: Measuring Latency**

### **Simulating Traffic**

To test latency under load, use a tool like **Apache JMeter** or Python’s `concurrent.futures` to send multiple read requests concurrently.

#### **Python Code for Concurrent Reads**

```python
from concurrent.futures import ThreadPoolExecutor

def measure_latency(consistency_type):
    def read_request():
        response = dynamodb.get_item(
            TableName='TestTable',
            Key={'id': {'S': '1'}},
            ConsistentRead=consistency_type == 'strong'
        )
        return time.time()

    with ThreadPoolExecutor(max_workers=10) as executor:
        start_times = [time.time() for _ in range(10)]
        results = executor.map(read_request, start_times)
        latencies = [end - start for start, end in zip(start_times, results)]
        print(f"{consistency_type.capitalize()} Consistency Avg Latency: {sum(latencies)/len(latencies):.4f}s")

measure_latency('strong')
measure_latency('eventual')
```

----------

## **4. Reporting the Results**

### **Latency Observations**

Run the above code multiple times and collect latency metrics for both consistency types.
| Consistency Type      | Avg Latency (ms) | Behavior During Reads      |
|-----------------------|------------------|----------------------------|
| Strong Consistency    | ~30-50 ms       | Returns the latest data    |
| Eventual Consistency  | ~10-20 ms       | May return stale data      |


----------

## **5. Visualizing Results**

### **Latency Histogram**

Visualize the latency distribution using Python’s `matplotlib`:

```python
import matplotlib.pyplot as plt

strong_latencies = [30, 32, 35, 50, 45]  # Replace with actual data
eventual_latencies = [10, 12, 15, 18, 20]

plt.hist(strong_latencies, bins=5, alpha=0.7, label='Strong Consistency')
plt.hist(eventual_latencies, bins=5, alpha=0.7, label='Eventual Consistency')
plt.xlabel('Latency (ms)')
plt.ylabel('Frequency')
plt.title('Latency Comparison: Strong vs Eventual Consistency')
plt.legend()
plt.show()
```

----------

## **6. MongoDB Alternative**

For MongoDB, similar tests can be conducted with **read concern levels**:

-   **Strong consistency**: Use `majority` read concern.
-   **Eventual consistency**: Use `local` or default read concern.

----------

## **7. Insights from the Experiment**

1.  **Strong Consistency**:
    
    -   Guarantees the latest data but incurs higher latency due to synchronization overhead.
    -   Use Cases: Banking, inventory systems, and any application requiring real-time accuracy.
2.  **Eventual Consistency**:
    
    -   Reduces latency by allowing stale reads.
    -   Use Cases: Social media, recommendation systems, and analytics.