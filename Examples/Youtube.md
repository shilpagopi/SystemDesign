# Youtube
### FR
* view videos
* upload videos
* like/dislike

OOS:
* Restricted access
* Analytics
* Comments
* Search
* Login/Authentication
* Channels
* Recommendations
* Notifications

### NFR
* Reliability
* Availability >> consistency
* Low latency

### Data Entities
* user (id indexed, other metdata)
* videos (video id) - sharding based on video ID (if based on userid, data imbalance)

### API
* uploadVideo(content, title, desc, )
* streamVideo(id,offset,resolution)

### BOE
* Read heavy say,200:1
* Estimates:
  * 500M DAU, 5 vdos/day =>2.5B views/day 25k video views/sec
  * one minute of video needs 50MB of storage
  * every sec. 10 hrs of video is uploaded
 
### HLD
* A successful upload will return HTTP 202 (request accepted) and once the video encoding is completed
the user is notified through email with a link to access the video.
* Processing Queue: Each uploaded video will be pushed to a processing queue to be de-queued
later for encoding, thumbnail generation, and storage.

### Deepdives
* video transcoding
* video deduplication: during upload: check chunkwise matching using algortihms (e.g., Block Matching, Phase Correlation, etc.) and skip uploading those unless new video is of higher quality
  
