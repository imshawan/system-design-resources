## **Eventual Consistency Simulation**

**Eventual consistency** is a consistency model used in distributed systems where updates to data are propagated to all nodes, but not immediately. Over time, and in the absence of additional updates, all nodes eventually converge to the same state.

This approach prioritizes **availability** and **partition tolerance** (AP systems in the CAP theorem), making it ideal for large-scale, distributed databases like **Cassandra** or **DynamoDB**.

----------

### **1. Building a Distributed Key-Value Store**

We’ll simulate eventual consistency by:

1.  Creating multiple nodes representing a distributed system.
2.  Implementing an asynchronous replication mechanism for propagating updates.
3.  Testing data convergence across nodes.

#### **Setting Up the Environment**

Install Python and required libraries:

```bash
pip install flask requests threading
```

#### **Node Implementation**

Each node in our distributed key-value store will:

1.  Store data in memory.
2.  Handle writes locally and propagate changes to other nodes asynchronously.

Here’s the Python code for a single node:

```python
from flask import Flask, request, jsonify
import threading
import requests
import time

app = Flask(__name__)

# In-memory key-value store
store = {}

# List of other nodes in the system
peers = []

# Endpoint to write data to the store
@app.route('/write', methods=['POST'])
def write():
    data = request.get_json()
    key = data['key']
    value = data['value']
    
    # Write to local store
    store[key] = value
    print(f"Updated local store: {store}")
    
    # Propagate changes asynchronously
    threading.Thread(target=propagate_update, args=(key, value)).start()
    
    return jsonify({"status": "success", "message": f"Key {key} updated"}), 200

# Endpoint to replicate data from another node
@app.route('/replicate', methods=['POST'])
def replicate():
    data = request.get_json()
    key = data['key']
    value = data['value']
    
    # Update the local store
    store[key] = value
    print(f"Replicated {key}: {value}. Current store: {store}")
    
    return jsonify({"status": "success"}), 200

# Propagate update to all peer nodes
def propagate_update(key, value):
    for peer in peers:
        try:
            requests.post(f"{peer}/replicate", json={"key": key, "value": value})
        except Exception as e:
            print(f"Failed to replicate to {peer}: {e}")

if __name__ == '__main__':
    import sys
    port = int(sys.argv[1])
    if len(sys.argv) > 2:
        peers.extend(sys.argv[2:])
    
    print(f"Starting node on port {port} with peers {peers}")
    app.run(port=port, debug=True)
```

----------

### **2. Running the Simulation**

#### **Step 1: Start Nodes**

Run multiple instances of the script with different ports to simulate nodes:

```bash
python3 node.py 5000 http://localhost:5001 http://localhost:5002
python3 node.py 5001 http://localhost:5000 http://localhost:5002
python3 node.py 5002 http://localhost:5000 http://localhost:5001
```

Each node is aware of its peers through the command-line arguments.

#### **Step 2: Perform Writes**

Send a write request to one node:

```bash
curl -X POST -H "Content-Type: application/json" -d '{"key": "name", "value": "Alice"}' http://localhost:5000/write
```

#### **Step 3: Verify Convergence**

Check the data on all nodes:

```bash
curl http://localhost:5000/replicate
curl http://localhost:5001/replicate
curl http://localhost:5002/replicate
``` 

All nodes should eventually have the same key-value pair (`{"name": "Alice"}`).

----------

### **3. Observing Eventual Consistency**

1.  **Write Propagation Delay**:
    
    -   Writes to a node are immediately visible locally.
    -   Other nodes may take time to update because propagation happens asynchronously.
2.  **Failure Recovery**:
    
    -   If a node is temporarily unavailable during propagation, it won’t receive the update.
    -   When the node comes back online, subsequent writes or re-syncing mechanisms can ensure eventual convergence.

----------

### **4. Enhancing the Simulation**

To make this more robust:

-   Implement **vector clocks** or **timestamps** to handle conflicting writes.
-   Introduce a background thread to periodically reconcile state between nodes.

----------

### **5. Summary**

In this simulation:

-   Data consistency is achieved **eventually**, but there’s no guarantee of immediate synchronization.
-   This reflects real-world eventual consistency systems like **DynamoDB** and **Cassandra**, which optimize for high availability and partition tolerance.

----------

### **Visualization of Data Flow**

Here’s a **Diagram** showing the propagation of writes in our eventual consistency system:

```mermaid
graph TD
    Client --> Node1[Node 1]
    Client --> Node2[Node 2]
    Client --> Node3[Node 3]
    Node1 -- Async Replication --> Node2
    Node1 -- Async Replication --> Node3
    Node2 -- Async Replication --> Node1
    Node2 -- Async Replication --> Node3
    Node3 -- Async Replication --> Node1
    Node3 -- Async Replication --> Node2
 ```

This demonstrates how nodes exchange updates to ensure all eventually converge to the same state.