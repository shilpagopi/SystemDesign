# Authentication

* Authentication: verify identity of users
* Authorization: determine which users have permission to take which actions

##### Password storage
* Use Hash and store hash values
* Use salting: add 'salt' for each user +actual user password and hash, store hash and salt values.
* Use HTTPS protocol and encryption while sending async requests
* Do not log passwords while processing login
* Once logged in, for the signed-in session, generate a token to be submitted with subsequent requests.
* Session tokens are to be hashed and stored, and should have an expiry time.
* Signatures? - private key, public key?

##### Cookies
* A cookie is simply a text string that the browser associates with a given key and sends back to the server whenever it makes a request. 
* The server may include, in any response, one or more “Set-Cookie” headers, indicating the key and value to store, along with expiration and security metadata.
* "Secure" flag on a cookie tells the browser only to include it in HTTPS requests, keeping it out of unencrypted traffic.
* “HttpOnly” flag: don't allow access to the cookie through JavaScript, frontend code won't have direct access.
* "SameSite" flag: manage sending requests that originate from a different site, to help prevent cross-site request forgery (CSRF) attacks.

###### How to pass userid in API requests?
Send it in a header as JWT authentication session token (If you put it in the body, anybody could misuse it)

