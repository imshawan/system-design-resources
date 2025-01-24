## **Strong Consistency Implementation**

Strong consistency ensures that all operations on a distributed system are immediately visible to all nodes, providing an up-to-date and uniform view of the data. A common example of a strong consistency setup is a **relational database** with **ACID (Atomicity, Consistency, Isolation, Durability)** properties. Below, we’ll design a simple system with PostgreSQL to demonstrate strong consistency, and write a script to test transactional consistency by simulating concurrent writes.

----------

### **1. Relational Database Setup**

#### **PostgreSQL Installation**

1.  Install PostgreSQL:
    
    ```bash
    sudo apt update
    sudo apt install postgresql postgresql-contrib
    ```
    
2.  Start PostgreSQL:
    
    ```bash
    sudo service postgresql start
    ```
    
3.  Access PostgreSQL shell:
    
    ```bash
    sudo -u postgres psql
    ```
    

#### **Create a Sample Database**

1.  Create a database:
    
    ```sql
    CREATE DATABASE strong_consistency_demo;
    ```
    
2.  Switch to the database:
    
    ```sql
    \c strong_consistency_demo;
    ```
    
3.  Create a `bank_accounts` table:
    
    ```sql
    CREATE TABLE bank_accounts (
        id SERIAL PRIMARY KEY,
        account_number INT UNIQUE,
        balance DECIMAL NOT NULL
    );
    ```
    
4.  Insert sample data:
    
    ```sql
    INSERT INTO bank_accounts (account_number, balance)
    VALUES (101, 1000), (102, 2000);
    ```
    

----------

### **2. Script to Test Transactional Consistency**

The following script simulates **concurrent writes** to a database and tests transactional consistency. For example, transferring money between two accounts requires ensuring that the sum of balances remains consistent even under concurrent operations.

#### **Python Script for Testing**

Install the required dependencies:

```bash
pip install psycopg2-binary
```

```python
import psycopg2
from threading import Thread

# Database connection details
DB_CONFIG = {
    "dbname": "strong_consistency_demo",
    "user": "postgres",
    "password": "your_password",
    "host": "localhost"
}

# Function to transfer money between accounts
def transfer_money(account_from, account_to, amount):
    try:
        conn = psycopg2.connect(**DB_CONFIG)
        conn.autocommit = False  # Use transactions
        cursor = conn.cursor()

        # Step 1: Check balance of account_from
        cursor.execute("SELECT balance FROM bank_accounts WHERE account_number = %s FOR UPDATE", (account_from,))
        balance = cursor.fetchone()[0]

        if balance < amount:
            raise ValueError("Insufficient funds")

        # Step 2: Deduct from account_from
        cursor.execute("UPDATE bank_accounts SET balance = balance - %s WHERE account_number = %s", (amount, account_from))

        # Step 3: Add to account_to
        cursor.execute("UPDATE bank_accounts SET balance = balance + %s WHERE account_number = %s", (amount, account_to))

        conn.commit()
        print(f"Transferred {amount} from Account {account_from} to Account {account_to}")
    except Exception as e:
        conn.rollback()
        print(f"Transaction failed: {e}")
    finally:
        cursor.close()
        conn.close()

# Simulate concurrent transactions
def simulate_concurrent_transactions():
    threads = [
        Thread(target=transfer_money, args=(101, 102, 500)),
        Thread(target=transfer_money, args=(102, 101, 300))
    ]

    for thread in threads:
        thread.start()

    for thread in threads:
        thread.join()

if __name__ == "__main__":
    simulate_concurrent_transactions()
```

----------

### **3. Observing Results**

#### **Expected Behavior**

1.  If the database uses strong consistency, **transactions will be serialized**:
    
    -   Each transaction locks the rows it accesses (`FOR UPDATE`), ensuring no conflicts occur.
    -   Even with concurrent operations, the final balances reflect all successful transactions.
2.  You can query the database to verify balances:
    
    ```sql
    SELECT * FROM bank_accounts;
    ```
    

#### **Output Example**

For the above script:

-   Account `101` starts with `1000` and Account `102` starts with `2000`.
-   After successful transactions:
    -   Account `101`: `800`
    -   Account `102`: `2200`

If one transaction fails (e.g., due to insufficient funds), it rolls back, leaving balances unaffected.

----------

### **4. Key Concepts Explained**

#### **ACID Properties**

1.  **Atomicity**: Transactions are all-or-nothing. Partial updates don’t persist.
2.  **Consistency**: The database moves from one valid state to another.
3.  **Isolation**: Transactions are executed as if they were serialized.
4.  **Durability**: Once committed, changes persist even after crashes.

#### **FOR UPDATE Lock**

-   The `FOR UPDATE` clause locks rows for the duration of the transaction, preventing other transactions from modifying them.

----------

### **5. Summary**

-   The setup demonstrates strong consistency using PostgreSQL.
-   The script ensures that **transactions are serialized** and **data integrity is maintained**, even under concurrent operations.
-   Strong consistency is essential for systems where precise data accuracy is required, like **banking** or **financial systems**.