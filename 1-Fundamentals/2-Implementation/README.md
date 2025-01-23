
## **Drawing and Planning**

Visualizing concepts like **CAP Theorem**, **PACELC**, and system metrics is essential to understanding their trade-offs and practical implications. Below, we’ll create diagrams and flowcharts to explain these concepts and help map system behaviors.

----------

### **1. CAP Theorem Trade-Off Diagram**

#### **Explanation**

The CAP theorem states that in a distributed system, you can guarantee only two of the following three properties simultaneously:

1.  **Consistency (C):** All nodes see the same data at the same time.
2.  **Availability (A):** Every request gets a response, even if some nodes are down.
3.  **Partition Tolerance (P):** The system continues to function despite network partitions.

#### **Diagram Description**

A triangle labeled with **Consistency**, **Availability**, and **Partition Tolerance** at the vertices. Each edge shows the trade-offs when you focus on two properties:

-   Consistency + Availability = No partition tolerance.
-   Availability + Partition Tolerance = Weaker consistency.
-   Consistency + Partition Tolerance = Limited availability.

----------

### **Flowchart Code for CAP Diagram**

```mermaid
graph TD
    A[CAP Theorem]
    C[Consistency]
    P[Partition Tolerance]
    A2[Availability]

    A --> C
    A --> P
    A --> A2

    subgraph Trade-Offs
        C -- "No Partition Tolerance" --> A2
        A2 -- "Weaker Consistency" --> P
        P -- "Limited Availability" --> C
    end
 ``` 

----------

### **2. PACELC Theorem Diagram**

#### **Explanation**

PACELC extends CAP by adding **latency trade-offs** when there is no partition:

-   **Partition:** During a network partition, you must choose between **availability** and **consistency**.
-   **Else:** When there is no partition, you must choose between **latency** and **consistency**.

#### **Diagram Description**

The diagram is a decision flow:

1.  **Is there a network partition?**
    -   If yes, choose between **availability** and **consistency** (CAP).
    -   If no, choose between **latency** and **consistency** (ELC).

----------

### **Flowchart Code for PACELC Diagram**

```mermaid
graph TD
    A[Start]
    B[Network Partition?]
    C[Availability]
    D[Consistency]
    E[Latency]

    A --> B
    B -- Yes --> C
    B -- Yes --> D
    B -- No --> E
    E --> D
   ```

----------

### **3. Mapping System Metrics: Latency and Throughput Flow**

#### **Explanation**

This flowchart illustrates how **latency** and **throughput** are impacted by architectural decisions in a distributed system:

1.  **Read/Write Requests** → Enter the system.
2.  If **partition** occurs, trade-offs affect consistency or availability.
3.  **Latency** increases if consistency is prioritized.
4.  **Throughput** improves if availability is prioritized.

----------

### **Flowchart Code for Metrics Mapping**

```mermaid
graph TD
    A[Client Request]
    B[Distributed System]
    C[Network Partition?]
    D[Increase Latency]
    E[Improve Throughput]
    F[Consistency Trade-Off]
    G[Availability Trade-Off]

    A --> B
    B --> C
    C -- Yes --> F
    C -- No --> G
    F --> D
    G --> E
```
