0. Simple web stack



   +-----------------------------------------------------------+
   |                        User                               |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request
                   |
   +-----------------------------------------------------------+
   |                      Internet                             |
   +-----------------------------------------------------------+
                   |
                   | DNS Resolution
                   |
   +-----------------------------------------------------------+
   |                       DNS Servers                         |
   |                                                           |
   |   +-----------------------+                               |
   |   |   foobar.com          |                               |
   |   |   (Authoritative)     |                               |
   |   |                       |                               |
   |   |   www.foobar.com      |                               |
   |   |   (A Record: IP)      |                               |
   |   +-----------------------+                               |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request
                   |
   +-----------------------------------------------------------+
   |                    Load Balancer                          |
   | (HAproxy with Round Robin distribution algorithm)         |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 1     |          | Application    |   |
   |   |       (Nginx)         |          | Server 1       |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 2     |          | Application    |   |
   |   |       (Nginx)         |          | Server 2       |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request (distributed by Load Balancer)
                   |
   +-----------------------------------------------------------+
   |                  Application Servers                      |
   |     (Runs the application code and interacts with         |
   |                the database)                              |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |    Database    |   |
   |   |       Server 1        |          |    (MySQL)     |   |
   |   |                       |          |    Primary     |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |    Database    |   |
   |   |       Server 2        |          |    (MySQL)     |   |
   |   |                       |          |    Replica     |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+



Explanation of the additional elements:

- I added two load balancers (HAproxy) to distribute the incoming traffic to the web servers (Nginx). This improves the performance and availability of the website by balancing the load and avoiding overloading a single server. HAproxy is a popular open source load balancer that supports various protocols and algorithms1.
- I added two web servers (Nginx) to serve the static and dynamic content of the website. Nginx is a high-performance web server that can also act as a reverse proxy, cache, and load balancer2. Having two web servers increases the redundancy and fault tolerance of the system.
- I added two application servers to run the business logic and interact with the database. The application server can be any platform or framework that supports the development of web applications, such as Node.js, Ruby on Rails, Django, etc. Having two application servers also increases the redundancy and fault tolerance of the system.
- I added two databases (MySQL) to store and retrieve the data for the website. MySQL is a widely used open source relational database management system that supports various data types and queries3. Having two databases allows for a primary-replica (master-slave) cluster, which improves the scalability and reliability of the system.


What distribution algorithm the load balancer is configured with and how it works:

One possible distribution algorithm that the load balancer can use is round robin, which assigns each incoming request to the next available server in a circular order. This is a simple and fair algorithm that ensures equal distribution of load among the servers4.


Is the load-balancer enabling an Active-Active or Active-Passive setup, explain the difference between both:

In this infrastructure, the load balancer is enabling an active-active setup, which means that both load balancers are active and can receive and process requests. This increases the availability and performance of the system, as there is no single point of failure and the load is shared between the load balancers. The load balancers can also monitor each other and take over the traffic if one of them fails.
In contrast, an active-passive setup means that only one load balancer is active and the other one is passive, which means that it only acts as a backup in case the active one fails. This provides high availability, but not high performance, as the passive load balancer is idle and does not share the load.


How a database Primary-Replica (Master-Slave) cluster works:

A database primary-replica (master-slave) cluster works by having one database (the primary or master) that handles all the write operations, and one or more databases (the replicas or slaves) that handle all the read operations. The primary database replicates its data to the replica databases, so that they have the same data. This improves the scalability and reliability of the system, as the read load is distributed among the replicas and the data is backed up in case the primary fails.

What is the difference between the Primary node and the Replica node in regard to the application:

The difference between the primary node and the replica node in regard to the application is that the primary node is responsible for writing the data to the database, while the replica node is responsible for reading the data from the database. The application server needs to communicate with the primary node when it needs to insert, update, or delete data, and with the replica node when it needs to query or fetch data. The application server also needs to handle the case when the primary node fails and switch to another node as the new primary.


Explain what the issues are with this infrastructure:

- Where are SPOF:

- A single point of failure (SPOF) is a component of the system that, if it fails, will cause the whole system to fail. In this infrastructure, there are some potential SPOFs, such as:
- The network connection between the load balancers and the web servers, or between the web servers and the application servers, or between the application servers and the databases. If any of these connections fail, the communication between the components will be disrupted and the system will not function properly.
- The primary database node. If the primary database node fails, the system will not be able to write data to the database, and the application server will have to switch to another node as the new primary. This can cause data loss, inconsistency, or downtime.

