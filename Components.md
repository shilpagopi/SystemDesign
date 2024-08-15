# Components

* Cache
* Load balancer
* API gateway
* User session storage (can be in a noSQL db)
* Distribute(Queue)
* Evaluation (logging, metrics, monitoring)

* DNS
* CDN
* Sharding
* Data Replication

## Back-of-the-envelope Calculation
* Estimate QPS (queries per second),DAU (daily active users) and storage requirements:  \
* Peek QPS = 2 * QPS  \
* 1 Char = 1 byte, 1 Integer = 4 bytes, 1 Float = 4 bytes
<img width="819" alt="image" src="https://github.com/user-attachments/assets/6dfa9681-c5e5-463d-920a-f1d628531c0e">

