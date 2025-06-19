 # Bloom Filters and Count-Min Sketch
probabilistic data structures -less memory than exact data structures - approximate answers and can have a small chance of error (specifically, false positives). 
They are incredibly useful in scenarios where memory is constrained, speed is paramount, and a small degree of error is acceptable

## Bloom Filters
To test whether an element is a member of a set. Instead of storing the actual elements, a Bloom filter stores a "fingerprint" of the elements using multiple hash functions.

##### How it Works:
<img width="738" alt="image" src="https://github.com/user-attachments/assets/cfefd58f-616d-4e2d-85dc-165f4b384434" />

###### Key Characteristics and Trade-offs:
* No False Negatives
* Possible False Positives
* Space Efficient: O(k)
* Time Efficient(Adding and checking elements are constant-time operations, O(k))
* Cannot Delete Elements: In a basic Bloom filter, you cannot reliably remove elements. If you unset a bit, it might correspond to another element that is in the set, leading to false negatives for that other element. (Counting Bloom Filters are a variant that addresses this by using counters instead of bits).
* Configurable Error Rate: You can tune the size of the bit array (m) and the number of hash functions (k) to achieve a desired false positive rate. A larger m or more k (up to an optimal point) generally reduces the false positive rate, but uses more memory/CPU.

###### Common Use Cases:
* Checking for previously seen items:
* Web browsers checking if a URL is malicious (blacklist).
* Databases checking if a key exists in a cache before an expensive disk lookup (e.g., Cassandra uses them).
* Recommendation engines filtering out already-shown items.
* Deduplication: Detecting if a new item is a duplicate of something already processed.
* Spell checkers: Rapidly checking if a word is in a dictionary

## Count-Min Sketch
To estimate the frequency of events in a stream of data

##### How it Works:
<img width="738" alt="image" src="https://github.com/user-attachments/assets/aa634099-7fe1-4a58-a88b-305d4b7d0098" />

<img width="738" alt="image" src="https://github.com/user-attachments/assets/e0c85bb6-bbbb-4cbd-994d-55337568abb6" />

###### Key Characteristics and Trade-offs:
Adv: Space-Efficient, Fast Updates and Queries, Probabilistic Guarantees, Handles Data Streams: 
Disadvantages: Nonexact results, Overestimation: never underestimates, No Deletions: Standard Count-Min Sketch does not support decrements or deletions of items. (There are variations like the "Count-Min Sketch with Conservative Updates" that address this partially).

###### Common Use Cases:
Network Traffic Analysis: Estimating the frequency of IP addresses, ports, or protocols.
Trending Topics/Hashtags: Identifying popular topics on social media.
Heavy Hitters Detection: Finding elements that appear very frequently (often combined with other techniques).
Database Query Optimization: Estimating the selectivity of predicates.
Botnet Detection: Identifying malicious IP addresses with unusual activity patterns.


  
