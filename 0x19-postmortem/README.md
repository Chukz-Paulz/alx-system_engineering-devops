Postmortem: The Outage of E-commerce Website Due to Database Failure
Issue Summary
Duration: August 12, 2024, 02:00 AM - 04:30 AM UTC
Impact: The e-commerce website was completely inaccessible, affecting 100% of users. Customers were unable to browse products, place orders, or access their accounts. The outage resulted in an estimated loss of $15,000 in revenue and multiple customer complaints.
Root Cause: A sudden surge in traffic caused by a promotional campaign led to an overload on the primary database, causing it to crash and resulting in a complete outage of the website.
Timeline
02:00 AM: Monitoring alerts triggered, indicating that the website was unresponsive.
02:05 AM: Engineers received automated alerts and began investigating.
02:10 AM: Initial investigation suggested that the issue might be related to the web server; restarted the web server, but the issue persisted.
02:20 AM: Further investigation revealed that the database server was unresponsive. Attempts to restart the database server failed.
02:30 AM: The issue was escalated to the database administration (DBA) team.
02:45 AM: DBA team identified that the database had crashed due to an overload of read/write operations.
03:00 AM: DBA team initiated a database recovery process and increased server resources to handle the load.
03:30 AM: The database was successfully restored, and the website became partially accessible.
04:00 AM: Full functionality was restored after optimising database queries and distributing the load across multiple database instances.
04:30 AM: Monitoring confirmed the system was stable, and the incident was declared resolved.
Root Cause and Resolution
Root Cause: The root cause of the outage was the database server crashing due to an overwhelming number of read/write operations triggered by a promotional campaign. The database server was not adequately provisioned to handle the sudden spike in traffic, leading to a crash.
Resolution: The issue was resolved by recovering the database, increasing server resources, and optimising database queries. Additionally, the load was distributed across multiple database instances to prevent future overloads.
Corrective and Preventative Measures
Improvements:
Implement auto-scaling for the database server to automatically handle traffic spikes.
Enhance monitoring to detect early signs of database overload.
Review and optimize database queries to reduce the load on the server.
Tasks:
Task 1: Configure auto-scaling for the database server.
Task 2: Set up advanced monitoring and alerting for database performance metrics.
Task 3: Conduct a comprehensive review of all database queries and optimize them for performance.
Task 4: Perform regular load testing to ensure the system can handle traffic spikes.
Task 5: Implement a secondary database server for failover in case of future issues.
This postmortem highlights the need for robust database infrastructure, especially during high-traffic events, and underscores the importance of proactive monitoring and optimization to prevent future outages.


