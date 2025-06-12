# Timeline/Feed

### FR
* posts from the people, pages, and groups that a user follows.
* posts may contain media

### NFR
* Feed should appear in a few seconds (say 2-3).
* Latest posts should appear with low lag (say 5s)

OOS:
* Ads

### BOE
* Availability>>Consistency
* QPS: 300M DAU, 5 times a day, 1.5B queries in a day=> QPS: 17,500
* Storage: 100 posts, 1kb avg size, 100Kb*300M=>30T

### Deepdives
* Pagination
* 
