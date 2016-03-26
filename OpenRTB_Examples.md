# Single impression PC Request #

```
{
    "id": "80ce30c53c16e6ede735f123ef6e32361bfc7b22",
    "imp": [
        {
            "id": "1",
            "banner": {
                "h": 250,
                "w": 300,
                "pos": 0
            },
            "bidfloor": 0.03,
            "pmp": {
                "private_auction": 1,
                "deals": [
                    {
                        "id": "DX-1985-010A",
                        "bidfloor": 2.5,
                        "at": 2
                    }
                ]
            }
        }
    ],
    "site": {
        "id": "102855",
        "domain": "http://www.usabarfinder.com",
        "cat": "IAB3-1",
        "page": "http://eas.usabarfinder.com/eas?cu=13824;cre=mu;target=_blank",
        "publisher": {
            "id": "8953",
            "name": "local.com",
            "cat": "IAB3-1",
            "domain": "local.com"
        }
    },
    "device": {
        "ua": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/537.13  (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
        "ip": "123.145.167.*"
    },
    "user": {
        "id": "55816b39711f9b5acf3b90e313ed29e51665623f"
    },
    "at": 1,
    "cur": [
        "USD"
    ],
    "regs": {
        "coppa": 1
    },
    
}
```

# PC Multiple Impression Request #
```
{
    "id": "8652a8680db33faabbf3fa76150f35df50a67060",
    "imp": [
        {
            "id": "121-dt1",
            "banner": {
                "h": 250,
                "w": 300,
                "pos": 1
            },
            "bidfloor": 0.05
        },
        {
            "id": "121-dt2",
            "banner": {
                "h": 728,
                "w": 90,
                "pos": 0
            },
            "bidfloor": 0.12
        }
    ],
    "site": {
        "id": "15047",
        "domain": "dailymotion.com",
        "cat": "IAB1",
        "page": "http://www.dailymotion.com/video/xxeauj_www-dramacafe-tv-hd-yyyy-yy-yyyyyyy-2012-yyyy_shortfilms",
        "publisher": {
            "id": "8796",
            "name": "dailymotion",
            "cat": "IAB3-1",
            "domain": "dailymotion.com"
        }
    },
    "user": {
        "id": "518c3da3717203f34019b038",
    },
    "device": {
        "ua": "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0; GTB7.4; (R1 1.6); SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0)",
        "ip": "123.145.167.189"
    },
    "at": 1,
    "cur": [
        "USD"
    ]
}
```

# PC Bid Response with Win Notification #

```
{
    "id": "BID-4-ZIMP-4b309eae-504a-4252-a8a8-4c8ceee9791a",
    "seatbid": [
        {
            "bid": [
                {
                    "id": "32a69c6ba388f110487f9d1e63f77b22d86e916b",
                    "impid": "32a69c6ba388f110487f9d1e63f77b22d86e916b",
                    "price": 0.065445,
                    "adid": "529833ce55314b19e8796116",
                    "nurl": "http://ads.com/win/529833ce55314b19e8796116?won=${AUCTION_PRICE}",
                    "adm": "<iframe src=\"http://ads.com/render/529833ce55314b19e8796116\" width=\"300\" height=\"250\" frameborder=\"0\" scrolling=\"no\" marginheight=\"0\" marginwidth=\"0\" topmargin=\"0\" leftmargin=\"0\"></iframe>",
                    "adomain": [
                        "ads.com"
                    ],
                    "cid": "529833ce55314b19e8796116",
                    "crid": "529833ce55314b19e8796116_1385706446",
                    "attr": []
                }
            ],
            "seat": "772"
        }
    ],
    "cur": "USD"
}
```

