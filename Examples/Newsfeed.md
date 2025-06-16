# Timeline/NewsFeed

### FR
* posts from the people, pages, and groups that a user follows.
* posts may contain media
* notify people of new posts

### NFR
* Feed should appear in a few seconds (say 2-3).
* Latest posts should appear with low lag (say 5s)

OOS:
* Ads
* Monitoring
* Ranking based on time, popularity, user history

### Core data entities
* User (user id - index, ...)
* User-Connections (publisher - indexed, follower-secondary index, type of subscribedentity)
* Posts (postid-index, userid (GSI), timestamp, contenturl). 

### APIs
* getNewsFeed(userid, #count/pagelimit, cursor (currtimestamp of lastviewed post))
* post(userid, content, creation_timestamp)

### BOE
* Availability>>Consistency
* QPS: 300M DAU, 5 times a day, 1.5B queries in a day=> QPS: 17,500
* Storage: 100 posts, 1kb avg size, 100Kb*300M=>30TB. If a machine can store in memory 100GB, we need 300 machines.

### HLD
* Post service: use queues
* Follow service
* Feedservice

### Deepdives
* Pagination - get user's last viewed timestamop or cursor. Query posts before that as user scrolls down. On app-load, last viewed should be fresh so cursor could be time_now(). 
* The process of pushing a post to all the followers is called a fanout. By analogy, the push approach is
called fanout-on-write, while the pull approach is called fanout-on-load. Go for hybrid.

### Diagram
<img width="1024" alt="image" src="https://github.com/user-attachments/assets/93903c3f-e846-47e3-9c45-478c4618094e" />

### Followups
Optimisations: Selective,dynamic generation of user feeds based on access frequency.
