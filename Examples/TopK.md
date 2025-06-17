# Top K or Heavy Hitters
### FR
* Top k most viewed/popular entities 
* Time window: min, hr day, alltime - Sliding window vs Tumbling window(fixed)

OOS: 
* Arbitrary starting points for time
* Partial watches not accounted for

### NFR
* Consistency - Staleness 1min.
* Are approximate counts okay? Then use count min sketch or bloom filters
* Low latency - <200ms

### Core Data Model
* Input data stream

### API
getTopKViews(k,window) or GET /topkviews?k={}&window={} -> [(videoIDviews)] Paginated results

### BOE
* 100B Views/day ->QPS: 1M views/sec
* Storage: assume 1M new videos/day => 1M*(12 bytes to store id, 8 bytes to store count)=>20MB/day => 20 * 365 *10 years = 73GB for 10 years.

### Deepdives
* Lagged Queues for accuracy
* Bloomfilters or Count min sketch for approximations

### Diagram
<img width="1343" alt="image" src="https://github.com/user-attachments/assets/c1f6bd83-01e8-4698-b170-a44d1351004f" />
<img width="1062" alt="image" src="https://github.com/user-attachments/assets/fdc15308-7e8c-4eff-b7e8-550ad294f969" />
<img width="1424" alt="image" src="https://github.com/user-attachments/assets/6b9986c6-faf8-4ecf-809d-3960f5e5c172" />




