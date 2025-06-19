# BookMyShow

## FR
* Book Tickets
* Search Events
* View an event

OOS: 
* payment gateways
* event creation
* cancellation

### NFR
* Consistency wrt to booking (no double booking)
* Availability>>Consistency
* Surges for popular events
* Reliability (tickets should not disappear)

### Core Data Entities
* Users (userid,...)
* Events (eventid, event title, time, location, eventtype,....)
* Bookings (userid, eventid, details, status...)
* Artists
* Venues
POSTGRES - ACID properties for booking and relational nature of data

### APIs
* GET: search(search string, filters = {location, dates, eventtype} ->paginated results
* reserve_booking() -> OK
* confirm_booking() -> acknowledgement (notification, email)
  <img width="1335" alt="image" src="https://github.com/user-attachments/assets/29499f21-1392-42bd-ab7f-fd3c7ad32927" />

### BOE
* Read>>write 100:1

### HLD
* search service
* view event + event creation service -> filter out availabel seats obtained from db further using distributed Ticket Lock storing temp reservations.
* reserve/book service:  implement temp ticket reservation
  * Option 1: less effective option is using a cron job to delete the reserved statuses in db
  * Options 2: using redis cache with TTL (Distributed Lock) to store ticketid:reservedTrue (only available or booked status in DB)

### Deepdives
* Elastic Search with location and other criteria search
* Caches for quick searches or view events 
* Virtual waiting queue for selected events to handle high surge. This could be a kafka queue or a redis sortedset based on incoming timestamp.

<img width="1161" alt="image" src="https://github.com/user-attachments/assets/99c1ed26-6ee8-4223-ba95-81f06e16486f" />

### Followups
* How to update seat booking layout with bookings/reservations happened in last 10 min?
  Long Polling/Server side events (SSE) for continuous update.