- Security issues (no firewall, no HTTPS):

- No firewall. A firewall is a device or software that controls the incoming and outgoing network traffic based on predefined rules. A firewall can help protect the system from unauthorized access, malicious attacks, or unwanted traffic. Without a firewall, the system is exposed to various security threats and vulnerabilities.
- No HTTPS. HTTPS is a protocol that encrypts the communication between the client and the server using SSL/TLS certificates. HTTPS can help protect the data from being intercepted, modified, or stolen by third parties. Without HTTPS, the data is transmitted in plain text and can be easily compromised.

- No monitoring:
- Monitoring is the process of collecting, analyzing, and reporting the performance, availability, and health of the system. Monitoring can help identify and troubleshoot issues, optimize resources, and improve user experience. Without monitoring, the system may suffer from undetected errors, inefficiencies, or failures.

1. Distributed web infrastructure

+-----------------------------------------------------------+
|                        User                               |
+-----------------------------------------------------------+
                   |
                   | HTTP Request
                   |
   +-----------------------------------------------------------+
   |                      Internet                             |
   +-----------------------------------------------------------+
                   |
                   | DNS Resolution
                   |
   +-----------------------------------------------------------+
   |                       DNS Servers                         |
   |                                                           |
   |   +-----------------------+                               |
   |   |   foobar.com          |                               |
   |   |   (Authoritative)     |                               |
   |   |                       |                               |
   |   |   www.foobar.com      |                               |
   |   |   (A Record: IP)      |                               |
   |   +-----------------------+                               |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request
                   |
   +-----------------------------------------------------------+
   |                    Load Balancer                          |
   | (HAproxy with Round Robin distribution algorithm)         |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 1     |          | Application    |   |
   |   |       (Nginx)         |          | Server 1       |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 2     |          | Application    |   |
   |   |       (Nginx)         |          | Server 2       |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request (distributed by Load Balancer)
                   |
   +-----------------------------------------------------------+
   |                  Application Servers                      |
   |     (Runs the application code and interacts with         |
   |                the database)                              |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |    Database    |   |
   |   |       Server 1        |          |    (MySQL)     |   |
   |   |                       |          |    Primary     |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |    Database    |   |
   |   |       Server 2        |          |    (MySQL)     |   |
   |   |                       |          |    Replica     |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+



    2 Servers:
        Why: To ensure high availability and load distribution. If one server fails, the other can take over, reducing the risk of downtime.

    1 Web Server (Nginx):
        Why: To handle incoming HTTP requests and act as a reverse proxy, forwarding requests to the application server.

    1 Application Server:
        Why: To run the application code and process user requests. It interacts with the database to retrieve or store data.

    1 Load Balancer (HAproxy):
        Why: To distribute incoming traffic across multiple servers to ensure no single server becomes a bottleneck. It also provides failover support.

    1 Set of Application Files (Codebase):
        Why: To contain the actual code that powers the website. This is the core logic of the application.

    1 Database (MySQL):
        Why: To store and manage the data required by the application. It ensures data persistence and integrity.

Load Balancer Configuration

    Distribution Algorithm: Round Robin
        How It Works: The Round Robin algorithm distributes incoming requests sequentially across the available servers. Each server receives an equal number of requests in a cyclic order, ensuring balanced load distribution.

    Active-Active vs. Active-Passive Setup:
        Active-Active: Both servers are actively handling requests. If one server fails, the other continues to handle the load. This setup maximizes resource utilization and provides high availability.
        Active-Passive: One server is actively handling requests while the other is on standby. If the active server fails, the passive server takes over. This setup ensures failover but does not utilize all resources efficiently.

Database Primary-Replica (Master-Slave) Cluster

    How It Works:
        The Primary (Master) node handles all write operations and replicates the data to the Replica (Slave) nodes.
        The Replica nodes handle read operations and can take over as the Primary node in case of failure.

    Difference Between Primary and Replica Nodes:
        Primary Node: Handles all write operations and ensures data consistency. It is the single source of truth for the database.
        Replica Node: Handles read operations and provides redundancy. It can take over as the Primary node in case of failure, ensuring high availability.

This infrastructure ensures high availability, load balancing, and efficient resource utilization, making it robust and scalable.

2. Secured and monitored web infrastructure

