# Status #

This is a DRAFT proposal for inclusion in a future revision of OpenRTB 2.x

# Introduction #

The United States Federal Trade Commission has changed the compliance rules for the Children’s Online Privacy Protection Act (“COPPA”), effective July 1, 2013.    The proposal effects websites, and associated services), that have been identified as: (1) directed to users under 13 years of age; or (2) collecting information from users actually known to be under 13 (collectively “Children’s Sites”).

The FTC has written a comprehensive FAQ on the change here:

http://business.ftc.gov/documents/Complying-with-COPPA-Frequently-Asked-Questions

# Impacts #

The FAQ specifically calls out these areas relevant for OpenRTB as 'Personal Information' that is not to be collected.

  * Geolocation information sufficient to identify street name and name of a city or town;
  * Persistent identifiers when they can be used to recognize a user over time and across different Web sites or online services.

# OpenRTB Request Object Modification #

New 'Regulations' object at the base level:

```
"regs":{
 "coppa":1
}
```

# Recommendations to Implementers #

OpenRTB Exchanges and Bidders should
  1. Provide a facility for sites to be declared as 'child directed'
  1. Implement the regulations object extension
  1. Provide facilities within campaigns to target for and against this signal
  1. Degrade the Geographic information to be less exact prior to logging or transmission
  1. Suppress the assignment and synchronization of identifiers (depending on usage)

It is recommended that when regs.coppa is 1 then exchange should additionally manipulate the OpenRTB bid request object as follows:

Device Object
  * suppress didmd5 and didsha1 device IDs
  * truncate ip field - remove lowest 8 bits
  * truncate ipv6 field - remove lowest 32 bits

Geo Object
  * suppress lat/long
  * suppress metro, city and zip

User Object
  * suppress id, buyerid, yob, gender


# Notes #

Steve Bellovin, CTO of the FTC, argued for a standardized signaling protocol here (dated January 2013)
http://techatftc.wordpress.com/2013/01/02/coppa-and-signaling/