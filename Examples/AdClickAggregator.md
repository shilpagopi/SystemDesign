# Ad Click Aggregator
### FR
* Users can click on ads
* Advertisers can view metrics/dashboard with min granularity of 1 min

OOS:
* Client device types
* Ad-serving, targeting and recommendations
* Bot detection
* User auth
* Conversion tracking

### NFR
* Scalability to supprot peak QPS
* Low latency queries by adv <2-3s
* Fault tolerance/reliability: not lose clicks
* User can click only once on an Ad on one serving (based on ad impression id)

### System Interface (not necessarily APIs)
* user click stream data -> redirection
* advertiser queries -> metrics

### BOE
* 10M ads
* Peak QPS: 10k clicks/sec

### HLD
* Use Spark (mapreduce) for almost realtime aggregations. say every 5 min. and store the results in an OLAP (ONline Analytical Processing) server
* Use Queues with data retention (say, 7 days), topic ids coudl be ad ids and Flink for real time aggregator over 1 min window. If window is larger, do checkpointing of Flint every 15 mins.

### Deepdives
* Ad impression id - unique for each browser placement. couple it with sugnature to prevent user abuse.

### Followups
* Hot Ads?  
  Use topic id in queues=AdID+[A-Z]. Flint can handle ~10k QPS.

### Diagram
<img width="1370" alt="image" src="https://github.com/user-attachments/assets/abc04be8-5a51-40de-8d47-86b7a7f8025f" />
