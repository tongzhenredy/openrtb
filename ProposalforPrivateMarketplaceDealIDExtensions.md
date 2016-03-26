# Introduction #

The industry work around Private Marketplaces and DealIDs have created a need to both standardize the protocol within RTB as well as describe best practices for bidding with DealID.

# API Changes #

Proposal is relative to OpenRTB 2.1

The below is a proposal for a standardized extension.  These objects would give within the OpenRTB request object.  They define the context of the PMP opportunity, the "privateness" of the opportunity as well as the list of eligible deals that are enabled for the Impression.

**Private Marketplace Object**
| **Field**	| **Scope**	| **Type**	| **Description** |
|:----------|:----------|:---------|:----------------|
| private\_auction	| recommended |	integer  |	A value of 1 indicates that only bids for the enumerated deals will be allowed to serve. A value of 0 (or unspecified) indicates that open market bids are welcome as well. |
| deals	    | recommended |	array of objects	 | A  list of Direct Deal objects which are eligible for all impressions included in the request. |
| ext	      | optional  |	object   |	A placeholder to support flexibility beyond the standard defined in this specification.  |


**DirectDeals Object**
| **Field**	| **Scope**	| **Type**	| **Description** |
|:----------|:----------|:---------|:----------------|
| id	       | required	 | string	  | A unique identifier for the deal.|
| bidfloor	 | recommended	| float	   | Price floor in CPM. WARNING/Best Practice Note: Although this value is a float, OpenRTB strongly suggests using integer math for accounting to avoid rounding errors.|
| bidfloorcur | optional  | string   | If bid floor is specified and multiple currencies supported per bid request, then currency should be specified here using ISO-­‐4217 alphabetic codes. Note, this may be different from bid currency returned by bidder, if this is allowed on an exchange. |
| wseats	   | optional  |	array of strings	| Array of buyer seats allowed to bid on this auction. Seats are an optional feature of exchange. For example, [“4”,”34”,”82”,”A45”] indicates that only advertisers using these exchange seats are allowed to bid on the impressions in this auction. |
| at        | optional  | integer  | Auction type. If “1”, then first price auction. If “2”, then second price auction. Additional auction types can be defined as per the exchange’s business rules. |
| ext       | optional  | object   | A placeholder to support additional flexibility beyond the standard defined in this specification. |

# Example #
```
{
    "pmp": {
        "private_auction": 0,
        "deals": [
            {
                "id": "AB-Agency1-0001",
                "bidfloor": 2.5,
                "wseats": [
                    "Agency1"
                ],
                "at": 1,
                "ext": {}
            },
            {
                "id": "XY-Agency2-0001",
                "bidfloor": 2,
                "wseats": [
                    "Agency2"
                ],
                "at": 2,
                "ext": {}
            }
        ],
        "ext": {}
    }
}
```

# Bid Response #


**Bid Object Addition**
| **Field**	| **Scope**	| **Type**	| **Description** |
|:----------|:----------|:---------|:----------------|
| deal\_id  | optional  | string   | Indicates that this bid belongs to the indicated Deal Object sent in the request. |

## Example ##

Just the bid object is given below.  The other elements of the response are omitted.

```

    "seatbid": [
        {
            "bid": [
                {
                    "id": "1",
                    "impid": "102",
                    "price": 9.43,
                    "deal_id": "ABC-1234-6789",
                    "adid": "314",
                    "nurl": "http: //adserver.com/winnotice?impid=102",
                    "adm": "%3C!DOCTYPE%20html%20PUBLIC%20%5C%22-%2F%2FW3C%2F%2FDTD%20XHTML%201.0%20Transitional%2F%2FEN%5C%22%20%5C%22http%3A%2F%2Fwww.w3.org%2FTR%2Fxhtml1%2FDTD%2Fxhtml1-transitional.dtd%5C%22%3E%3Chtml%20xmlns%3D%5C%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxhtml%5C%22%20xml%3Alang%3D%5C%22en%5C%22%20lang%3D%5C%22en%5C%22%3E...%3C%2Fhtml%3E",
                    "adomain": [
                        "advertiserdomain.com"
                    ],
                    "iurl": "http: //adserver.com/pathtosampleimage",
                    "cid": "campaign111",
                    "crid": "creative112",
                    "attr": [
                        1,
                        2,
                        3,
                        4
                    ]
                }
            ],
            "seat": "512"
        }
    ]

```

# Best Practice Bidding Logic #


```
1 Receive request and parse
2 Create empty bid list for response
3 If request contains the pmp object
4   match bids against each pmp.deals[]
5   enforce targeting for dealID and seatID
6   append best M matching bids to response
7 If private_auction = False
8   match open auction bids against the request
9   append top N bids by price to response
10 Return response list to exchange
```

Where
  * M >= 1, preferably one per matching DealID
  * N >= 2 to assist with blocking rate issues

