# DropBox

### FR
* upload content (text/image files) (with file size limit)
* download content
* sync

OOS:
* video content
* blob storage
* share with other users
* offline editing
* versioning
* notification about update of shared files
* storage limitations
* resolve conflict

### NFR
* Highly available
* Reliable
* Low latency uploads and downloads
* Support large files (50GB) - resume if disconnected in between
* ACIDity (consistent in term of file operations) sync accuracy

### Core Data Entities
* client metadata (clientid) (indexed on clientid and userid (secondary index))
* user metadata (indexed on userid)
* doc chunk metadata (docid, chunkid, owneruser, created, updated, updatedbyclientid, access, checksum) - composite indexing on docid,chunkid - SQL for ACID
  - Rangebased partitioning on chunkid? Chunkid = dociId+autoincrement (to ensure all chunks stay in same range or single shard)?
* local on-device metadata database - local workspace

### APIs
* POST files/upload; body: file, filemetadata -> 200 OK (userid with authentication)
* GET files/fileid -> filemetadata (userid with authentication)
* sync(starttimestamp) -> [filemetadata] or  GET /sync/lasttime stamp
  <img width="396" alt="image" src="https://github.com/user-attachments/assets/6d343834-557e-4dbe-bf96-6f87755355bf" />

### BOE
* Both read and write heavy
* 100M DAU, 500M total users, storage in PBs

### HLD
* Client app for upload/download/sync/resolve conflict files
* local chunker (say, 5MB chunks)
* local hasher
* local synchronizer
* Long HTTP Polling for checking for updates on any chunk hash
* Queue for any sync(upload/edit/download) request
* Notification queue for multiple clients (based on clientids)

### Deepdives
* Chunking: Multipart upload
* Synchronizing: Delta sync only chunks, Reconciliation

<img width="1424" alt="image" src="https://github.com/user-attachments/assets/9449325e-3faa-42d0-9b83-46aec93a4ab3" />


### Followups
* How to ensure that a file is actually succesfully uploaded into blob storage?
  Option 1: Blob storage notification (but it is supported only for whole files not as part of multi-part upload)
  Option 2: Trust and verify (let the client notify the system upload complete, but we will query blob storage to verify and then update database.
* Is CDN required?
  Good to **skip** if a large majority of files is locally shared, it will anyway be placed on the closes server and it is not publicly accessed like social media
* How to optimize the databytes sent over to blob storage?
  Compression for text files (based on MimeType)
* What connection type to remote?
  Websockets could be an overkill, as the user may not always be using the app. Adaptive polling based on increased frequencies once the user starts using the app.
* How to do temporary version control?
  Use event bus so that we can track and apply or undo recent actions to the file. If the event bus queue becomes too long, then do snapshotting after certain points.
