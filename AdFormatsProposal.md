In OpenRTB 2.32 we introduced wmin/wmax and hmin/hmax in an attempt to support one bid request for multiple sizes.
Unfortunately it didn't solve the problem well as it does not give the bidder unambiguous directions.

Problem statement:
A seller wants to expose an impression that support multiple 'formats'. Example:
Single impression will support [600x250](600x250.md) that changes shape on rollover via a well defined
IAB Rising stars unit or [300x250](300x250.md) with a fixed creative size.

Using wmin/wmax & hmin/hmax allows the seller's exchange to specify the below:
"wmin": 300
"wmax": 600
"w":    300
"hmin": 250
"hmax": 250
"h":    250

However these instructions to the bidder are ambiguous as a 500x250 format is technically within the constraints expressed,
yet not desired by the seller.  Additionally the seller wants confirmation of the format of the bid response in order to price
the ad placement differently based upon the bid's creative size.  Analytics and reporting on ad formats is also more difficult
as the exchange must map the bid response back to a standard adunit size supported, also a source of ambiguity.


The following work-arounds are known to exist to eliminate this ambiguity in OpenRTB
- use of private extension to specify an array of w/h pairs.
- parallel bid requests to the same bidder with different sizes
- sequential cascaded requests to the same bidder with different sizes

We propose the following approach for OpenRTB 2.3 or a subsequent draft.

Request object changes

Banner object
| Field | Scope | Type | Default | Description |
|:------|:------|:-----|:--------|:------------|
| w      | optional/recommended | integer | -       | Width of the impression in pixels. Since some ad types are not restricted by size this field is not required, but it’s highly recommended that this information be included when possible. |
| h      | optional/recommended |  integer | -       | Height of the impression in pixels. Since some ad types are not restricted by size this field is not required, but it’s highly recommended that this information be included when possible. |
| wmin   | optional | integer | -       | Deprecated  |
| wmax   | optional | integer | -       | Deprecated  |
| hmin   | optional | integer | -       | Deprecated  |
| hmax   | optional | integer | -       | Deprecated  |
| sizes  | optional | array of objects  | -       | An array of objects representing available ad sizes and formats for bid on this impression.  If utilized it is recommended that the array contain at least one entry.  See Sizes Object for more detail. |

Note that if w & h are omitted is recommended that the sizes array of objects be present and populated with at least one entry.

Sizes Object
| Field | Scope | Type | Default | Description |
|:------|:------|:-----|:--------|:------------|
| w      | recommended | integer | -       | Width of the ad size in pixels.  |
| h      | recommended | integer | -       | Height of the ad size in pixels. |
| fmts    | optional    | array of integers | -       | The format ID of the ad.  See Table X.Y below |

Response Object changes

Bid.Object
| Field | Scope | Type | Default | Description |
|:------|:------|:-----|:--------|:------------|
| w      | required | integer | -       | Width of the ad size in pixels.  |
| h      | required | integer | -       | Height of the ad size in pixels. |
| fmt   | recommended | integer | -       | The format ID of the ad.  See Table X.Y below |

Example Request:
```

"banner": { 
    "sizes": [ 
        {
            "w": 600,
            "h": 250,
            "fmts": [55,]
        },
        {
            "w": 300,
            "h": 250,
            "fmts":  [3,]
        }

     ],
    "pos": 0 
    },
```
Example Response:
```
{
"id": "19511639a3c96823fa4d6f9a577473ec934eba3d",
"seatbid": [
    {
        "seat": "testseat",
        "bid": [
            {
                "adm": "Some html+js here",
                "adomain": [
                    "abc123.com"
                ],
                "price": 1.81767,
                "crid": "12345678",
                "w": 600,
                "h": 250,
                "fmt": 55
            }
        ]
    }
]
}
```

An advantage of this representation is that the bidder can simply iterate on the sizes array and match the w/h/fmt with its internal list of ad formats available for bidding.

As a note Google's AdX protocol provides a facility to solve this problem via an array of w & h sizes.  This has proven to work in practice yet doesn't allow the optional 'format id' to unambiguously specify that a given ad format is supported for bidding (along with all known features of that ad format).
"w": [600,300]
"h": [250,250]


