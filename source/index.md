---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://api.affin.io' target="_blank">Dashboard</a>
  - <a href='https://affin.io/api-signup/' target="_blank">Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Affinio API! You can use our API to access Affinio Reports, Tribes, Ads, and other endpoints.

We have language bindings in Shell; You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

If you have any questions, please don't hesitate to reach out to <a href="mailto:phil@affin.io" target="_blank">Phil Renaud</a> (or <a href="https://twitter.com/phil_renaud" target="_blank">on twitter</a>).

# Authentication

```shell
# With shell, you can just pass the correct API_KEY with each request
```

<aside class="notice">
We are limiting access to our API to specific partners at the present time. Please reach out to <a href='mailto:phil@affin.io' target="_blank">Phil Renaud</a> to request access.
</aside>

Affinio uses unique keys to allow access to our API. You can register a new Affinio API key at our <a href='https://affin.io/api-signup/' target="_blank">Developer Portal</a>.

# Tribes and Reports

## Get My Reports (and Public Reports)

```shell
curl "https://api.affin.io/v1/campaigns/my_campaigns?api_key=YOUR_API_KEY"
curl "https://api.affin.io/v1/campaigns/my_campaigns?api_key=YOUR_API_KEY&from=0&size=5"
```

> ###Expected Return
>```json
[
  {
    "id": 12345,
    "name": "Affinio Followers Demo Report",
    "number_of_clusters": "20",
    "source": "Twitter",
    "email": "phil@affin.io",
    "belongs_to": ["phil@affin.io","stephen@affin.io"],
    "resource_ids": ["12345_0","12345_1","12345_2","12345_3","...","12345_18","12345_19"],
    "filters": {"bio_location":"Canada","followers_of":"affinioinc,phil_renaud,t1mburke"},
    "members_count": 999999,
    "started": 1428682154
  },
  {
    "id": 12346,
    "name": "Affinio Followers Demo Report 2",
    "number_of_clusters": "8",
    "source": "Instagram",
    "email": "stephen@affin.io",
    "belongs_to": ["phil@affin.io","stephen@affin.io","brian@affin.io"],
    "resource_ids": ["12345_0","12345_1","12345_2","12345_3","...","12345_18","12345_19"],
    "filters": {"followers_of":"mlb"},
    "members_count": 9999999,
    "started": 1428682154
  }
]
```

This endpoint retrieves basic details about all Reports to which you have access.

Note: Pagination of results can be done by using the from and size parameters. The from parameter defines the offset from the first result you want to fetch. The size parameter allows you to configure the maximum amount of campaigns to be returned.
from defaults to 0, and size defaults to 10.

### HTTP Request

`GET http://api.affin.io/v1/campaigns/my_campaigns`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | YOUR_API_KEY | Your API key
from | 0 | The offset from the first result you want to fetch. 
size | 10 | The maximum amount of campaigns to be returned.
created_before | null | Returns all campaigns created before a certain date (in milliseconds)
created_after | null | Returns all campaigns created after a certain date (in milliseconds)













## Get a specific report

```shell
curl "https://api.affin.io/v1/campaigns/campaigns?api_key=YOUR_API_KEY&tribe_id=12345"
```

> ###Expected Return

>```json
{
  "id": 12345,
  "name": "Affinio Followers Demo Report",
  "number_of_clusters": "20",
  "source": "Twitter",
  "email": "phil@affin.io",
  "belongs_to": ["phil@affin.io","stephen@affin.io"],
  "tribes": [
      {
        "id": "12345_0",
        "name": "tribe 1"
      },
      {
        "id": "12345_1",
        "name": "tribe 2"
      },
      {
        "id": "12345_2",
        "name": "tribe 3"
      }
    ],
  "filters": {"bio_location":"Canada","followers_of":"affinioinc,phil_renaud,t1mburke"},
  "members_count": 999999,
  "started": 1428682154
}
```

This endpoint retrieves basic details about a given report to which you have access.

### HTTP Request

`GET http://api.affin.io/v1/campaigns/campaigns`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | YOUR_API_KEY | Your API key
id | n/a | The ID of the campaign you're trying to retrieve





















## Create a report

