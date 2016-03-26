This page captures the proposed changes from OpenRTB 2.3 to 2.4.


---

### 1. Multi Sizes/Formats Per Impression ###

See [AdFormatsProposal](https://code.google.com/p/openrtb/wiki/AdFormatsProposal).

This is being done via custom extensions in various exchanges.

### 2. Mobile Bundle description fix ###

Alter description of app.bundle to conform to now it's being used in practice by Nexage, MoPub, etc.

"A platform-specific application identifier. On Android this should be a bundle or package name (e.g., com.foo.mygame); on iOS it is a numeric ID (e.g., 628677149). It is intended to be unique to the app and independent of the exchange."