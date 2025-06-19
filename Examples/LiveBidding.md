# Live Bidding
### FR
* Users can place bids on an active auction item. Bid Increment: Fixed or percentage?
* Auction has a definite end time. Winner determination at the end of the auction. Sniping/Extension: Does a bid in the last 'X' seconds extend the auction time? 
* Notification to winner/losers.

## NFR
* All participants see bids update in real-time <200ms
* Bids must be validated (e.g., minimum bid increment, sufficient funds).
* Fairness: All bids should be processed in the order they were received.
* Consistency: Bid state (current highest bid, bid history) must be consistent across all users and system components.

OOS: 
* Sealed-bid
* User Funds Handling (Pre-authorization? Wallet system? (Assume pre-authorization or checking wallet balance at bid time)

