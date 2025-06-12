# Twitter Search

### FR
* Search textual string among the tweets
* AND or OR
* Ranking - based on time (use creationtime in index)

OOS
* Image/video based search
* Hashtags
* UserProfile searches
* Pagination
* Pruning older tweets from index
* Ranking based on other factors like personal history, popularity, user's graph

### NFR
* Low latency (cache)
* Availability>>Consistency
* Updates cannot have a large offset/lag to better user experience

### Core data entities
* Tweet (id, contenturl)
* Search table: words ->tweetids. Use consistent hashing over words. 
* Hashtag - Tweet mapping

### APIs
* Search (search string, #results)

### BOE
* Estimates:
  * 1.5B users, 800M DAU, 400M tweets/day (tweet size 300b), 730B tweets in 5 yrs =>need 5 bytes (~40) bits to store tweet id. 730GB/year for tweet id storage (values field in hashmap)
  * 500M searches in a day: QPS: 5k queries/s
  * 300K English words and 200K nouns: 500k total words in our index. avg word len: 5 char=> 2.5M chars => 5MB (1 char = 2 bytes) (key field in hashmap)
  * Index only past 2 years tweets
* Updates and reads almost similar - updates cannot have a large offset/lag

### HLD
* do not index stop words
