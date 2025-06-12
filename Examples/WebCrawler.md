# WebCrawler
### FR
* crawl web:For HTML pages only, HTTP Protocol only.
* extensible for new type of webpages

OOS: 
* HTML pages only? Or should we fetch and store other types of media? (Hint: break down the parsing module into different sets of modules: one for HTML, another for images,etc.)
* Othe protocols

### NFR
* Deduplication
* Robots Exclusion Protocol: What is ‘RobotsExclusion’ and how should we deal with it? Courteous Web crawlers implement the
Robots Exclusion Protocol, which allows Webmasters to declare parts of their sites off limits to
crawlers. The Robots Exclusion Protocol requires a Web crawler to fetch a special document called
robot.txt which contains these declarations from a Web site before downloading any real content from
it.

### Core Data entities
* URLs (url - indexed, crawled time, crawling success, content urls) 
* URL dedupe: list of crawled urls
* URL dedupe: list of to-be crawled urls
* Doc dedupe: checksums of docs
  
### BOE 
* 15B websites in 4 weeks: 15B / (4 weeks * 7 days * 86400 sec).  QPS~= 6200 pages/sec
* Storage: 1MB per file: 15PB over a month, adding buffer: 20PB

### HLD
* How to crawl:
* Use BFS (generally) and Path-ascending crawling (Eg. For http://foo.com/a/b/page.html, crawl /a/b/, /a/, and /.)
1. Pick a URL from the unvisited URL list.
2. Determine the IP Address of its host-name.
3. Establish a connection to the host to download the corresponding document.
4. Parse the document contents to look for new URLs.
5. Add the new URLs to the list of unvisited URLs.
6. Process the downloaded document, e.g., store it or index its contents, etc.
7. Go back to step 1

<img width="1246" alt="image" src="https://github.com/user-attachments/assets/e3cfa347-64fc-48e3-b113-00ba67fe6e85" />

* URL frontier: To store the list of URLs to download and also prioritize which URLs should be
crawled first.
* HTTP Fetcher: To retrieve a web page from the server.
* Extractor: To extract links from HTML documents.
* Duplicate Eliminator: To make sure the same content is not extracted twice unintentionally.
* Datastore: To store retrieved pages, URLs, and other metadata.

### Deepdives
* To avoid downloading robots.txt file on every request, our crawler’s HTTP protocol module can maintain a fixed-sized cache mapping host-names to their robot’s exclusion rules.
* Deduplication: 64-bit checksum: MD5 or SHA
* Checkpointing during a crawl
* How to handle crawler traps? For example, people have written traps that dynamically generate an infinite Web of documents.

  
