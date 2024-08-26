# Replication
* Why? To avoid a single point of failure, increase availability, to improve proximity (and thereby latency), to increase throughput (more compute power)
* Leader: Machine that handles write requests to the data store.
* Followers: Machines that are replicas of the leader node, and cater to read requests.
* Failover: In case the leader goes down, one of the follower nodes (mostly with the most up-to-date data) is promoted to be the leader

#### Synchronous, asynchronous, semi-synchronous replication
* synchronous : When a write request comes in, the leader waits for an acknowledgment from all of the followers to mark it as acknowledged. 
* async: no waiting, directly marked successful, risky, but less time-consuming for the client
* semi-synchronous: where only one follower is synchronously updated and the rest are asynchronous. When the former crashes, one of the latter is made a synchronous follower. 

#### Multi leader
* more than one machine can take the write requests, more reliable
* every machine (including leaders) needs to catch up with the writes that happen over other machines
* Conflict resolution for concurrent writes: Keeping the update with the largest client timestamp, Sticky routing—writes from same client/index go to the same leader.

#### Leaderless replication
All machines can cater to write and read requests. In some cases, the client directly writes to all the machines, and requests are read from all the machines based on quorum. 
Quorum refers to the minimum number of acknowledgements (for writes) and consistent data values (for reads) for the action to be valid.
