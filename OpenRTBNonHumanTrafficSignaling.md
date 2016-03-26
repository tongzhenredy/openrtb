# Non Human Traffic signaling in OpenRTB #

DATE: May 16th, 2013, Updated Nov 18, 2013

TO: OpenRTB Dev List and Interested parties

FROM:  Neal Richter, Chief Scientist Rubicon Project and co-chair OpenRTB

This proposal provides a collaborative solution to eliminating botnets and so-called “non-human traffic” from the advertising ecosystem. Siloed detection will not solve this problem. We believe the best approach is to work together as technology platforms to detect, police and prevent such activity.
The proposal is below.

## Responsibility of the exchange ##
Make best effort to classify and reject "non-human traffic"  requests for ads to the exchange via the following best practices:
  * (Recommended) filter impressions from known spiders via user-agent classification
  * (Recommended) filter impressions from suspected NHT via a 'detector'
  * (Recommended) utilize no-bid responses from bidders to improve NHT detection over time via a feedback loop.
  * (Optional) filter impressions from a blacklist of IPs

Where
  * "Filtering the "impression" means that the exchange should respond to the 'ad call' with either a blank HTTP 204 response or an unpaid ad (PSA)
  * The impression should not be offered to any demand partner via RTB.

## Responsibility of the bidder ##
  * (Recommended) no-bid impressions from known spiders via user-agent classification
  * (Recommended) no-bid impressions from suspected NHT via a 'detector'
  * (Optional) no-bid impressions from a blacklist of IPs
  * no bid can have an optional reason code

Where
  * The DSP should respond with an object like the below rather than the supported HTTP 204 (EMPTY)

## Diagram ##

![https://openrtb.googlecode.com/files/OpenRTB-NHT-Signaling.png](https://openrtb.googlecode.com/files/OpenRTB-NHT-Signaling.png)

## User-agent Filtering ##

The IAB provides a list of HTTP user-agents as do other vendors.  The recommended action is to filter impressions or no-bid with a code when the user-agent matches a known list of self-identified "web spiders".

## IP Filtering ##

In all cases the IP used for any filtering should be the originating IP, not an intermediate proxy IP.

If the exchange is using some server-to-server proxy to receive requests, then IP filtering should either be skipped in favor of an access token, OR the exchange should use the originating IP.

Various sources of IP lists are available for cloud and co-lo IPs, anonymizing proxies etc.  Similarly IP lists are available for suspected botnets.

## NHT Detector ##

It is recommended that exchanges and DSPs implementing the spec create a 'Detector'.  The goal of the Detector is to classify impression as 'human traffic' or 'non-human traffic' via various methodologies.

Vendors exist to provide these services.  Many industry participants have developed internal proprietary technology to detect various types of NHT.

Further details on the recommendations of the Detectors are beyond the scope of this proposal.

## OpenRTB Protocol Extension ##

Add an optional "no-bid" field and value to the root object of the response.

0) Field name is "nbr" in the root response object.

1) Add a code field to the bid response that contains an enum of reasons:

| **Value** | **Description** |
|:----------|:----------------|
| 0         | Unknown error   |
| 1         | Technical Error |
| 2         | Invalid Request |
| 3         | Known Web Spider |
| 4         | Suspected Non-Human Traffic |
| 5         | Cloud, Data center, or Proxy IP |
| 6         | Unsupported Device |
| 7         | Blocked Publisher or Site |
| 8         | Unmatched user  |


## Example Response for Known Web Spider ##

In this example the bidder has classified the impression as being from a webspider.  For example the user agent is associated with the Bing.com search engine's web crawler.

```
Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)
```

The bidder would recognize this in the user-agent string from the OpenRTB request's device.ua field.

```
{"id":"1234567890", "seatbid":[], "nbr":3}
```

## Example for Non-Human Traffic ##

In this example, the bidder has classified the impression opportunity, or bid request, as being non-human traffic via a detection algorithm.

Via some suite of data and algorithms running on a bidder's system, the bidder has determined the impression opportunity from www.bobsnewzsite.com exhibits suspicious site or user browsing patterns. The bidder's algorithm scores each impression opportunity with a suspicious traffic probability. When the scored probability is above a threshold the bidder will reject the opportunity and send a no-bid code of "4" back to the seller's exchange. A higher than threshold suspicious probability indicates the opportunity is likely to be non-human.

```
{"id":"1234567890", "seatbid":[], "nbr":4}
```

## Notes ##

Note that the OpenRTB 1.x spec had a ‘nbr’ field (no bid reason) and a set of values. It was abandoned in the 2.x due to under utilization. It is being reintroduced now to further industry wide efforts to detect and police non-human traffic via the new flag.

## Logging and Processing ##

The exchange then has the option of logging this no-bid code for further analysis.

Recommended best practice would be to use a voting scheme for individual impressions and subsequently reduce the impressions to a pattern suitable for adding to the Detector.   For example if more than N DSPs all signaled that a given request was NHT then a pattern is extracted and analysed for frequency and accuracy of classification.

## Possible Difficulties ##

Mobile and Video typically use intermediate proxies prior to the request hitting the exchange.  More thinking needed for any attempt at IP filtering.  Recommendation is to implement accordingly to allow these impressions.

## Alternatives ##

An alternative could be a design of an offline pattern exchange between the various parties.  This involves  a new protocol design.

## Open Questions ##

What about anonymizing proxies like Tor etc?

## Acknowledgements ##
Hat tip to Dstillery, DataXu, MediaMath, PubMatic, Casale and many others for giving feedback on this spec.