| Format ID | Format Name                     | WxH         | Note |
|:----------|:--------------------------------|:------------|:-----|
|       1   | Banner                          | 468x60             | Fixed size |
|       2   | Leaderboard                     | 728x90             | Fixed size |
|       3   | Half Banner                     | 234x60             | Fixed size |
|       4   | Micro Bar                       | 88x31              | Fixed size |
|       5   | Button 1                        | 120x90             | Fixed size |
|       6   | Button 2                        | 120x60             | Fixed size |
|       7   | Button                          | 125x125            | Fixed size |
|       8   | Skyscraper                      | 120x600            | Fixed size |
|       9   | Wide Skyscraper                 | 160x600            | Fixed size |
|      10   | Half Page Ad                    | 300x600            | Fixed size |
|      12   | Vertical Banner                 | 120x240            | Fixed size |
|      13   | Small Square                    | 200x200            | Fixed size |
|      14   | Square                          | 250x250            | Fixed size |
|      15   | Medium Rectangle                | 300x250            | Fixed size |
|      16   | Large Rectangle                 | 336x280            | Fixed size |
|      17   | Vertical Rectangle              | 240x400            | Fixed size |
|      18   | Rectangle                       | 180x150            | Fixed size |
|      19   | 3:1 Rectangle                   | 300x100            | Fixed size |
|      24   | Vertical Links - 160            | 160x90             | Fixed size |
|      25   | Vertical Links - 180            | 180x90             |
|      26   | Vertical Links - 200            | 200x90             |
|      27   | Horizontal Links - 468          | 468x15             |
|      28   | Horizontal Links - 728          | 728x15             |
|      29   | Large Half Page Ad              | 300x850            |
|      30   | Pixel                           | 1x1                |
|      31   | Swedish Panorama                | 980x120            |
|      32   | Swedish Stortavla               | 250x360            |
|      33   | Norwegian Skyscraper            | 180x500            |
|      35   | Norwegian Top Banner            | 980x150            |
|      36   | Finnish Suurtaulu               | 140x350            |
|      37   | Finnish PikseliÃ                | 468x400            |
|      38   | Danish Megaboard                | 930x180            |
|      39   | Polish Billboard                | 750x100            |
|      40   | Polish Double Billboard         | 750x200            |
|      41   | Polish Triple Billboard         | 750x300            |
|      42   | Skin                            | 2x4                |
|      43   | Mobile Banner 1                 | 320x50             |
|      44   | Mobile Banner 2                 | 300x50             |
|      45   | Mobile Banner 3                 | 480x75             |
|      46   | Mobile Banner 4                 | 480x60             |
|      47   | Mobile Banner 5                 | 240x37             |
|      48   | Square 300                      | 300x300            |
|      49   | Square 336 (deprecated)         | Deprecated 336x280 |
|      50   | Tablet Landscape Banner         | 1024x90            |
|      51   | Tablet Portrait Banner          | 768x90             |
|      52   | Tablet Full Page (Status Bar)   | 1004x768           |
|      53   | Tablet Interstitial Landscape   | 1024x768           |
|      54   | Portrait                        | 300x1050           |
|      55   | Pushdown                        | 970x90             | Expands Down to 970x415|
|      56   | Super Leaderboard               | 900x250            |
|      57   | IAB Billboard                   | 970x250            | Expands Down to 970x500|
|      58   | Leaderboard 2                   | 1000x90            |
|      59   | Mobile Banner 6                 | 320x80             |
|      60   | Mobile Banner 7                 | 320x150            |
|      61   | Full Page                       | 1000x1000          |
|      62   | Leaderboard 3                   | 980x50             |
|      63   | Leaderboard 4                   | 1000x40            |
|      64   | Norwegian Netboard              | 580x500            |
|      65   | Monster                         | 640x480            |
|      66   | Danish Monsterboard             | 930x600            |
|      67   | Mobile Interstitial Portrait    | 320x480            |
|      68   | Wallpaper/Skin                  | 1800x1000          |
|      69   | Artikkelboard                   | 480x400            |
|      70   | Nordic Big Panorama 1           | 1100x210           |
|      71   | Nordic Big Panorama 2           | 852x210            |
|      72   | Swedish Takeover                | 320x320            |
|      73   | Swedish Half Takeover           | 320x160            |
|      74   | Nordic Stortavla                | 1250x240           |
|      75   | Nordic Big Banner               | 1250x250           |
|      76   | Nordic Outsider                 | 265x720            |
|      77   | Nordic Outsider 2               | 265x600            |
|      78   | Double Panorama                 | 980x240            |
|      79   | Monster Panorama                | 980x300            |
|      80   | Finnish Monster Panorama        | 980x400            |
|      81   | Danish Text Links               | 450x150            |
|      82   | Large Mobile Banner             | 300x75             |
|      83   | Czech Wallpaper                 | 480x300            |
|      85   | Mini Rectangle                  | 300x120            |
|      86   | SideKick                        | SideKick 300x250   |
|      87   | Slider                          | Slider 950x90      |
|      88   | FilmStrip                       | FilmStrip 300x600  |
|      89   | Large Portrait                  | 300x1200           |
|      90   | Strip                           | 548x150            |
|      91   | Wallpaper 300x250               | Wallpaper 300x250  |
|      92   | Wallpaper 728x90                | Wallpaper 728x90   |
|      93   | Wallpaper 160x600               | Wallpaper 160x600  |
|      94   | Czech Rectangle                 | 970x310            |
|      95   | Small Czech Leaderboard         | 970x100            |
|      96   | Large Czech Leaderboard         | 970x210            |
|      97   | Seznam HP                       | 467x120            |
|      98   | Russian Leaderboard             | 1000x120           |
|      99   | Slider 300x250                  | Slider 300x250     |
|     100   | Curtain 300x250                 | Curtain 300x250    |
|     101   | Mobile Interstitial Landscape   | 480x320            |
|     102   | Tablet Interstitial Portrait    | 768x1024           |
|     103   | Nordic Helsida                  | 480x280            |
|     104   | Nordic Panorama XXL             | 1250x360           |
|     105   | Nordic Outsider XXL             | 250x800            |
|     106   | Nordic Insider XXL              | 300x480            |
|     107   | Nordic Rectangle                | 440x220            |
|     108   | Mobile Half Page Ad             | 320x240            |
|     109   | Double Banner                   | 468x120            |
|     110   | Custom Pushdown                 | 994x66             |
|     111   | Swedish Big Panorama            | 1250x480           |
|     112   | Skyline                         | 1366x40            |
|     113   | Large Leaderboard               | 1000x300           |
|     114   | Extended Big Rectangle          | 505x500            |
|     115   | Norwegian Surfboard             | 480x150            |
|     116   | Billboard                       | 950x250            |
|     117   | Mobile Double Size Banner       | 320x100            |
|     118   | Custom                          | 160x410            |
|     119   | Giant Skyscraper                | 256x600            |
|     120   | Wideboard                       | 994x250            |
|     121   | Tablet Interstitial Landscape   | 1280x800           |
|     122   | Tablet Interstitial Portrait    | 800x1280           |
|     123   | Airline Boarding Pass           | 650x110            |