# Twitter Search

### FR
* Users can create posts
* Users can like posts
* Search textual string among the tweets
* Ranking - based on time (use creationtime in index), or likes

OOS
* Image/video based search
* Hashtags
* UserProfile searches
* Pagination
* Pruning older tweets from index
* No Filters
* Ranking based on other factors like personal history, popularity, user's graph
* Fuzzy search
* Type correction
* Personalization

### NFR
* Low latency search (cache) <500ms
* Availability>>Consistency
* Updates cannot have a large offset/lag to better user experience (say ~1min)

### Core data entities
* Tweet (id, contenturl)
* Search table: words ->tweetids. Use consistent hashing over words. 
* Hashtag - Tweet mapping
* Likes
* Users

### APIs
* Search (search string, #results, sortcriteria (likes or time)
* Like PUT likes/postid (user id in header)
* Create posts

### BOE
* Estimates:
  * 1.5B users, 800M DAU, 400M tweets/day (tweet size 300b), 730B tweets in 5 yrs =>need 5 bytes (~40) bits to store tweet id. 730GB/year for tweet id storage (values field in hashmap)
  * 500M searches in a day: QPS: 5k queries/s
  * 300K English words and 200K nouns: 500k total words in our index. avg word len: 5 char=> 2.5M chars => 5MB (1 char = 2 bytes) (key field in hashmap)
  * Index only past 2 years tweets
* Updates and reads almost similar (think users may search once or post once per day) - updates cannot have a large offset/lag

### HLD
* create posts: tokenization(do not index stop words), put index in redis cluster. 
* like service: likes db, likes aggregator - write only if likes change by log to the base 2
* search: search cache, store like:<postid>, time:<postid> in reddis cluster

### Deepdives
* indexing using redis cluster
* likes update aggregator

### Diagrams
<img width="1494" alt="image" src="https://github.com/user-attachments/assets/c6bcdd02-4519-43cc-925a-4dde33507e5e" />

### Follow-ups
* How to store all indices?
  Do count min sketch on keywords: and remove all cold keywords to cold storage
* How to index bigrams?
  Either store popular ones in redis lookups and if not found, search for individual one-grams, filter through all the contents and merge and sort.
* How to handle hot keywords?
  In partioning key: create keyword+[A-Z] shards only for popular keywords
