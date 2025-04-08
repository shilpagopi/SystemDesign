# Network Protocols

## Default:HTTP (Hypertext Transfer Protocol) 
* an application layer protocol forming the foundation of data communication for the World Wide Web
* a request–response protocol in the client–server model
* Request methods: Read-only : GET, HEAD, OPTIONS, and TRACE

##### REST or Representational State Transfer 
* A single URI to access via various actions/HTTP verbs (GET/PUT/POST/PATCH/DELETE)
* It requires to write the requests for each type of entity in your database, in contrast to GraphQL.
* Not as space efficient as RPC.
* REST API (Representational State Transfer Application Programming Interface) is an architectural style for designing networked applications. It relies on a stateless, client-server, cacheable communications protocol, and in virtually all cases, that protocol is HTTP.
* Key Principles of REST: Separate client-server entities, stateless, cacheable, etc.
* GET: Retrieves a resource or a list of resources.
* POST: Creates a new resource.
* PUT (works somewhat like replace): Updates an existing resource, replacing the entire resource with the new representation.
* PATCH: Partially updates an existing resource, modifying only specific attributes.
* DELETE: Deletes a resource.
  
## Websockets vs. SSE
For real-time communication: WebSockets for two-way communication and Server-Sent Events (SSE) for one-way communication from server to client
Both these are built on top of TCP and IP. 

WebSockets use a handshake (an initial HTTP request) to establish a persistent, full-duplex TCP connection. Once the connection is established, both the client and server can send data to each other at any time. More complex to set up than SSE. 
Use Cases: Chat applications, real-time dashboards, collaborative editing tools. 
Tools: Elastic Load Balancer AWS supports Websockets load balancing. 
Using a Publish/Subscribe or pub/sub broker is an effective method of horizontally scaling WebSockets.

SSE uses a persistent HTTP connection for one-way data streaming.Then the server can push real-time updates to a client.Can't transmit binary data. 
Use Cases: Stock tickers, news feeds, game updates. 

## WebRTC
WebRTC allows for real-time communication between clients and direct peer-to-peer connections. 
Usecases: video, chat, file sharing, and live video streaming. 

## TCP and UDP

The two underlying transport layers that facilitate data transfer in fundamentally different ways.

TCP (Transmission Control Protocol) is a standard that defines how to establish and maintain a network conversation via the Internet. TCP is the most commonly used protocol on the Internet and any connection-oriented network. When you browse the web, your computer sends TCP packets to a web server. A web server responds by sending TCP packets back to your computer. A connection is first established between two devices before any data is exchanged, and TCP uses error correction to ensure that all packets are delivered successfully. If a packet is lost or corrupted, TCP will try to resend it.

UDP (User Datagram Protocol) is a connectionless, unreliable transport layer protocol. It does not require a connection to be established or maintained and does not guarantee that messages will be delivered in order. Meaning that there can be some data loss if a packet does not get sent or if it’s corrupted. UDP is often used for streaming media or real-time applications where dropped packets are less problematic than ensuring delivery.

## Short Polling (How can server send a new info to client?)
HTTP short polling is a technique where the client repeatedly sends requests to the server until it responds with new data. Once it receives data it starts the process again and repeatedly asks until something else is available.

## Long Polling (How can server send a new info to client?)
With HTTP long polling, a single request is made from the client, and then the server keeps that connection open until new data is available and a response can be sent. After the client receives the response, a new connection is immediately made again.

  
## RPC or the Remote Procedure Call 
* more space efficient than REST
* no special syntax like REST
* only used for internal communication (say remote login)

## GraphQL 
* single request fetch all needed backend data and modify at front end
* more front-end development work, no backend dat aaggregations

## References
* https://getstream.io/blog/communication-protocols/
  
  
  
