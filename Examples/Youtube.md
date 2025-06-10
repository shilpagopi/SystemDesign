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
* videos (video id)

### API
* uploadVideo()
* getVideo(id)

### BOE
* Read heavy say,200:1
* Estimates:
  * 500M DAU, 5 vdos/day =>2.5B views/day 25k video views/sec
  * one minute of video needs 50MB of storage
  * every sec. 10 hrs of video is uploaded

### Deepdives
* video transcoding
* 
