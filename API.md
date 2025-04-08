# Network Connectivity
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

SSE uses a persistent HTTP connection for one-way data streaming.Then the server can push real-time updates to a client.Can't transmit binary data. 
Use Cases: Stock tickers, news feeds, game updates. 


  
## RPC or the Remote Procedure Call 
* more space efficient than REST
* no special syntax like REST
* only used for internal communication (say remote login)

## GraphQL 
* single request fetch all needed backend data and modify at front end
* more front-end development work, no backend dat aaggregations
  
  
  
