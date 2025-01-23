## **Hands-On Exercises for Consistency Models**

This section provides practical exercises to help you understand the concepts of **strong consistency**, **eventual consistency**, and **causal consistency** in distributed systems. By simulating these models and implementing a simple key-value store, you’ll gain hands-on experience with the trade-offs and behaviors of different consistency guarantees.

----------

#### **Exercise 1: Simulate Strong, Eventual, and Causal Consistency**

##### **Objective**

Understand the behaviors of strong, eventual, and causal consistency by simulating a distributed environment with multiple nodes.

##### **Setup**

1.  Use a programming language like Python, JavaScript, or Go to create a simulation.
2.  Create a cluster of **nodes** representing a distributed system. Each node will hold its copy of the data.

----------

##### **Part 1: Simulate Strong Consistency**

-   **Concept**: All replicas in the system must agree on the data state immediately after any update.
    
-   **Steps**:
    
    1.  Implement a **leader-based replication system** where one node acts as the leader.
    2.  When a client writes data, the leader processes the write and synchronizes it to all replicas before acknowledging the client.
    3.  Block reads and writes during synchronization to ensure consistency.
-   **Example Code Snippet (Python)**:
    

```python
class StrongConsistencyNode:
    def __init__(self):
        self.data = {}
    
    def write(self, key, value, replicas):
        # Synchronize with replicas before confirming
        for replica in replicas:
            replica.data[key] = value
        self.data[key] = value
        print(f"Write complete: {key} -> {value}")
    
    def read(self, key):
        return self.data.get(key, "Key not found")

# Simulate leader and replicas
leader = StrongConsistencyNode()
replicas = [StrongConsistencyNode() for _ in range(2)]
leader.write("x", 10, replicas)  # Strong consistency enforced
print(replicas[0].read("x"))  # Reads 10
```

----------

##### **Part 2: Simulate Eventual Consistency**

-   **Concept**: Updates are eventually propagated to all replicas, but they might return stale data temporarily.
    
-   **Steps**:
    
    1.  Implement a **lazy propagation mechanism** where updates are asynchronously sent to replicas.
    2.  Allow clients to read inconsistent data while waiting for synchronization.
-   **Example Code Snippet (Python)**:
    

```python
import time

class EventualConsistencyNode:
    def __init__(self):
        self.data = {}
    
    def write(self, key, value, replicas):
        # Update local data
        self.data[key] = value
        print(f"Write initiated: {key} -> {value}")
        # Asynchronously propagate updates
        for replica in replicas:
            time.sleep(1)  # Simulate delay
            replica.data[key] = value
            print(f"Replica updated: {key} -> {value}")
    
    def read(self, key):
        return self.data.get(key, "Key not found")

# Simulate nodes
node1 = EventualConsistencyNode()
replicas = [EventualConsistencyNode() for _ in range(2)]
node1.write("y", 20, replicas)  # Eventual propagation
print(replicas[0].read("y"))  # Initially returns "Key not found"
time.sleep(2)
print(replicas[0].read("y"))  # Eventually returns 20
```

----------

##### **Part 3: Simulate Causal Consistency**

-   **Concept**: Operations with causal dependencies respect their order across replicas, while independent operations can be unordered.
    
-   **Steps**:
    
    1.  Track causal dependencies using a **version vector**.
    2.  Delay updates until all dependencies are satisfied.
-   **Example Code Snippet (Python)**:
    

```python
class CausalConsistencyNode:
    def __init__(self):
        self.data = {}
        self.version_vector = {}
    
    def write(self, key, value, version, replicas):
        # Update version vector
        self.version_vector[key] = version
        self.data[key] = value
        print(f"Write complete: {key} -> {value} at version {version}")
        # Propagate to replicas
        for replica in replicas:
            replica.receive_update(key, value, version)
    
    def receive_update(self, key, value, version):
        # Apply only if causally consistent
        if self.version_vector.get(key, 0) < version:
            self.data[key] = value
            self.version_vector[key] = version
            print(f"Replica updated: {key} -> {value} at version {version}")
    
    def read(self, key):
        return self.data.get(key, "Key not found")

# Simulate nodes
node1 = CausalConsistencyNode()
replicas = [CausalConsistencyNode() for _ in range(2)]
node1.write("z", 30, 1, replicas)  # Write with version
print(replicas[0].read("z"))  # Read after propagation
```

----------

#### **Exercise 2: Implement a Simple Key-Value Store with Eventual Consistency**

##### **Objective**

Build a distributed key-value store where updates propagate asynchronously to replicas.

##### **Steps**:

1.  **Setup**: Create a key-value store with multiple replicas.
2.  **Write Mechanism**: Write operations update the local replica first and propagate updates asynchronously.
3.  **Read Mechanism**: Reads may return stale data until synchronization is complete.
4.  **Conflict Resolution**: Use techniques like **last-write-wins (LWW)** or **timestamps** to resolve conflicting updates.

##### **Example Code Snippet (Python)**:

```python
import threading

class KeyValueStore:
    def __init__(self):
        self.data = {}
    
    def write(self, key, value, replicas):
        self.data[key] = value
        print(f"Local write: {key} -> {value}")
        # Asynchronously propagate to replicas
        for replica in replicas:
            threading.Thread(target=replica.update, args=(key, value)).start()
    
    def update(self, key, value):
        self.data[key] = value
        print(f"Replica updated: {key} -> {value}")
    
    def read(self, key):
        return self.data.get(key, "Key not found")

# Create nodes
node1 = KeyValueStore()
replicas = [KeyValueStore() for _ in range(2)]
node1.write("a", 100, replicas)
print(replicas[0].read("a"))  # Stale read
```

----------

#### **Discussion on Results**

1.  **Strong Consistency**:
    
    -   Every read returns the latest data, but write operations may block until replicas are synchronized.
    -   Best for banking or financial systems where correctness is critical.
2.  **Eventual Consistency**:
    
    -   Updates propagate slowly, leading to stale reads temporarily.
    -   Suitable for social media or product catalog systems where immediate consistency is not essential.
3.  **Causal Consistency**:
    
    -   Updates respecting causality ensure a coherent user experience without requiring strict global synchronization.
    -   Ideal for collaboration tools or version control systems.

----------

#### **Key Takeaways**

-   These exercises help visualize the trade-offs between **latency, availability, and consistency**.
-   Distributed systems must balance these factors based on specific application requirements.
-   Practical implementation of consistency models deepens understanding and prepares developers to design scalable and efficient distributed systems.