```shell
curl -H "Content-Type: application/json" -X POST -d '{"api_key":"YOUR_API_KEY", "followers_of":"phil_renaud", "source":"Twitter", "report_type":"network_graph", "name":"Test Report", "number_of_clusters":8}' "https://api.affin.io/v1/campaigns/create_report"

curl -H "Content-Type: application/json" -X POST -d '{"api_key":"YOUR_API_KEY", "followers_of":"phil_renaud", "source":"Twitter", "report_type":"tweet_content", "name":"Test Report", "tweet_content":"AffinioInc"}' "https://api.affin.io/v1/campaigns/create_report"
```

> ###Expected Return

> ```json
{
  "id": 12345,
  "name": "Affinio Followers Demo Report",
  "number_of_clusters": 8,
  "source": "Twitter",
  "email": "phil@affin.io",
  "belongs_to": ["phil@affin.io"],
  "filters": {
        "location":"global", 
        "followers_of":"affinioinc,phil_renaud,t1mburke"
            },
  "members_count": 999999,
  "started": 1428682154
}
```

This endpoint creates a report.

### HTTP Request

`POST http://api.affin.io/v1/campaigns/create_report`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | YOUR_API_KEY | Your API key
source | Twitter | Available sources: Twitter, Instagram, Pinterest
report_type | network_graph | Available report types: network_graph, tweet_content, upload_csv
followers_of | NULL | Required when network_graph is selected as report_type
bio_location | NULL | Optional for network_graph type, not required for other report types
bio_keywords | NULL | Optional for network_graph type, not required for other report types
tweet_content | NULL | Required when tweet_content is selected as report_type
monitored_terms | NULL | Optional
number_of_clusters | 8 | Optional
minInfluencerFollowers | NULL | Optional
maxInfluencerFollowers | NULL | Optional


## Get cluster_svg

```shell
curl "https://api.affin.io/v1/campaigns?api_key=YOUR_API_KEY&id=12345,23456,56789"
```


> ###Expected Return

> ```json
{
  "id": 12345,
  "name": "Affinio Followers Demo Report",
  "cluster_svg": "https://s3-us-west-2.amazonaws.com/com.affinio.reports/485913199/SVGs/cluster.svg"
}
```


This endpoint retrieves the cluster_svg of a campaign or multiple campaigns.

### HTTP Request

`GET http://api.affin.io/v1/campaigns/get_svg`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | YOUR_API_KEY | Your API key
id | n/a | A campaign id or campaign ids separated by comma.























# Tribe Monitoring

## Get Gained/Lost Numbers about a given Tribe or Report

```shell
curl "https://api.affin.io/v1/updates/12345_10"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
{
  "timeline": [
        {"date":1424215563000,"count":2324},
        {"date":1424316109877,"count":2334},
        {"date":1424402595481,"count":2340},
        {"date":1424488938672,"count":2344},
        {"date":1424575309125,"count":2347},
        {"date":1424661720085,"count":2349},
        {"date":1424748181169,"count":2353},
        {"date":1425007423502,"count":2361},
        {"date":1425093891752,"count":2362},
        {"date":1425180132382,"count":2365},
        {"date":1425266575857,"count":2368},
        {"date":1425352921362,"count":2367},
        {"date":1425439393128,"count":2381},
        {"date":1425525746959,"count":2397},
        {"date":1425612128734,"count":2399},
        {"date":1425698633694,"count":2401},
        {"date":1425784975264,"count":2409},
        {"date":1425871390577,"count":2407},
        {"date":1425957799567,"count":2406},
        {"date":1426044230400,"count":2409},
        {"date":1426168105296,"count":2414},
        {"date":1426217028517,"count":2416},
        {"date":1426303308619,"count":2418},
        {"date":1426389754188,"count":2417},
        {"date":1426562543741,"count":2422},
        {"date":1426735529692,"count":2423},
        {"date":1426821734675,"count":2423},
        {"date":1426908013114,"count":2425},
        {"date":1427080839882,"count":2426},
        {"date":1427167327357,"count":2425},
        {"date":1427253706159,"count":2426},
        {"date":1427340075070,"count":2429},
        {"date":1427426472959,"count":2430},
        {"date":1427512857909,"count":2434},
        {"date":1427599239115,"count":2435},
        {"date":1427685698864,"count":2435},
        {"date":1427772160485,"count":2434}
  ]
}
```

Retrieves the members count (often followers count) of a given tribe or report over time. The date property is formated in milliseconds.

