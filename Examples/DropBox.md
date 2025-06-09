# DropBox

### FR
* upload content (text/image files) (with file size limit)
* edit content
* share with other users
* auto-sync after editing and uploading from one device
* auto-sync during download on a different device

OOS:
* video content
* offline editing
* versioning
* notification about update of shared files
* storage limitations
* resolve conflict

### NFR
* ACIDity (consistent in term of file operations)
* Highly available
* Reliable
* Low latency

### Data Entities
* client metadata (clientid) (indexed on clientid and userid (secondary index))
* user metadata (indexed on userid)
* doc chunk metadata (docid, chunkid, owneruser, created, updated, updatedbyclientid, access, checksum) - composite indexing on docid,chunkid - SQL for ACID
  - Rangebased partitioning on chunkid? Chunkid = dociId+autoincrement (to ensure all chunks stay in same range or single shard)?
* local on-device metadata database - local workspace

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
* Chunking
* Synchronizing
