# PasteBin
### FR
* upload text/image (size limit) and get a generated URL
* access URL
* custom alias (size limited)
* TTL
* delete (both from cache and db)

OOS: 
* video upload
* restricted access
* analytics
* authentication

### NFR
* URLs not guessable, unique
* Availability >> consistency
* Reliability
* Rate limited

### Data Entities
url,contentkey,userid,createdAt,TTL, ..
user table

### Capacity Estimation
Mildly read-heavy: say 5:1 (read replicas of db?)
Estimates: 1M pastes
QPS: ~60 pastes/s
Cache: 20% memory

### APIs
paste_data(data object,userid, TTL, customalias)
get_paste(url)
delete_paste(url,userid)

### HLD,Deepdives
All similar to tinyURL