+-----------------------------------------------------------+
|                        User                               |
+-----------------------------------------------------------+
                   |
                   | HTTPS Request
                   |
   +-----------------------------------------------------------+
   |                      Internet                             |
   +-----------------------------------------------------------+
                   |
                   | DNS Resolution
                   |
   +-----------------------------------------------------------+
   |                       DNS Servers                         |
   |                                                           |
   |   +-----------------------+                               |
   |   |   foobar.com          |                               |
   |   |   (Authoritative)     |                               |
   |   |                       |                               |
   |   |   www.foobar.com      |                               |
   |   |   (A Record: IP)      |                               |
   |   +-----------------------+                               |
   +-----------------------------------------------------------+
                   |
                   | HTTPS Request
                   |
   +-----------------------------------------------------------+
   |                    Firewall 1                             |
   +-----------------------------------------------------------+
                   |
                   | HTTPS Request
                   |
   +-----------------------------------------------------------+
   |                    Load Balancer                          |
   | (HAproxy with Round Robin distribution algorithm)         |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 1     |          | Application    |   |
   |   |       (Nginx)         |          | Server 1       |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 2     |          | Application    |   |
   |   |       (Nginx)         |          | Server 2       |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request (distributed by Load Balancer)
                   |
   +-----------------------------------------------------------+
   |                  Application Servers                      |
   |     (Runs the application code and interacts with           |
   |                the database)                              |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |    Database    |   |
   |   |       Server 1        |          |    (MySQL)     |   |
   |   |                       |          |    Primary     |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |    Database    |   |
   |   |       Server 2        |          |    (MySQL)     |   |
   |   |                       |          |    Replica     |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | Data Collection
                   |
   +-----------------------------------------------------------+
   |                  Monitoring Clients                       |
   |     (Data collectors for Sumologic or other              |
   |                monitoring services)                       |
   |   +-----------------------+          +----------------+   |
   |   |    Monitoring Client1|          |Monitoring Client2|  |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |    Monitoring Client3|          |                |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+



    3 Firewalls:
        Why: To protect the infrastructure from unauthorized access and potential security threats. Firewalls filter incoming and outgoing traffic based on predefined security rules.

    1 SSL Certificate:
        Why: To serve www.foobar.com over HTTPS, ensuring secure communication between the user's browser and the server. HTTPS encrypts data in transit, protecting it from eavesdropping and tampering.

    3 Monitoring Clients:
        Why: To collect data for monitoring services like Sumologic. Monitoring clients gather performance metrics, logs, and other data to help identify and resolve issues proactively.



Firewalls:

    Purpose: Firewalls are network security devices that monitor and control incoming and outgoing network traffic based on predetermined security rules. They act as a barrier between a trusted internal network and untrusted external networks, such as the Internet.

HTTPS Traffic:

    Why: Serving traffic over HTTPS ensures that data transmitted between the user's browser and the server is encrypted. This protects sensitive information from being intercepted or tampered with during transit.



Monitoring:

    Purpose: Monitoring is used to track the performance, availability, and health of the infrastructure. It helps identify issues, optimize performance, and ensure that the system meets service level agreements (SLAs).
    Data Collection: Monitoring tools collect data through agents or clients installed on the servers. These clients gather metrics such as CPU usage, memory usage, disk I/O, network traffic, and application-specific performance indicators.
    Monitoring Web Server QPS: To monitor the web server's Queries Per Second (QPS), you can configure the monitoring client to collect HTTP request metrics from the web server logs or directly from the web server software. This data can be visualized in dashboards to track performance trends and identify bottlenecks.



    Terminating SSL at the Load Balancer Level:
        Issue: Terminating SSL at the load balancer level means that the traffic between the load balancer and the web servers is not encrypted. This can expose sensitive data to potential interception within the internal network.
        Solution: Implement end-to-end encryption by configuring the load balancer to re-encrypt traffic before forwarding it to the web servers.

    Single MySQL Server Capable of Accepting Writes:
        Issue: Having only one MySQL server capable of accepting writes creates a single point of failure. If this server goes down, write operations will be disrupted, leading to potential data loss or inconsistency.
        Solution: Implement a Primary-Replica (Master-Slave) setup where the Replica can take over as the Primary in case of failure. Alternatively, consider using a multi-master setup for high availability.

    Servers with All the Same Components:
        Issue: Having servers with all the same components (database, web server, and application server) can lead to resource contention and performance bottlenecks. It also makes it difficult to scale individual components independently.
        Solution: Separate the components onto different servers or use containerization to isolate services. This allows for independent scaling and better resource management.

