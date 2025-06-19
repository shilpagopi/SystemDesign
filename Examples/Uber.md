# Uber
### FR
* users can enter src,dest to get fare estimates
* user can book a ride 
* driver accept/reject user request
* drivers can navigate to pickup/drop locations

OOS: 
* user request criteria filters: cab types, driver ratings, cab ratings
* ride sharing/pooling
* ride cancellation by user/driver
* pickup time estimates
* monitoring/logginging/alerting/CICD

### NFR
* consistent ride-matching (1:1 matching)
* high availability other than ride matching
* low latency ride matching
* track cab locations almost real-time
* surges/peak times

### Core Data Entities
* users (userid, curr_location, ...)
* cab (cabid, cab location, cab status - Occupied, Available, Unavailable)
* rides (rideid, userid, cabid, ride status - Ride matched, ride started, ride ended)
* Location - entity (not obvious - latest locations of all drivers)

### APIs
<img width="421" alt="image" src="https://github.com/user-attachments/assets/7f472397-645b-494b-99ee-a659e950f967" />

### HLD
* Ride estimate service
* Ride Matching service
* Cab-location update service

### Deepdives
* Location update/search: Use Redis and its Geohashing.
  Quadtrees vs GeoHashing: Quadtrees splits by k (entity count) good, when you have uneven data distribution and infrequent updation. Geohashes divides the world into 32bit encoded grids, irrespective of density. No need of queues to redis cluster (example TPS: 600k) because it can handle massive throughput.
* Ride matching consistency:
  * send a ride request to 1 driver at a time and say allot 10 seconds for timeout.
  * to ensure driver gets only one request at a time (instead of say 100 requests during a surge event), we need to coordinate between multiple distributed instances.
    Follow similar approach as ticket reservation, use distributed Lock with TTL of 10 seconds, to say he is temporarily reserved. Dynamo DB also supports this kind of locking with TTL.

### Diagrams
<img width="1042" alt="image" src="https://github.com/user-attachments/assets/56a9b4d3-c064-4771-acd0-090c9c638345" />

### Followups
* How to bring down locations updates from drivers?
  Client dynamic location update : parked, driver offline.


