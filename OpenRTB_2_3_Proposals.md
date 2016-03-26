This page captures the proposed changes from OpenRTB 2.2 to 2.3.  The primary driver for v2.3 is support for Native Ad Units, but other enhancements and corrections are also being considered.

---

### 1. Native Ad Units ###

A Native Subcommittee has been formed to define the request and response structures for native ad units.  A principle of _separation of concerns_ has been adopted to reduce the coupling between OpenRTB (as commerce and transport) and native specific issues (and those of future ad units) to allow independent evolution of these specs.

A Word document has been circulated to capture the proposed OpenRTB impact called "Native Proposal for OpenRTB 2.3".  The highlights are listed here:

  * An impression can be specified as native by including an `imp.native` object as an alternative to the existing `imp.banner` or `imp.video` objects.
  * The `imp.native` object contains `request` as an opaque string that includes request parameters conforming to the Native Ad Specification, `version` which indicates the version of the Native Ad Specification to which `request` conforms, and an optional `video` child object (reuse of the existing OpenRTB `video` object) that can be used to convey video parameters when allowed as a native asset.
  * The ad unit response is transported from bidder to exchange as an opaque string conforming to the Native Ad Specification, the version of which must match that of the request.  Existing OpenRTB transport options and macro substitution features apply.

### 2. Test Mode ###

This is to allow an exchange to send bid requests for integration testing that will not be billable.  Therefore:

  * Add attribute `test` (optional integer) to the top-level bid request object, where 0 (default if omitted) = live auction; 1 = test auction (i.e., not billable).

### 3. Mobile Website Signal ###

This is to indicate that a website is mobile optimized.  Therefore:

  * Add attribute `site.mobile` (optional integer), where 0 = no, 1 = yes, omitted is unknown.

### 4. Additional Device Attributes ###

With recent advances in display technology, requests are getting more frequent for data describing device screen density, pixel ratio, etc.  Requests for hardware versions when known have also become common since the UA doesn't always provide this (e.g., iPhone 4 vs. 5).  Therefore:

  * Add attribute `device.h` (optional integer) as the physical height of the screen in pixels.
  * Add attribute `device.w` (optional integer) as the physical width of the screen in pixels.
  * Add attribute `device.pxratio` (optional float) as the ratio of physical pixels to device independent pixels.
  * Add attribute `device.ppi` (optional integer) as the pixels per linear inch.
  * Add attribute `device.hwv` (optional string) to represent the hardware version of the device.

### 5. Refinements to Do-Not-Track ###

There has been some debate in the industry over the nuances of the do-not-track signal.  The consensus is that the browser's DNT should be interpreted by a different set of rules from the "limit ad tracking" signal developed by Apple and mimicked by Android.  Therefore:

  * Add device.lmt, where 1 = enabled (i.e., limit tracking), 0 = disabled (i.e., no limit on tracking).
  * Clarify guidance to indicate that dnt is the standard browser header do-not-track and lmt is the commercially adopted limit tracking signal (e.g., as endorsed by iOS and Android).

### 6. UTC Offset ###

This is to provide bidders with the information they need to perform day-parting based on local time.  As this is a function of location, the `geo` object is the appropriate object.  This also enables local time to be expressed for the current device location and/or the user's home location, given that both the `device` and `user` objects can have a child `geo` object.  Since day-parting often includes awareness of day, a simple local time is insufficient.  Minutes offset from UTC is an efficient way to convey the essential data as an integer and compute the time, day, or full date from common UTC language features.  Therefore:

  * Add attribute `geo.utcoffset` (optional integer) as the number of minutes from UTC that represents local time.  For `device.geo.utcoffset`, this would be the local time where the device is currently located.  For `user.geo.utcoffset`, this would be the local time of the user's home base.

### 7. Block-Check Support in Bids ###

Exchanges can enforce block lists via `bid.adomain` for advertisers and `bid.attr` for creative attributes.  For the same purpose, this proposal is to add to the `bid` object the content category of the creative (for checking against blocked categories) and the bundle of the app if the ad is promoting an app (a very common mobile case and one for which publishers often want surgical blocking below the advertiser level).  Therefore:

  * Add response attribute `bid.cat` (optional string) as the IAB content category of the creative.
  * Add response attribute `bid.bundle` (optional string) as the bundle (i.e., unique ID) of the app being advertised.

### 8. Fix "`keywords`" ###

The `keywords` attribute appears in `site`, `app`, `user`, and `content` objects.  From OpenRTB 1.0 - 2.1, this attribute in all relevant objects was a comma-separated list of keywords.  In the 2.2 spec, a note was added that its type can be either string (i.e., a comma-separated list as before) or an array of individual keyword strings.  The merits of either notwithstanding, it is not possible to define a JSON schema to cover this dual typed case which foils coverage by the Validator.  It also foils a bidder from mapping the JSON to class structure.  Therefore:

  * Revert `keywords` in these 4 objects to the pre- v2.2 definition of only a string type defined as comma-separated list.
  * If the group feels that an array of keywords is more useful, the type could either be changed in the next major version (i.e., 3.0) or a string array variant (e.g., `kywrds`) could be added now to allow for transition.

### 9. Fix "`content.context`" ###

The `content.context` attribute is an enumeration and as such should be an integer, but the spec calls it out as a string.  This originated as a long-undetected spec error and should be changed to an integer like all other enumerations.  Fixing the type primarily, however, would violate the rules of backward compatibility for minor versions.  Therefore:

_Option-1:_
  * Change the type of the existing attribute to integer.  It's not pure, but if we had consensus, I would support this rather than carry the baggage of Option-2 and since I believe a) this was a spec error from the beginning, and b) this is not as heavily used a field as most (albeit conjecture on my part).  It might not even break some JSON parsers.

_Option-2 (pure):_
  * Add `content.cntxt` (optional integer) with the same meaning but in integer rather than string form.  Exchanges who choose to convey this optional information would send both string and integer forms enabling bidders to transition without breakage.
  * Annotate the string variant `content.context` as deprecated and we'll drop it entirely in 3.0.

### 10. Fix "`device.carrier`" ###

The `device.carrier` attribute description currently reads "Carrier or ISP derived from the IP address.  Should be specified using Mobile Network Code (MNC)".

Aside from the ambiguous "should", MNC is problematic since it is not unique without the country code; thus carrier codes are usually MCC+MNC.  But MCC+MNC is still problematic.  It's not always possible to get this code and it's cumbersome to target on it.  Some carriers have multiple MCC+MNC codes (e.g., Vodafone has two in Spain), so buyers who don't care about the distinction (probably most) would have to specify all the variations to cover their bases.  Buyers who want to target Vodafone in Spain probably do not care about the specific codes; what they would care about more naturally is carrier="VODAFONE" and country="ESP".

While standard codes maintained by an external authority are generally preferable when available (i.e., country codes, language codes, etc.), this is a case where the codes are not aligned with our needs and common names should be used.  There is OpenRTB precedent for other attributes with common but non-standard values (e.g., OS, device make/model).  Therefore:

  * Clarify the description of `device.carrier` to return the common name of the carrier and cite a best-practice for exchanges to publish their complete list to their buyers.