### HTTP Request

`GET http://api.affin.io/v1/updates/12345_10`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | n/a | The ID of the tribe or campaign you're trying to retrieve















# Content and Bios

## Top Hashtags

```shell
curl "https://api.affin.io/v1/content/top_hashtags?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "MH17",
    "count": 89
  },
  {
    "term": "Gaza",
    "count": 76
  },
  {
    "term": "WorldCupFinal",
    "count": 76
  },
  {
    "term": "India",
    "count": 74
  }
]
```

Retrieves the top 100 hashtags for a given tribe.

### HTTP Request

`GET https://api.affin.io/v1/content/top_hashtags`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve




## Top Mentions

```shell
curl "https://api.affin.io/v1/content/top_mentions?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "narendramodi",
    "count": 132
  },
  {
    "term": "timesofindia",
    "count": 122
  },
  {
    "term": "YouTube",
    "count": 99
  },
  {
    "term": "PMOIndia",
    "count": 80
  }
]
```

Retrieves the top mentions for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_mentions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve



## Top Tweet Keywords

```shell
curl "https://api.affin.io/v1/content/top_tweetkeywords?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "people",
    "count": 152
  },
  {
    "term": "roof",
    "count": 144
  },
  {
    "term": "time",
    "count": 140
  },
  {
    "term": "today",
    "count": 132
  }
]
```

Retrieves the top tweet keywords for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_tweetkeywords`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve


## Top Bio Keywords

```shell
curl "https://api.affin.io/v1/content/top_biokeywords?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "writer",
    "count": 15
  },
  {
    "term": "nyc",
    "count": 8
  },
  {
    "term": "nerd",
    "count": 8
  },
  {
    "term": "student",
    "count": 8
  },
]
```

Retrieves the top bio keywords for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_biokeywords`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve



## Top Categories

```shell
curl "https://api.affin.io/v1/content/top_categories?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "Gaming",
    "count": 78
  },
  {
    "term": "Services",
    "count": 75
  },
  {
    "term": "Food/Travel",
    "count": 59
  },
  {
    "term": "Health",
    "count": 13
  }
]
```

Retrieves the top categories for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_categories`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve


## Top Apps

```shell
curl "https://api.affin.io/v1/content/top_apps?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term":"Twitter for iPhone",
    "count":910
  },
  {
    "term":"Twitter for Websites",
    "count":845
  },
  {
    "term":"Twitter for Android",
    "count":331
  },
  {
    "term":"Twitter for iPad",
    "count":313
  }
]
```

Retrieves the top apps for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_apps`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve




## Top Favorites

```shell
curl "https://api.affin.io/v1/content/top_favorites?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term":"tbt",
    "count":910
  },
  {
    "term":"oscars",
    "count":845
  },
  {
    "term":"socialmedia",
    "count":331
  },
  {
    "term":"socialmediaweek",
    "count":313
  }
]
```

Retrieves the top favorites for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_favorites`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve


## Top Locations

```shell
curl "https://api.affin.io/v1/content/top_locations?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "London",
    "count": 85
  },
  {
    "term": "Los Angeles",
    "count": 77
  },
  {
    "term": "New York",
    "count": 64
  },
  {
    "term": "Toronto",
    "count": 49
  }
]
```

Retrieves the top locations for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_locations`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve



## Top URLs

```shell
curl "https://api.affin.io/v1/content/top_urls?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "http://www.si.com/nba/2014/07/11/lebron-james-cleveland-cavaliers",
    "count": 24
  },
  {
    "term": "https://twitter.com/i/t/worldcup",
    "count": 22
  },
  {
    "term": "http://bit.ly/1zu0IHz",
    "count": 22
  },
  {
    "term": "https://vine.co/v/MtZ2xhIjDPe",
    "count": 16
  },
  {
    "term": "http://uapp.ly",
    "count": 12
  }
]
```

Retrieves the top URLs for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_urls`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve



## Top Domains

```shell
curl "https://api.affin.io/v1/content/top_domains?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "term": "twitter.com",
    "count": 78
  },
  {
    "term": "washingtonpost.com",
    "count": 31
  },
  {
    "term": "theguardian.com",
    "count": 29
  },
  {
    "term": "theatlantic.com",
    "count": 28
  },
  {
    "term": "cnn.com",
    "count": 26
  }
]
```

Retrieves the top domains for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_domains?api_key=YOUR_API_KEY&tribe_id=12345_10`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve




