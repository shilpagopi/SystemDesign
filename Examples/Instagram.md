# Instagram or Twitter
### FR
* Follow
* Create posts or tweets(text,images)
* Feed

OOS:
* short videos
* restricted access
* analytics
* authentication
* searches and hashtags
* commenting, follow recommendations
* account creation
* DMs

### NFR
* Read-heavy
* Availability>>Consistency
* Reliability
* Low latency feed

### Data Entities
* user.. metadata
* postid, postcontenturl, userid, ... (indexed by postid)
* follower,followee (indexed by followee)

### BOE
* Read-heavy 
* Instagram Estimates: 500M total users, with 1M daily active users. 2M new photos every day, 23 new photos every second.
* Twitter Estimates: 100M DAU

### Deepdives
* Photo ID creation:To sort photos on their time of creation, photoID = time+autoincrementingsequence 
To store the number of seconds for next 50 years? 86400 sec/day * 365 (days a year) * 50 (years) => 1.6 billion seconds
We would need 31 bits to store this number. Since on the average, we are expecting 23 new photos per
second; we can allocate 9 bits to store auto incremented sequence. So every second we can store (2^9
=> 512) new photos. We can reset our auto incrementing sequence every second.
* Feedcreation
* Media rendering

### Diagrams
<img width="1592" alt="image" src="https://github.com/user-attachments/assets/bc6a7ae8-346c-4da5-81ff-194f281c321d" />

