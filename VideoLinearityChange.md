# Introduction #

For certain video placements (typically mid-rolls), it is alright to accept linear and non-linear ads. Up to version 2.1 of specs this is not possible. This change addresses this issue.

# Proposed change #

Make the linearity field of the video object optional:
  * If no value is set in the bid request, any ad (linear or not) can be present in the response.
  * If a value is set in the bid request, only ads of the corresponding type can be present in the response.

New phrasing proposed for field description:

Indicates whether the ad impression must be linear, non-linear or can be of any type (field not set). See Table 6.6 Video Linearity for possible values.