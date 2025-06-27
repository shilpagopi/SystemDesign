# Tinder
## FR
* Users can set preferences
* Users get feed based on preferences and location
* User can swipe left/right
* Users are notified of a match

OOS: 
* creatign profile and upload images

## NFR
* avoid showing repeat profiles
* consistency for swipes
* low latency stack loading say, <500ms
* scale to handle high write throughput

### BOE
* 10M DAU

### Core Data Entities
* User Profiles (userid)
* Swipe (userid) - swipeid, userid, candidateid, timestamp
* Matches (userid,candidateid,timestamp) composite key

### APIs
<img width="367" alt="image" src="https://github.com/user-attachments/assets/ed0b0210-6780-4e7e-8c12-12168e149bdd" />

### Deepdives
* Swipe Matching Consistency - Redis Cache (Distributed Cache)
* Location - PostGRES with postGIS or Elastic Search
* Candidate Feed (Cache)

### Diagrams
<img width="1346" alt="image" src="https://github.com/user-attachments/assets/3d80a37a-2d52-4dc6-9f33-f79b01f12cc1" />

