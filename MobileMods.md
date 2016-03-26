# Introduction #

Here is an update to the proposal I made for new attributes and enumeration values based on feedback from our discussion at the 04-09-2013 OpenRTB Summit.  Nexage has been using extensions for all of these and I believe they are sufficiently broad in their utility to propose promoting them into the standard.  All proposed items are backward compatible.

# Proposed Modifications #

## Item-1: Expansion of Enumerated API Frameworks ##

**Current State:**  Current enumeration values listed in Table 6.4 of OpenRTB v2.1 are “1” = VPAID 1.0, “2” = VPAID 2.0, “3” = MRAID, “4” = ORMMA.

**Proposed Change:**  Change “3” to MRAID-1 and add “5” = MRAID-2.

**Rationale:**  MRAID-2 features are a superset of MRAID-1.  Buyers wishing to use MRAID-2 features need to know when clients support those additional features and thus the proposed new “5” value.  Without this distinction (i.e., the current state), the only safe interpretation of “3” is in fact MRAID-1.  Therefore this proposal is backward compatible and adds a useful new distinction for those who choose to use it.  At the OpenRTB Summit, I believe we agreed that changes to these lists should ultimately be coordinated with IAB, but that we could move forward in the meantime.

## Item-2: Expansion of Enumerated Device Types ##

**Current State:**  Current enumeration values listed in Table 6.16 of OpenRTB v2.1 are “1” = Mobile/Tablet, “2” = Personal Computer, “3” = Connected TV.

**Proposed Change:**  Add “4” = Phone, “5” = Tablet, “6” = Connected Device, and “7” = Set Top Box.

**Rationale for 4 & 5:**  Assumptions can often be made about phones that cannot be made about tablets (e.g., using the max banner size that fits the width, etc.) because of the practical upper limit to their size.  Thus mobile buyers need to know phone vs. tablet.  Without this distinction (i.e., the current state), the only safe interpretation of “1” is unspecified mobile which could be either.  This propose doesn’t change this interpretation, thus is backward compatible, and adds a useful new distinction for those who choose to use it.  Similar to Item #1, I believe we agreed that changes to these lists should ultimately be coordinated with IAB, but that we could move forward in the meantime.  We further discussed that as devices continue to converge, some form of screen size may become more broadly useful.  I believe here too we decided to move forward on this as the enumeration already exists and the new distinction provides immediate value, subject to potential evolution in the future to a size-based approach.

**Rationale for 6 & 7:**  In a more simplistic scenario where a single app developer (publisher) writes a more or less uniform way of delivering advanced advertising in streaming video assets, on a single delivery platform, there would be no need to differentiate between the end device/ app store platforms. This simplistic scenario is evolving into more complex business models quite rapidly though.  Typically apps developed for a specific device and app store combination run within that silo. Thus smart TV apps do their own rendering. But some apps running on connected devices do not rely on Smart TV to do the rendering - they typically have ad network plugins that request the ads dynamically, and insert them during playback. The new stream with ads is then delivered via digital or analog auxiliary input to the TV as if it was just a monitor. So it sometimes matters to know which device type it is from a purely execution perspective - to match delivered video assets, bitrates, etc.  But additionally, it is also important to know the device profile from an analytical and business point of view. For e.g. it is plausible that this information could be used in targeting decisions, reconciliation and audit, etc.  Finally, a third reason is that the content industry's stance on rights for ad inventory on streaming video assets to "OTT devices" is still evolving. There are different quarters of the video content delivery ecosystem (both traditional and emerging) who might have interest in selling ad inventory for which they have negotiated rights on specific platforms. This trend is yet to unravel but could create yet another need for specifically identifying which platform (device type)/ which publisher/ which format.


## Item-3: Clear Advertiser ID as an Additional Device ID ##

**Current State:**  The device object has hashed versions of the equipment ID (e.g., IMEI of the device) and platform or O/S level ID (i.e., Android ID, iOS UDID, etc.).

**Proposed Change:**  Add “ifa” as an optional attribute to the Device object to be used as a clear (i.e., not hashed) advertiser ID when available.

**Rationale:**  Nearly a year ago, Apple introduced their Advertiser Identifier (a.k.a., ID for Advertisers or IFA) intended as a user-safe alternative to UDID and intended to be used in the clear.  Apple alone is reason enough to support this given their position in the market, but ultimately we believe that others will follow their lead.

## Item-4: Hashed MAC Address as Additional Device IDs ##

**Current State:**  See Item #3.

**Proposed Change:**  Add “macsha1” and “macmd5” as optional attributes to the Device object.

**Rationale:**  Many in mobile began using MAC address when Apple threatened to prohibit UDID usage before they released their Advertiser Identifier.  Although the Apple driven impetus for using MAC address is waning, the proliferation of advertising via other types of IP enabled devices (e.g., set top boxes, DVRs, who knows what else) may continue to keep the ubiquitous MAC address relevant.  At the OpenRTB Summit, some expressed the possibility of using the existing equipment ID fields for MAC address.  For some use cases such as frequency capping, this certainly works.  However, one counter case is CPI Tracking in mobile (CPI = Cost per Install) where two independent parties need to match IDs (i.e., the publisher/SSP that displayed an ad for an app and install beacon of that app).  Overloading the use of attributes can cause confusion in cases like this and can result in breakage that is hard to debug.

## Item-5: Minimum Banner Size Parameters ##

**Current State:**  The "h" and "w" parameters in the Banner object imply exact size required.

**Proposed Change:**  Add an optional set of size parameters called "hmin" and "wmin", which if included would indicate that a range of sizes are allowed with these minimum dimensions with "h" and "w" taken as recommended.  If they are not included, then "h" and "w" should be considered exact as is currently the case.

**Rationale:**  In mobile, there is often a concept of allowed sizes.  The current h/w doesn't allow this to be conveyed.  After considering an alternative of an array of sizes, we realized that the real use cases would all be satisfied by the simpler proposal.  An example simple banner use case might be primarily 320x50, but width at least 300 is acceptable (e.g., h=50, w=320, wmin=300).  An interstitial example could be full screen desired, but partial is ok (e.g., w=320 h=480, wmin=300, hmin=250).