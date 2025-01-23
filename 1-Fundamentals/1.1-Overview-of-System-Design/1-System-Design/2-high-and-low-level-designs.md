## **1. High-Level Design (HLD)**

**Purpose**: Focuses on the overall structure and architecture of the system. It provides a bird’s-eye view of how the system will function and interact with external systems.

### **Key Characteristics of HLD:**

-   **Scope**: Describes the system as a whole and its major components.
-   **Abstraction**: Provides a conceptual overview without diving into implementation details.
-   **Audience**: Primarily targeted at stakeholders, project managers, architects, and senior developers.
-   **Output**:
    -   Architecture diagrams
    -   Data flow diagrams
    -   Component relationships (e.g., services, databases, APIs)
    -   Technology stack decisions
-   **Focus Areas**:
    -   **System Architecture**: Defines how major components like databases, load balancers, and APIs will interact.
    -   **Scalability**: Addresses horizontal/vertical scaling and capacity planning.
    -   **Communication Protocols**: Determines whether components communicate via REST APIs, gRPC, etc.
    -   **High-Level Features**: Defines broad functionality without getting into specific implementation details.

### **Examples of HLD:**

-   Designing an online video streaming service like Netflix:
    -   Components: CDN, authentication service, media storage, recommendation engine.
    -   Interaction: Media storage communicates with a CDN; recommendation service connects to user profiles.
    -   Database: The decision is to use NoSQL for user data and object storage for videos.

----------

## **2. Low-Level Design (LLD)**

**Purpose**: Focuses on the details of individual components or modules within the system. It breaks down the high-level design into specific implementation steps.

### **Key Characteristics of LLD:**

-   **Scope**: Deals with the inner workings of each component/module.
-   **Abstraction**: Provides a detailed blueprint that includes how the system will be coded.
-   **Audience**: Targeted at developers and engineers responsible for implementation.
-   **Output**:
    -   Class diagrams
    -   Data structure details
    -   Algorithms and logic
    -   API contracts (inputs/outputs, error codes)
    -   Pseudo-code
-   **Focus Areas**:
    -   **Implementation Details**: Specifies methods, classes, and how logic will be implemented.
    -   **Data Structures and Algorithms**: Defines what will be used for tasks like searching, sorting, or caching.
    -   **API Specifications**: Details input, output, and error handling for individual APIs.
    -   **Database Queries**: Provides SQL or NoSQL query details.

### **Examples of LLD:**

-   For the video streaming service:
    -   User authentication module:
        -   API endpoint: `/login`
        -   Input: Email and password
        -   Logic: Validate credentials against the database using hashing.
        -   Output: Return JWT token on success, error on failure.
    -   Media storage service:
        -   Algorithm: Store videos as chunks and map chunks to user data.
        -   Data structure: Use hashmaps for chunk indexing.