This updated infrastructure ensures enhanced security, secure communication, and proactive monitoring, addressing potential issues and optimizing performance.

3. Scale up

+-----------------------------------------------------------+
|                        User                               |
+-----------------------------------------------------------+
                   |
                   | HTTPS Request
                   |
   +-----------------------------------------------------------+
   |                      Internet                             |
   +-----------------------------------------------------------+
                   |
                   | DNS Resolution
                   |
   +-----------------------------------------------------------+
   |                       DNS Servers                         |
   |                                                           |
   |   +-----------------------+                               |
   |   |   foobar.com          |                               |
   |   |   (Authoritative)     |                               |
   |   |                       |                               |
   |   |   www.foobar.com      |                               |
   |   |   (A Record: IP)      |                               |
   |   +-----------------------+                               |
   +-----------------------------------------------------------+
                   |
                   | HTTPS Request
                   |
   +-----------------------------------------------------------+
   |                    Firewall 1                             |
   +-----------------------------------------------------------+
                   |
                   | HTTPS Request
                   |
   +-----------------------------------------------------------+
   |                    Load Balancer Cluster                  |
   | (HAproxy with Round Robin distribution algorithm)         |
   |   +-----------------------+          +----------------+   |
   |   |      Load Balancer 1  |          |  Load Balancer2|   |
   |   |       (HAproxy)        |          |   (HAproxy)    |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request (distributed by Load Balancer)
                   |
   +-----------------------------------------------------------+
   |                  Web Servers                             |
   |                                                           |
   |   +-----------------------+          +----------------+   |
   |   |      Web Server 1     |          |   Web Server 2|   |
   |   |       (Nginx)         |          |    (Nginx)     |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request (distributed by Load Balancer)
                   |
   +-----------------------------------------------------------+
   |                  Application Servers                      |
   |     (Runs the application code and interacts with           |
   |                the database)                              |
   |   +-----------------------+          +----------------+   |
   |   |       Application     |          |   Application  |   |
   |   |       Server 1        |          |    Server 2   |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | HTTP Request (distributed by Load Balancer)
                   |
   +-----------------------------------------------------------+
   |                  Database Servers                         |
   |                                                           |
   |   +-----------------------+          +----------------+   |
   |   |      Database 1      |          |   Database 2   |   |
   |   |       (MySQL)        |          |    (MySQL)     |   |
   |   |    (Primary)         |          |   (Replica)    |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+
                   |
                   | Data Collection
                   |
   +-----------------------------------------------------------+
   |                  Monitoring Clients                       |
   |     (Data collectors for Sumologic or other              |
   |                monitoring services)                       |
   |   +-----------------------+          +----------------+   |
   |   |    Monitoring Client1|          |Monitoring Client2|  |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   |   +-----------------------+          +----------------+   |
   |   |    Monitoring Client3|          |                |   |
   |   |                       |          |                |   |
   |   +-----------------------+          +----------------+   |
   +-----------------------------------------------------------+



    1 Server:
        Why: To ensure high availability and load distribution. If one server fails, the other can take over, reducing the risk of downtime.

    1 Load Balancer (HAproxy) Configured as Cluster:
        Why: To distribute incoming traffic across multiple servers to ensure no single server becomes a bottleneck. It also provides failover support. Configuring HAproxy as a cluster ensures high availability and redundancy for the load balancer itself.

    Split Components (Web Server, Application Server, Database) with Their Own Server:
        Why: To isolate services and optimize resource utilization. This allows for independent scaling and better performance management.

SPECIFICS ABOUT THE INFRASTRUCTURE



    1 Server:
        Purpose: Adding an additional server ensures high availability and load distribution. If one server fails, the other can take over, reducing the risk of downtime.

    1 Load Balancer (HAproxy) Configured as Cluster:
        Purpose: Configuring HAproxy as a cluster ensures high availability and redundancy for the load balancer itself. This setup distributes incoming traffic across multiple servers, preventing any single server from becoming a bottleneck and providing failover support.

    Split Components (Web Server, Application Server, Database) with Their Own Server:
        Purpose: Splitting components onto their own servers isolates services and optimizes resource utilization. This allows for independent scaling and better performance management, ensuring that each component can operate efficiently without resource contention.

