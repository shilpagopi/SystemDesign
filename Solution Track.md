# Solution Track
* Requirements: functional and non-functional
   * Identify the main business objects and their relations
   * Data types: Structured data, Media and blobs (jar,tar files, binary data)
  * What information do these objects hold? Can it contain media?
  * access patterns: Given [object A], get all related [object B]
  * mutability of objects: editing, deletion
  * NFR: Performance (optimize for user facing sync requests), partition tolerance, Availability, Consistency, Security (eg. running low trust user submitted code in isolation)
* Scale: Read and write QPS and Data storage
* Design: Microservices
