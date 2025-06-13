# Uber
### FR
* users can request ride within a search radius
* driver accept/reject user request
* track cab locations

OOS: 
* user request criteria filters: cab types, driver ratings, cab ratings
* no ride sharing
* ride cancellation by user/driver

### NFR
* availability
* consistent ride-matching
* driver can be assigned only one ride
* one ride should be assigned only one driver
* low latency ride matching

### Core Data Entities
* users (userid, curr_location, ...)
* cab (cabid, cab location, cab status - Occupied, Available, Unavailable)
* rides (rideid, userid, cabid, ride status - Ride matched, ride started, ride ended)