## Top Influencers

```shell
curl "https://api.affin.io/v1/content/top_influencers?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "id": 11740902,
    "name": "Tim Ferriss",
    "screen_name": "tferriss",
    "score": 64364.455959025
  },
  {
    "id": 115485051,
    "name": "Conan O'Brien",
    "screen_name": "ConanOBrien",
    "score": 19607.535340593
  },
  {
    "id": 16303106,
    "name": "Stephen Colbert",
    "screen_name": "StephenAtHome",
    "score": 17965.386784869
  },
  {
    "id": 15485441,
    "name": "jimmy fallon",
    "screen_name": "jimmyfallon",
    "score": 17958.614459845
  }
]
```

Retrieves the top 1000 influencers for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_influencers`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve




## Competitive Breakdown

```shell
curl "https://api.affin.io/v1/content/top_competitivebreakdown?api_key=YOUR_API_KEY&tribe_id=12345_10"
```

> ###Expected Return

> ```json
[
  {
    "users": [
      {
        "id": 74286565,
        "screen_name": "Microsoft",
        "followers_count": 6553243
      }
    ],
    "size": 142211
  },
  {
    "users": [
      {
        "id": 8633582,
        "screen_name": "Linux",
        "followers_count": 177523
      }
    ],
    "size": 1070
  },
  {
    "users": [
      {
        "id": 74286565,
        "screen_name": "Microsoft",
        "followers_count": 6553243
      },
      {
        "id": 8633582,
        "screen_name": "Linux",
        "followers_count": 177523
      }
    ],
    "size": 669
  }
]
```

Retrieves the competitive breakdown for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_competitivebreakdown`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve










# Affinity Search API

## Tribes influenced by a User

```shell
curl "https://api.affin.io/v1/affinity_search/influenced_by?api_key=YOUR_API_KEY&handle=phil_renaud"
curl "https://api.affin.io/v1/affinity_search/influenced_by?api_key=YOUR_API_KEY&handle=phil_renaud&from=0&size=5"
curl "https://api.affin.io/v1/affinity_search/influenced_by?api_key=YOUR_API_KEY&handle=phil_renaud&ids=12345_1,12345_5"
```

> ###Expected Return

> ```json
[
  {
    "tribe":"Halifax Locals",
    "report":"@Collide_Halifax followers",
    "tribeid":"12345_10",
    "campaignid":"12345",
    "affinity_score": 150305.35,
    "affinity_rank": 8
  },
  {
    "tribe":"Designers/Devs",
    "report":"@Collide_Halifax followers",
    "tribe_id":"12345_3",
    "report_id":"12345",
    "affinity_score": 155305.35,
    "affinity_rank": 1
  },
  {
    "tribe":"Ad Agency Alumni",
    "report":"Formerly Phoenician",
    "tribe_id":"12346_10",
    "report_id":"12346",
    "affinity_score": 150305.35,
    "affinity_rank": 185
  }
]
```

Finds all tribes over which a given user has significant influence.

Note: Pagination of results can be done by using the from and size parameters. The from parameter defines the offset from the first result you want to fetch. The size parameter allows you to configure the maximum amount of hits to be returned.
from defaults to 0, and size defaults to 10.



### HTTP Request

`GET http://api.affin.io/v1/affinity_search/influenced_by`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API Key
handle | n/a | The handle of the user you're looking for
ids | NULL | Tribe ids separated by comma. Note: If no tribe ids are passed in, all user owned campaigns will be searched. 
from | 0 | The offset from the first result you want to fetch. 
size | 10 | The maximum amount of campaigns to be returned.



## Tribes containing a Member

```shell
curl "https://api.affin.io/v1/affinity_search/contains_member?api_key=YOUR_API_KEY&handle=phil_renaud"
```

> ###Expected Return

> ```json
[
  {
    "tribe":"Halifax Locals",
    "report":"@Collide_Halifax followers",
    "tribe_id":"12345_10",
    "report_id":"12345"
  },
  {
    "tribe":"Designers/Devs",
    "report":"@Collide_Halifax followers",
    "tribe_id":"12345_3",
    "report_id":"12345"
  },
  {
    "tribe":"Ad Agency Alumni",
    "report":"Formerly Phoenician",
    "tribe_id":"12346_10",
    "report_id":"12346"
  }
]
```

