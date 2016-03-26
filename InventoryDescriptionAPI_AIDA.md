# Proposal for Inventory Description API #
## Purpose ##

This API proposal has a dual purpose:
  * To allow SSPs to describe and represent publisher inventory across all buying channels.
  * To allow DSPs and buyers to buy SSP inventory more efficiently by using detailed inventory information

This is an offline API, not a real-time bid-level API.

## Design Philosophy ##

A common friction in APIs is arguments: requirements, passing, etc.  This API should be fully linkable.  One can start at a top level simple API and find links within the returned objects to drill down.

## Use Cases ##

**Basic Discovery of Inventory from Publishers**
  * **As a** buyer using a DSP
  * **I can** see full list of publishers available through each exchange or SSP, including metadata and descriptions
  * **So that** I can approximate how much inventory I can get and on what kinds of sources

**Find/Target specific Publishers or “Private Exchanges” (these are identified by publisher)**
  * **As a** buyer or agency using a DSP
  * **I can** find and target specific publishers (or sites within publishers) on a specific SSP
  * **So that** I can better execute a "direct deal"

**Estimate inventory availability, including size breakout**
  * **As a** buyer or agency using a DSP
  * **I can** estimate the approximate impressions available for pubilshers & sites, broken out by sizes
  * **So that** I can plan delivery of the campaign

**Check if this inventory is allowed for this DSP?**
  * See appendix for open questions and notes on this.

**Can a DSP get an impression estimate by Country?**
  * See appendix for open questions and notes on this.

Link to Spec:  http://code.google.com/p/openrtb/downloads/detail?name=InventoryDescriptionAPISpecAIDA_ver1_RFC.pdf&can=2&q=