## **Scalability**

Scalability is a critical aspect of designing distributed systems. It ensures that your system can handle increasing loads efficiently, either by adding more resources (horizontal scaling) or upgrading existing ones (vertical scaling). This guide demonstrates how to implement a load balancer using Nginx and HAProxy, simulate increasing traffic, and observe the effects of horizontal scaling.

----------

### **Step 1: Implementing a Load Balancer**

A **load balancer** distributes incoming requests to multiple backend servers, ensuring no single server gets overwhelmed. We'll use **Nginx** and **HAProxy**, two widely used tools for this purpose.

#### **Option 1: Nginx Load Balancer**

1.  **Install Nginx**:
    
 ```bash
sudo apt update
sudo apt install nginx -y
  ```
    
2.  **Configure Nginx**:
    
    -   Open the Nginx configuration file:
        
        ```bash
        sudo nano /etc/nginx/nginx.conf
        ```
        
    -   Add a configuration for load balancing under the `http` block:
        
        ```nginx
        http {
            upstream backend_servers {
                server 127.0.0.1:5000;  # Backend server 1
                server 127.0.0.1:5001;  # Backend server 2
                server 127.0.0.1:5002;  # Backend server 3
            }
        
            server {
                listen 80;
        
                location / {
                    proxy_pass http://backend_servers;
                    proxy_set_header Host $host;
                    proxy_set_header X-Real-IP $remote_addr;
                }
            }
        }
        ```
        
3.  **Restart Nginx**: Apply the changes by restarting Nginx:
    
    ```bash
    sudo systemctl restart nginx
    ```
    
4.  **Run Backend Servers**: For simplicity, you can create simple backend servers using Flask or Express.js.
    
    **Example Flask Server** (Save as `server1.py`, `server2.py`, and `server3.py` with different ports):
 
	 ```python
	from flask import Flask
	app = Flask(__name__)

	@app.route('/')
	def hello():
	    return "Response from Server on Port 5000", 200  # Change port number for each server

	if __name__ == '__main__':
	    app.run(port=5000)  # Replace with 5001, 5002 for other servers

	```
	    
	Start the servers:
	    
	```bash
	    python3 server1.py &
	    python3 server2.py &
	    python3 server3.py &
	```
    
5.  **Test the Load Balancer**: Open a browser or use `curl` to access the Nginx server:
    
    ```bash
    curl http://localhost/
    ```
    
    Requests should be distributed across the backend servers in a round-robin manner.
    

----------

#### **Option 2: HAProxy Load Balancer**

1.  **Install HAProxy**:
    
    ```bash
    sudo apt update
    sudo apt install haproxy -y
    ```
    
2.  **Configure HAProxy**:
    
    -   Edit the HAProxy configuration file:
        
        ```bash
        sudo nano /etc/haproxy/haproxy.cfg
        ```
        
    -   Add the following configuration:
        
		```haproxy
		global
		    log stdout format raw
		    maxconn 2000
		    daemon

		defaults
		    log global
		    option httplog
		    timeout connect 5000ms
		    timeout client 50000ms
		    timeout server 50000ms

		frontend http_front
		    bind *:80
		    default_backend backend_servers

		backend backend_servers
		    balance roundrobin
		    server server1 127.0.0.1:5000 check
		    server server2 127.0.0.1:5001 check
		    server server3 127.0.0.1:5002 check

		```
        
3.  **Restart HAProxy**:
    
    ```bash
    sudo systemctl restart haproxy` 
    ```
    
4.  **Test the Load Balancer**: Send multiple requests to the HAProxy server and verify distribution:
    
    ```bash
    curl http://localhost/
    ```
    

----------

### **Step 2: Simulating Traffic**

To test scalability, we simulate increasing traffic using Apache Benchmark or a Python script.

#### **Using Apache Benchmark**:

Install Apache Benchmark:

```bash
sudo apt install apache2-utils -y
```

Simulate 1000 requests with 100 concurrent connections:

```bash
ab -n 1000 -c 100 http://localhost/
```

#### **Using a Python Script**:

Install `requests`:

```bash
pip install requests
```

**Traffic Simulation Script**:

```python
import requests
import threading

# Configuration
TOTAL_REQUESTS = 1000
CONCURRENT_THREADS = 100
TARGET_URL = "http://localhost/"  # Load balancer address

def make_request():
    try:
        response = requests.get(TARGET_URL)
        print(f"Status Code: {response.status_code}")
    except Exception as e:
        print(f"Request failed: {e}")

def simulate_traffic():
    threads = []
    for _ in range(TOTAL_REQUESTS // CONCURRENT_THREADS):
        for _ in range(CONCURRENT_THREADS):
            t = threading.Thread(target=make_request)
            t.start()
            threads.append(t)

        # Wait for all threads to complete
        for t in threads:
            t.join()

if __name__ == "__main__":
    simulate_traffic()

```

Run the script and observe how the load balancer handles traffic.

----------

### **Step 3: Observing Horizontal Scaling**

1.  **Baseline Performance**: Start with a single backend server and monitor:
    
    -   CPU usage
    -   Memory utilization
    -   Response times
2.  **Add More Servers**: Gradually add more backend servers by running the Flask/Express.js apps on different ports. Update the load balancer configuration to include the new servers.
    
3.  **Monitor Impact**: Use tools like `htop` for server resource monitoring or logging tools (e.g., ELK stack) to track request distribution and performance metrics.
    

----------

### **Expected Results**

-   **Single Server**: The single backend server may become a bottleneck under high traffic, resulting in slower response times or timeouts.
-   **Multiple Servers**: The load balancer efficiently distributes traffic, reducing individual server load and improving response times.

----------

### **Takeaways**

1.  Load balancers like Nginx and HAProxy ensure even distribution of requests, reducing bottlenecks.
2.  Horizontal scaling improves reliability and performance under heavy loads.
3.  Simulating traffic provides insights into how the system behaves under stress, allowing better optimization of resources.