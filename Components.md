# Components
* Cache
* Load balancer
* API gateway
* User session storage (can be in a noSQL db)
* Distribute(Queue)
* Evaluation (logging, metrics, monitoring)

#### Extras
* DNS
* CDN
* Sharding
* Data Replication

## Back-of-the-envelope Calculation
* Estimate QPS (queries per second),DAU (daily active users) and storage requirements:  \
* Peek QPS = 2 * QPS  \
* 1 Char = 1 byte, 1 Integer = 4 bytes, 1 Float = 4 bytes
* 1 day = 10^5 seconds
* 1 year = 3 x 10^7 seconds
<img width="819" alt="image" src="https://github.com/user-attachments/assets/6dfa9681-c5e5-463d-920a-f1d628531c0e">

## Basic Architecture
<img width="1172" alt="image" src="https://github.com/user-attachments/assets/8dcbbe8c-051b-427e-995b-89173a152913">


