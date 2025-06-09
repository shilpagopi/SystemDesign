# FBMessenger
### FR
* send a chat (group?) message
* open a chatview
* receive chat msg
* receive notifications if offline
* track online/offline status
* receive acknowledgement or delivered receipts

OOS:
* permissions - (group memebr, blocked member, etc.)
* Rate limited or monitoring (age appropriate content)
* Disappearing msg or One-time-view media
* Read receipts/blue ticks
* view any chat (use pagination)

### NFR
* Reliability
* Availability>Consistency
* Low latency
* Maintain the sequencing of the messages across devices: Use creation time (time of reception of msg at server) as part of generation of msgid, and sort the msg ids whiel displaying.
  
### Data Entities
* user metadata (userid(index),email, useractive status, last_active..)
* chat table(chatid(indexed),participantid,created,lastused..)
* chat table (chatid,msgid(secondary index?),contenturl,author,time..)
  
For supporting high rates of small writes, and insequence reads, prefer a a wide-column database solution like HBase. HBase is
a column-oriented key-value NoSQL database that can store multiple values against one key into
multiple columns. HBase is modeled after Google’s BigTable and runs on top of Hadoop Distributed
File System (HDFS). HBase groups data together to store new data in a memory buffer and, once the
buffer is full, it dumps the data to the disk. This way of storage not only helps storing a lot of small data
quickly, but also fetching rows by the key or scanning ranges of rows. HBase is also an efficient
database to store variably sized data, which is also required by our service.

### APIs
* sendMsg(chatid,content,userid)
* openChat(chatid, userid)
* LongPolling or Websockets (use routing mechanism probably using redis cache for storing userid:connection object mapping)
* Websockets preferred for tracking active/offline status as well

### BOE
* write-heavy, realtime (latency)
* Estimates: DAU: 500M, avg msgs per day: 40 => 20B messages per day. 200k write queries/s
* Storage Estimation: msg: 100 bytes, 20 billion messages * 100 bytes => 2 TB/day
* Bandwidth Estimation:25 MB/s *2(for upload and donwload)
* Server capacity estimation: chat servers count? For 500 million connections, assuming a modern server can handle 50K concurrent connections at any time, we would need 10K such servers.

### HLD
* sendMsg -> sends to chatserver, receives acknowledgement, msg relayed to other chat participants, write to db (no need to wait for completing this, implement retries for writing to db)
* Whenever a client starts the app:
  * send all notifications immediately
  * and stays online for say 3 sec:
    * pull the current status of all users intheir last x chats or viewport
    * update the active status of user
  * and starts a new chat, pullup that user's latest status
* get latest k msgs in a chat from cache
* For Push notifications, each user can opt-in from their device (or a web browser) to get notifications whenever there is a new message or event. Each manufacturer maintains a set of servers that handles
pushing these notifications to the user. To have push notifications in our system, we would need to set up a Notification server, which will take the messages for offline users and send them to the manufacture’s push notification server, which will then send them to the user’s device.


  