# PC Multiple Bid Response #
```
{
    "id": "BID-4-ZIMP-4b309eae-504a-4252-a8a8-4c8ceee9791a",
    "seatbid": [
        {
            "bid": [
                {
                    "id": "24195efda36066ee21f967bc1de14c82db841f07",
                    "impid": "24195efda36066ee21f967bc1de14c82db841f07",
                    "price": 1.028428,
                    "adid": "52a12b5955314b7194a4c9ff",
                    "nurl": "http://ads.com/win/52a12b5955314b7194a4c9ff?won=${AUCTION_PRICE}",
                    "adm": "<iframe src=\"http://ads.com/render/52a12b5955314b7194a4c9ff?won=${AUCTION_PRICE}\" width=\"728\" height=\"90\" frameborder=\"0\" scrolling=\"no\" marginheight=\"0\" marginwidth=\"0\" topmargin=\"0\" leftmargin=\"0\"></iframe>",
                    "adomain": [
                        "ads.com"
                    ],
                    "cid": "52a12b5955314b7194a4c9ff",
                    "crid": "52a12b5955314b7194a4c9ff_1386294105",
                    "attr": [],
                    "dealid": "DX-1985-010A"
                }
            ],
            "seat": "42"
        },
        {
            "bid": [
                {
                    "id": "24195efda36066ee21f967bc1de14c82db841f08",
                    "impid": "24195efda36066ee21f967bc1de14c82db841f08",
                    "price": 0.04958,
                    "adid": "527c9fdd55314ba06815f25e",
                    "nurl": "http://ads.com/win/527c9fdd55314ba06815f25e?won=${AUCTION_PRICE}",
                    "adm": "<iframe src=\"http://ads.com/render/527c9fdd55314ba06815f25e?won=${AUCTION_PRICE}\" width=\"300\" height=\"250\" frameborder=\"0\" scrolling=\"no\" marginheight=\"0\" marginwidth=\"0\" topmargin=\"0\" leftmargin=\"0\"></iframe>",
                    "adomain": [
                        "ads.com"
                    ],
                    "cid": "527c9fdd55314ba06815f25e",
                    "crid": "527c9fdd55314ba06815f25e_1383899102",
                    "attr": []
                }
            ],
            "seat": "772"
        }
    ],
    "cur": "USD"
}

```

# Mobile Bid Request #

```
{
    "id": "IxexyLDIIk",
    "imp": [
        {
            "id": "1",
            "banner": {
                "w": 728,
                "h": 90,
                "pos": 1,
                "btype": [
                    4
                ],
                "battr": [
                    14
                ],
                "api": [
                    3
                ]
            },
            "instl": 0,
            "tagid": "agltb3B1Yi1pbmNyDQsSBFNpdGUY7fD0FAw",
            "bidfloor": 0.5
        }
    ],
    "app": {
        "id": "agltb3B1Yi1pbmNyDAsSA0FwcBiJkfIUDA",
        "name": "Yahoo Weather",
        "cat": [
            "weather",
            "IAB15",
            "IAB15-10"
        ],
        "ver": "1.0.2",
        "bundle": "628677149",
        "publisher": {
            "id": "agltb3B1Yi1pbmNyDAsSA0FwcBiJkfTUCV",
            "name": "yahoo",
            "domain": "www.yahoo.com"
        },
        "storeurl": "https://itunes.apple.com/id628677149"
    },
    "device": {
        "dnt": 0,
        "ua": "Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3",
        "ip": "123.145.167.189",
        "geo": {
            "country": "USA",
            "lat": 35.012345,
            "lon": -115.12345,
            "city": "Los Angeles",
            "metro": "803",
            "region": "CA",
            "zip": "90049"
        },
        "dpidsha1": "AA000DFE74168477C70D291f574D344790E0BB11",
        "dpidmd5": "AA003EABFB29E6F759F3BDAB34E50BB11",
        "carrier": "310-410",
        "language": "en",
        "make": "Apple",
        "model": "iPhone",
        "os": "iOS",
        "osv": "6.1",
        "js": 1,
        "connectiontype": 3,
        "devicetype": 1
    },
    "user": {
        "id": "ffffffd5135596709273b3a1a07e466ea2bf4fff",
        "yob": "1984",
        "gender": "M"
    },
    "at": 2,
    "bcat": [
        "IAB25",
        "IAB7-39",
        "IAB8-18",
        "IAB8-5",
        "IAB9-9"
    ],
    "badv": [
        "apple.com",
        "go-text.me",
        "heywire.com"
    ]
}
```

# Mobile Bid Response #

```
{
    "id": "IxexyLDIIk",
    "seatbid": [
        {
            "bid": [
                {
                    "id": "1",
                    "impid": "1",
                    "price": 0.751371,
                    "adid": "52a5516d29e435137c6f6e74",
                    "nurl": "http://ads.com/win/112770_1386565997?won=${AUCTION_PRICE}",
                    "adm": "<a href=\"http://ads.com/click/112770_1386565997\"><img src=\"http://ads.com/img/112770_1386565997?won=${AUCTION_PRICE}\" width=\"728\" height=\"90\" border=\"0\" alt=\"Advertisement\" /></a>",
                    "adomain": [
                        "ads.com"
                    ],
                    "iurl": "http://ads.com/112770_1386565997.jpeg",
                    "cid": "52a5516d29e435137c6f6e74",
                    "crid": "52a5516d29e435137c6f6e74_1386565997",
                    "attr": []
                }
            ],
            "seat": "2"
        }
    ],
    "cur": "USD"
}
```

# Thanks #

  * Neal Richter - Rubicon Project
  * Seth Yates - BrandScreen