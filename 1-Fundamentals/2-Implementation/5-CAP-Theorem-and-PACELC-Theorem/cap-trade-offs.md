## **CAP Trade-offs in Distributed Systems: Setting Up, Simulating, and Demonstrating**

Understanding and observing the CAP theorem in action provides valuable insights into the trade-offs distributed systems must make between **Consistency**, **Availability**, and **Partition Tolerance**. In this exercise, we'll set up a distributed key-value store, simulate network partitions, and observe the behavior of the system under different configurations.

----------

## **1. Setting Up a Distributed Key-Value Store**

### **Choosing the Tool**

We use **Etcd**, a distributed key-value store, because of its strong consistency guarantees and ease of setup. It helps demonstrate how systems handle CAP trade-offs.

----------

### **Install and Configure Etcd**

#### **Step 1: Download Etcd**

1.  Visit the [Etcd GitHub Releases page](https://github.com/etcd-io/etcd/releases) and download the latest release for your operating system.
2.  Extract the binary:
    
    ```bash
    tar -xvf etcd-v*.tar.gz
    cd etcd-v*
    sudo mv etcd etcdctl /usr/local/bin/
    ```
    

#### **Step 2: Start a Multi-Node Etcd Cluster**

1.  Start the first node:
    
    ```bash
    etcd --name node1 \
         --initial-advertise-peer-urls http://localhost:2380 \
         --listen-peer-urls http://localhost:2380 \
         --listen-client-urls http://localhost:2379,http://127.0.0.1:2379 \
         --advertise-client-urls http://localhost:2379 \
         --initial-cluster node1=http://localhost:2380
     ```
    
2.  Start a second node:
    
    ```bash
    etcd --name node2 \
         --initial-advertise-peer-urls http://<NODE2_IP>:2380 \
         --listen-peer-urls http://<NODE2_IP>:2380 \
         --listen-client-urls http://<NODE2_IP>:2379 \
         --advertise-client-urls http://<NODE2_IP>:2379 \
         --initial-cluster node1=http://localhost:2380,node2=http://<NODE2_IP>:2380
     ```
    
3.  Verify the cluster:
    
    ```bash
    etcdctl --endpoints=http://localhost:2379 endpoint status
    ```
    

----------

## **2. Simulating a Network Partition**

A **network partition** occurs when communication between cluster nodes is disrupted. We'll simulate this using firewall rules.

#### **Step 1: Simulate Partition**

On the machine hosting `node1`, run:

```bash
sudo iptables -A INPUT -s <NODE2_IP> -j DROP
sudo iptables -A OUTPUT -d <NODE2_IP> -j DROP
```

#### **Step 2: Verify Partition**

Try writing to the cluster:

```bash
etcdctl --endpoints=http://localhost:2379 put key1 "value1"
```

Then read the value on the second node:

```bash
etcdctl --endpoints=http://<NODE2_IP>:2379 get key1
```

----------

## **3. Observing Behavior**

#### **Consistency Configuration**

If **Consistency** is prioritized:

-   Writes to the partitioned node will fail to maintain data integrity.
-   Observed behavior:
    
    ```bash
    Error: context deadline exceeded
    ``` 
    

#### **Availability Configuration**

If **Availability** is prioritized:

-   Writes to the partitioned node will succeed, but data may become temporarily inconsistent.
-   Observed behavior:
    -   Write succeeds on `node1`.
    -   Reading from `node2` returns no data or stale data.

----------

## **4. Working Demonstration**

### **Consistency Priority**

1.  Ensure strong consistency by configuring `--election-timeout` and `--heartbeat-interval` parameters in Etcd.
2.  Write a key-value pair during a partition:
    
    ```bash
    etcdctl --endpoints=http://localhost:2379 put key1 "value1"
    ```
    
3.  Try reading from a partitioned node:
    
    ```bash
    etcdctl --endpoints=http://<NODE2_IP>:2379 get key1
    ```
    
    -   The read will fail until the partition is resolved.

----------

### **Availability Priority**

1.  Configure Etcd to allow writes during a partition by reducing quorum requirements (not recommended for production).
2.  Write a key-value pair:
    
    ```bash
    etcdctl --endpoints=http://localhost:2379 put key1 "value1"
    ```
    
3.  Try reading from the second node:
    -   The key may be unavailable or return stale data until reconciliation occurs.

----------

## **5. Resolving the Partition**

Restore communication between nodes:

```bash
sudo iptables -D INPUT -s <NODE2_IP> -j DROP
sudo iptables -D OUTPUT -d <NODE2_IP> -j DROP
```

Observe that:

-   Writes on both nodes are reconciled.
-   Data consistency is restored after resolution.

----------

## **6. Visualizing CAP Behavior**

#### **Diagram: Consistency vs Availability**

```mermaid
graph TD
    A[Network Partition Occurs]
    A --> B{Consistency First?}
    B -- Yes --> C[Reject writes]
    B -- No --> D[Accept writes]
    C --> E[Data remains consistent]
    D --> F[Temporary inconsistency]
    F --> G[Reconcile after partition resolved]
    E --> G
   ```

----------

## **7. Practical Use Case Insights**

-   **Consistency-Oriented Systems**:
    -   Suitable for critical applications like banking or inventory management.
-   **Availability-Oriented Systems**:
    -   Ideal for applications like social media where data freshness is less critical.

----------

### **Code: Observing CAP in Action**

#### Write a Test Script

This Python script tests consistency and availability during a partition:

```python
import subprocess
import time
import etcd3

etcd = etcd3.client()

def write_key(key, value):
    try:
        etcd.put(key, value)
        print(f"Write success: {key} -> {value}")
    except Exception as e:
        print(f"Write failed: {e}")

def read_key(key):
    try:
        value, _ = etcd.get(key)
        print(f"Read success: {key} -> {value.decode() if value else 'None'}")
    except Exception as e:
        print(f"Read failed: {e}")

# Write during normal operation
write_key("test_key", "value1")

# Simulate partition
subprocess.run(["sudo", "iptables", "-A", "INPUT", "-s", "NODE2_IP", "-j", "DROP"])
subprocess.run(["sudo", "iptables", "-A", "OUTPUT", "-d", "NODE2_IP", "-j", "DROP"])
time.sleep(2)

# Attempt to write and read during partition
write_key("test_key", "value2")
read_key("test_key")

# Resolve partition
subprocess.run(["sudo", "iptables", "-D", "INPUT", "-s", "NODE2_IP", "-j", "DROP"])
subprocess.run(["sudo", "iptables", "-D", "OUTPUT", "-d", "NODE2_IP", "-j", "DROP"])
time.sleep(2)

# Verify consistency
read_key("test_key")
```

----------

## **8. Key Observations**

1.  **Trade-offs are unavoidable**: You cannot achieve all three CAP properties simultaneously during a partition.
2.  **Practical configurations**: Systems like Etcd favor consistency, while others like DynamoDB prioritize availability.
3.  **Monitoring and tuning**: Always monitor performance and adjust configurations based on your application's needs.

This hands-on exercise deepens your understanding of CAP trade-offs and equips you with tools to make informed architectural decisions.