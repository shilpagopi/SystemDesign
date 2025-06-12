# Rate Limiter

### FR
* Limits the number of events an entity (user, device, IP, etc.) can perform in a particular time window.
* Takes into account all servers

### NFR
* Availability >> Consistency
* Consistency is variable: stricter for credict card transaction limit, but can be relaxed for token limits.
* Low latency rate limiting verification (not affecting normal user experience)

### HLD
###### Throttling
It is the process of controlling the usage of the APIs by customers during a given
period. Throttling can be defined at the application level and/or API level. When a throttle limit is
crossed, the server returns HTTP status “429 - Too many requests".
* Hard Throttling: The number of API requests cannot exceed the throttle limit.
* Soft Throttling: In this type, we can set the API request limit to exceed a certain percentage. For
example, if we have rate-limit of 100 messages a minute and 10% exceed-limit, our rate limiter will
allow up to 110 messages per minute.
* Elastic or Dynamic Throttling: Under Elastic throttling, the number of requests can go beyond the
threshold if the system has some resources available. For example, if a user is allowed only 100
messages a minute, we can let the user send more than 100 messages a minute when there are free
resources available in the system.

* Fixed window vs Sliding window : Fixed window means user can send k requests at 11:59 and another k at 12:01. unnecessary spikes.