Recommendations
  * Minimum viable is “1+1” bidding
  * Ideal is “M+N” bidding

**Warning**
> Returning only one bid when both DealID and open auction bids are valid creates problems.  The exchange side may be configured by a publisher to prioritize all DealID bids above open auction bids, or to force a price auction between them with different floors by class of bid.  There are multiple common practices that depend on how the publisher prefers to sell inventory with DealID.

# Policy Recommendations #

  * A DealID is required for any situation where the auction may be awarded to a bid not on the basis of price alone.  Any prioritization of bids other than by price need a DealID
  * A DealID is recommended for all situations where a preferential floor may be assigned to a seat entity

# Anti Patterns #
Anti-Patterns
The below are a set of anti-patterns that we have observed in various attempts to implement DealID bidding logic.

**Subjecting DealID Bids to an internal auction on price**

The ideal bidding logic describes a process of being liberal about sending bids. DealID bids may not be subject to a classic price auction. There may be an expectation that the buyer and seller want prioritization to achieve a larger objective: complete delivery of the 'Order'. Thus any bidding logic that sorts DealID bids by price and (with or without open marketplace bids) and truncates the list too aggressively can endanger the fulfillment of the Deal.

**Associating DealID to the wrong Object**

DealID should be treated as a 'targeting token' associated to orders, line-items or campaigns. If the DealID is associated to a Seat/Buyer it may create an undesired application of the DealId to many active campaigns. Alternatively if it is associated to the Advertiser it may limit that entity to only a single DealID.

**Improper Handling of the Private vs Open Market Flag**

The private boolean flag indicates that the seller is willing or not willing to accept open market bids, ie "all bidders are welcome". If this flag is not read and interpreted correctly bid responses may be invalid. Open market bids sent to a private impression may be rejected and should not have been exposed to all bidders.

**Improper handling of SeatIDs**

If SeatIDs are treated as a filter of eligible demand partners on an open market impression, this defeats the 'all bidders are welcome' intention.

**Silently Applying Margin Discounts to DealID Bids**

With DealID buyers are sellers are communicating directly. The Exchange and DSP become third-party automation platforms. If there are any automatic or silent discounts of bid prices (based upon margins or fees) prior to sending them to the exchange, then the Deal may fail to function correctly or at all.


# Use cases #

**#1 Open Trading Agreement with Buyer**
  * Between publisher and buying entity
  * Publisher sets an access rule defining the price floor for a specific buyer.
  * Locked to the buyer
  * Broadcast price floor
  * public/open inventory
  * No DealID needed (dealID is optional)
  * no named advertiser(s)
  * no prioritization of bids
  * daily, total or freq caps optional on publisher/exchange side
  * all placements, or limited to specific placements
  * targeting is up to the buyer/bidder


**#2 Open Trading Agreement with Buyer with Named Advertisers**
  * [as #1 with a list of named advertisers](Same.md)

**#3 Open Bidding with DealID as Value-added Markers**
  * Between publisher and buying entity
  * Publisher sets a price floor for URL masked inventory.
  * public/open inventory (all buyers welcome)
  * DealID represents 'Package Tokens'
  * Each DealID signals that the impression falls into various content and placement categories
  * Floor is associated to each DealID to signal cost for usage of that Token
  * Winner is decided by bid price
  * execution of targeting is up to the buyer/bidder


**#4 First Look Trading Agreement**
  * Between publisher and buying entity
  * Publisher sets an access rule defining the price floor for the buyer
  * locked to the buyer
  * known price floor
  * DealID needed
  * Optional named advertiser list
  * Prioritization of bids expected
  * daily, total or freq caps optional on publisher/exchange side
  * all placements, or limited to specific placements
  * targeting is up to the buyer/bidder

**#5 Direct Option Deal with Advertiser via RTB**
  * Between Publisher and Advertiser or their representative.
  * Publisher sets a rule defining a price floor and prioritization for specific advertiser(s)
  * Fill rate is expected to be greater than or equal to X%
  * locked to the buyer
  * private/exclusive inventory
  * limited to a set list of advertiser names (generally variants of one name)
  * known price floor
  * DealID needed
  * Prioritization of bids expected
  * daily, total or freq caps freq caps will apply on bidder side.  Optional on Exchange side
  * limited to specific placements
  * targeting is mostly enforced by buyer/bidder


**#6 Direct Option Deal with Advertiser via RTB with private data**
  * Same as #4
  * DealID represents some combination of private first-party data from the Publisher

**#7 Full Fill Direct Deal with Advertiser via RTB**
  * Same as #4
  * Fill rate is expected to be 100% or nearly so.

**#8 Full Fill Direct Deal with Advertiser via RTB with private data**
  * Same as #6
  * DealID represents some combination of private first-party data from the Publisher