Finds all tribes to which a given user belongs.

Note: Pagination of results can be done by using the from and size parameters. The from parameter defines the offset from the first result you want to fetch. The size parameter allows you to configure the maximum amount of hits to be returned.
from defaults to 0, and size defaults to 10.



### HTTP Request

`GET http://api.affin.io/v1/affinity_search/contains_member`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API Key
handle | n/a | The handle of the user you're looking for
ids | NULL | Tribe ids separated by comma. Note: If no tribe ids are passed in, all user owned campaigns will be searched. 
from | 0 | The offset from the first result you want to fetch. 
size | 10 | The maximum amount of campaigns to be returned.






## Tribes that use a term

```shell
curl "https://api.affin.io/v1/affinity_search/uses_term?api_key=YOUR_API_KEY&term=javascript&metric=hashtag"
```

> ###Expected Return

> ```json
[
  {
    "tribe":"Halifax Locals",
    "report":"@Collide_Halifax followers",
    "tribeid":"12345_10",
    "campaignid":"12345",
    "rank": 3,
    "number_using": 435
  },
  {
    "tribe":"Designers/Devs",
    "report":"@Collide_Halifax followers",
    "tribe_id":"12345_3",
    "report_id":"12345",
    "rank": 3,
    "number_using": 435
  },
  {
    "tribe":"Ad Agency Alumni",
    "report":"Formerly Phoenician",
    "tribe_id":"12346_10",
    "report_id":"12346",
    "rank": 3,
    "number_using": 435
  }
]
```

Finds all tribes using a specific term (hashtag, mention, etc.)

### HTTP Request

`GET http://api.affin.io/v1/affinity_search/uses_term`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API Key
term | n/a | The term you're looking for
metric | 'hashtag' | The type of metric you're looking up
ids | NULL | Tribe ids separated by comma. Note: If no tribe ids are passed in, all user owned campaigns will be searched. 

<aside class="notice">
Possible metrics include "hashtag", "mention", "keyword", "bio_keyword", "location", "url", and "domain".
</aside>













# Cross-Platform API

## All known Social Media accounts of a given user

```shell
curl "https://api.affin.io/v1/cross_platform/cross_platform?api_key=YOUR_API_KEY&network=twitter&handle=phil_renaud"
```

> ###Expected Return

> ```json
{
  "twitter":"phil_renaud",
  "twitter_id":"123456789",
  "instagram":"phil_renaud",
  "youtube":null,
  "facebook":"philrenaud",
  "pinterest":"phil_renaud",
  "googleplus":"phil_renaud",
  "tumblr":"philrenaud"
}
```

Finds all social media account handles / IDs for a given user. Assumes twitter by search.

### HTTP Request

`GET http://api.affin.io/v1/cross_platform/cross_platform

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
handle | n/a | The handles of users you're looking for, handles are separated by comma
network | "twitter" | The network that the aforementioned handle belongs to











# Classification API

## Profile a user by analyzing content and favorites

```shell
curl "https://api.affin.io/v1/classify/phil_renaud"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
{
  "technology": 21.5,
  "advertising": 17.85,
  "design": 15.51,
  "...":"...",
  "fashion": 4.31
}
```

Reads tweets and favorites by a given user and uses machine learning / classification to determine their personality archetype.

### HTTP Request

`GET http://api.affin.io/v1/classify/phil_renaud`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
handle | n/a | The handle of the user you're looking for



<aside class="notice">
Possible classifications include "Technophiles", "Auto Enthusiasts", "Avid Investors", "Foodies", "Fashionistas", ...
</aside>









# Misc

## Translate a Country into Cities/Regions

```shell
curl "https://api.affin.io/v1/citify/Canada"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
[
  "Canada",
  "Toronto",
  "Vancouver",
  "Montreal",
  "Ontario",
  "Halifax",
  "Quebec",
  "British Columbia",
  "..."
]
```

Translates a country or region into smaller parts (regions, cities, neighbourhoods).

### HTTP Request

`GET http://api.affin.io/v1/citify/canada`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
country | n/a | The country to dive into

