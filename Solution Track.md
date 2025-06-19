# Solution Track
* Requirements: functional and non-functional
   * Identify the main business objects and their relations
   * Data types: Structured data, Media and blobs (jar,tar files, binary data)
   * APIs: access patterns: Given [object A], get all related [object B]
  * What information do these objects hold? Can it contain media?
  * mutability of objects: editing, deletion
  * NFR: Performance (optimize for user facing sync requests), partition tolerance, Availability, Consistency, Security (eg. running low trust user submitted code in isolation)
* Scale: Read and write, QPS and Data storage
* Design: Microservices
  
## For Product Design
<img width="1117" alt="image" src="https://github.com/user-attachments/assets/3cbfa577-296a-4829-be25-a40d36c779fb" />

## For Infrastructure Design
<img width="1572" alt="image" src="https://github.com/user-attachments/assets/f0c24130-1ab5-488e-982b-66479883b951" />

## Deepdive CheatSheet
No. | Problem Statement | Deepdives | Remarks
--|--|--|--
1| Dropbox | * Chunking: Multi-part upload <br> * Sync: Delta sync, fingerprinting, reconciliation| * Client side app <br> * Sync frequency: adaptive polling based on client usage

## BOE Estimates
No. | Problem Statement |Users | Entities
1| Dropbox |  | 

