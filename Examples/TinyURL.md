# TinyURL

### FR
* Generate short alias
* Redirection
* Custom URL support
* Custom Time to Live (TTL) (default: say 2 years)
* Delete URLs

OOS:
* Restricted URLs
* Analytics
* Login
* Editing

### NFR
* Unique URLs, not easily guessable
* Redirections low latency
* Time to Live (TTL) should be adhered
* Availability>>Consistency
* Rate limited usage to prevent abuse

### Data Model/Core Entities
* URL : alias, originalurl, createdby, time, ttl   
* USER: user metadata  
* Database: Since no complex relationships, NoSQL choice would also be easier to scale

### API
* GET getShortUrl(actual_url, custom_alias, ttl, username)  
* GET access(shorturl) (“HTTP 302 Redirect”)

### BOE Calculation
* Read-heavy
* QPS, Storage, Cache memory
* Estimates: Say, 200 URLs per second, assuming 1:100 write:read ratio, 20K redirections/s

### High Level Design
How would we perform a key lookup? We can look up the key in our database or key-value store to
get the full URL. If it’s present, issue an “HTTP 302 Redirect” status back to the browser, passing the
stored URL in the “Location” field of the request. If that key is not present in our system, issue an
“HTTP 404 Not Found” status or redirect the user back to the homepage.

Custom alias impose limit to url lengths. Check if unique.
DB Partitioning: Hash-Based Partitioning (Consistent Hashing)

TTL: 
Whenever a user tries to access an expired link, we can delete the link and return an error to the user, purge it then and there.
Periodic purging service 

Delete URLs: Need not reuse keys as we have plenty.

### Deepdive
##### Create URL 
###### Option-1: MD5 or SHA
We could use MD5 or SHA crypto hashing functions. Their primary purpose is to take an input (like a file, message, or password) of any length and produce a fixed-length string of characters, called a hash value or message digest. 
<img width="793" alt="image" src="https://github.com/user-attachments/assets/c5a3a0b3-bd2d-42a4-8ac4-8be9fbe93a00" />
Using base64 encoding, a 6 letter long key would result in 64^6 = ~68.7 billion possible strings
Using base64 encoding, an 8 letter long key would result in 64^8 = ~281 trillion possible strings
If we use the MD5 algorithm as our hash function, it’ll produce a 128-bit hash value. After base64 encoding, we’ll get a string having more than 21 characters (since each base64 character encodes 6 bits of the hash value). Since we only have space for 8 characters per short key, how will we choose our key then? We can take the first 6 (or 8) letters for the key. This could result in key duplication though,
Variants: Use usedid (needs user to be signed in), append increasing url id.

###### Option-2: Key Generation Service
Can concurrency cause problems? Allocate a set of pregenerated keys for each server. Even if the server dies, it is okay.
Synchronize(or get a lock on) to ensure duplicate keys are not distributed across servers.

###### Cache
* Cache storage: 6 (characters per key) * 68.7B (unique keys) = 412 GB. 170GB memory to cache 20% of daily traffic. Store TTL data as well (8 bytes for 1 datetime stamp). 2x memory.
* Least Recently Used eviction strategy

###### Security/Restricted Access
URL: add private/public access level  
url x userids : Multicolumn indexing. Column order matters. Url first.

  



