---
title: API Reference

language_tabs:
  - shell
  - javascript
  - python

toc_footers:
  - <a href='https://api.affin.io' target="_blank">Dashboard</a>
  - <a href='https://affin.io/api-signup/' target="_blank">Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

aHi this is a test

Welcome to the Affinio API! You can use our API to access Affinio Reports, Tribes, Ads, and other endpoints.

We have language bindings in Shell, Python, and Javascript; You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

If you have any questions, please don't hesitate to reach out to <a href="mailto:phil@affin.io" target="_blank">Phil Renaud</a> (or <a href="https://twitter.com/phil_renaud" target="_blank">on twitter</a>).

# Authentication

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
```

```shell
# With shell, you can just pass the correct header with each request
curl "API_ENDPOINT_HERE"
  -H "Authorization: YOUR_API_KEY"
```

<aside class="notice">
We are limiting access to our API to specific partners at the present time. Please reach out to <a href='mailto:phil@affin.io' target="_blank">Phil Renaud</a> to request access.
</aside>

Affinio uses unique keys to allow access to our API. You can register a new Affinio API key at our <a href='https://affin.io/api-signup/' target="_blank">Developer Portal</a>.

# Tribes and Reports

## Get My Reports (and Public Reports)

<!-- ```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.campaigns.get()
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.campaigns.get()
``` -->

```shell
curl "https://api.affin.io/v1/campaigns"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
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

### HTTP Request

`GET http://api.affin.io/v1/campaigns`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
owned_only | false | If set to true, the results will be limited to those reports you created yourself.
created_before | null | Returns all campaigns created before a certain date (in milliseconds)
created_after | null | Returns all campaigns created after a certain date (in milliseconds)
public | false | Returns public-domain tribes published by Affinio, such as "Digital Strategists" and "Fashionistas". Useful for testing.











## Get a specific report

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.campaigns.get(12345)
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.campaigns.get(12345)
```

```shell
curl "https://api.affin.io/v1/campaigns/12345"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
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
}
```

This endpoint retrieves basic details about a given report to which you have access.

### HTTP Request

`GET http://api.affin.io/v1/campaigns/12345`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | n/a | The ID of the campaign you're trying to retrieve




## Get All Tribes of a Report

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.tribes.get(12345)
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.tribes.get(12345)
```

```shell
curl "https://api.affin.io/v1/tribes"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
[
  {
    "id": "12345_0",
    "name": "Couponing Moms",
    "size": 23451,
    "density": 1.33,
    "average_affinity_score": 25.51,
    "tweets_per_month": 135.54
  },
  {
    "id": "12345_1",
    "name": "Fashionistas",
    "size": 23451,
    "density": 1.33,
    "average_affinity_score": 25.51,
    "tweets_per_month": 135.54
  }
]
```

This endpoint retrieves information about all Tribes of a given Report.

### HTTP Request

`GET http://api.affin.io/v1/tribes/12345`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
ID | false | The ID of the report from which you'd like tribes

<aside class="success">
If the ID is in a tribe format ("12345_5" as opposed to "12345"), a single tribe will be returned.
</aside>















# Tribe Monitoring

## Get Gained/Lost Numbers about a given Tribe or Report

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.updates.get(12345_10)
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.updates.get(12345_10)
```

```shell
curl "https://api.affin.io/v1/updates/12345_10"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
{
  "timeline": [
{"date":1424215563000,"count":2324},{"date":1424316109877,"count":2334},{"date":1424402595481,"count":2340},{"date":1424488938672,"count":2344},{"date":1424575309125,"count":2347},{"date":1424661720085,"count":2349},{"date":1424748181169,"count":2353},{"date":1425007423502,"count":2361},{"date":1425093891752,"count":2362},{"date":1425180132382,"count":2365},{"date":1425266575857,"count":2368},{"date":1425352921362,"count":2367},{"date":1425439393128,"count":2381},{"date":1425525746959,"count":2397},{"date":1425612128734,"count":2399},{"date":1425698633694,"count":2401},{"date":1425784975264,"count":2409},{"date":1425871390577,"count":2407},{"date":1425957799567,"count":2406},{"date":1426044230400,"count":2409},{"date":1426168105296,"count":2414},{"date":1426217028517,"count":2416},{"date":1426303308619,"count":2418},{"date":1426389754188,"count":2417},{"date":1426562543741,"count":2422},{"date":1426735529692,"count":2423},{"date":1426821734675,"count":2423},{"date":1426908013114,"count":2425},{"date":1427080839882,"count":2426},{"date":1427167327357,"count":2425},{"date":1427253706159,"count":2426},{"date":1427340075070,"count":2429},{"date":1427426472959,"count":2430},{"date":1427512857909,"count":2434},{"date":1427599239115,"count":2435},{"date":1427685698864,"count":2435},{"date":1427772160485,"count":2434},{"date":1427858469772,"count":2433},{"date":1427944879772,"count":2434},{"date":1428031231275,"count":2437},{"date":1428117629611,"count":2438},{"date":1428204143810,"count":2438},{"date":1428290428330,"count":2438},{"date":1428376870161,"count":2439},{"date":1428463308706,"count":2440},{"date":1428549736858,"count":2441},{"date":1428636107319,"count":2440},{"date":1428722489094,"count":2442},{"date":1428808840243,"count":2442},{"date":1428895287584,"count":2443},{"date":1428981693969,"count":2444}
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

## Most Discussed Topics


```shell
curl "https://api.affin.io/v1/topics/12345_10"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
[
  {
    "topic":"Hillary Clinton is running for President in 2016",
    "percent_discussing":9.13,
    "sentiment":65.53,
    "tweets_discussing":["120938123","120938123","120938123","120938123"]
  },
  {
    "topic":"Hillary Swank wins Best Actress",
    "percent_discussing":8.51,
    "sentiment":91.33,
    "tweets_discussing":["120938123","120938123","120938123","120938123"]
  },
  {
    "topic":"Affinio is Providing Rad Documentation",
    "percent_discussing":3.31,
    "sentiment":100,
    "tweets_discussing":["120938123","120938123","120938123","120938123"]
  },
  {
    "topic":"Snowstorms affect many flights",
    "percent_discussing":3.14,
    "sentiment":11.15,
    "tweets_discussing":["120938123","120938123","120938123","120938123"]
  }
]
```

Performs a Topic Modelling analysis of a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/topics/12345_10`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | n/a | The ID of the tribe or campaign you're trying to retrieve


## Top Hashtags


```shell
curl "https://api.affin.io/v1/content/top_hashtags?api_key=YOUR_API_KEY&tribe_id=12345_10"
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

Retrieves the top 100 hashtags for a given tribe or report.

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

Retrieves the top mentions for a given tribe or report.

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

Retrieves the top tweet keywords for a given tribe or report.

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

Retrieves the top bio keywords for a given tribe or report.

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

Retrieves the top 100 hashtags for a given tribe or report.

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

Retrieves the top 100 hashtags for a given tribe or report.

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

Retrieves the top favorites for a given tribe or report.

### HTTP Request

`GET http://api.affin.io/v1/content/top_favorites`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve


## Top Locations

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```shell
curl "https://api.affin.io/v1/content/top_locations?api_key=YOUR_API_KEY&tribe_id=12345_10"
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

Retrieves the top locations for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_locations`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve





## Top URLs

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```shell
curl "https://api.affin.io/v1/content/top_urls?api_key=YOUR_API_KEY&tribe_id=12345_10"
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

Retrieves the top URLs for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_urls`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve



## Top Domains

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```shell
curl "https://api.affin.io/v1/content/top_domains?api_key=YOUR_API_KEY&tribe_id=12345_10"
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

Retrieves the top domains for a given tribe.

### HTTP Request

`GET http://api.affin.io/v1/content/top_domains?api_key=YOUR_API_KEY&tribe_id=12345_10`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve




## Top Influencers

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```shell
curl "https://api.affin.io/v1/content/top_influencers?api_key=YOUR_API_KEY&tribe_id=12345_10"
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

Retrieves the top 1000 influencers for a given tribe.

### HTTP Request

`GET http://api.affin.io/content/top_influencers`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API key
tribe_id | n/a | The ID of the tribe you're trying to retrieve




## Competitive Breakdown

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.hashtags.get("12345_10")
```

```shell
curl "https://api.affin.io/v1/content/top_competitivebreakdown?api_key=YOUR_API_KEY&tribe_id=12345_10"
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

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.influenced_by.get("phil_renaud")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.influenced_by.get("phil_renaud")
```

```shell
curl "https://api.affin.io/v1/influenced_by?api_key=YOUR_API_KEY&handle=phil_renaud"
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

### HTTP Request

`GET http://api.affin.io/v1/affinity_search/influenced_by`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API Key
handle | n/a | The handle of the user you're looking for


## Tribes containing a Member

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.contains_member.get("phil_renaud")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.contains_member.get("phil_renaud")
```

```shell
curl "https://api.affin.io/v1/contains_member?api_key=YOUR_API_KEY&handle=phil_renaud"
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

### HTTP Request

`GET http://api.affin.io/v1/contains_member`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API Key
handle | n/a | The handle of the user you're looking for





## Tribes that use a Hashtag

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.uses_term.get("tbt")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.uses_term.get("tbt")
```

```shell
curl "https://api.affin.io/v1/uses_term?api_key=YOUR_API_KEY&term=javascript&metric=hashtag"
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

`GET http://api.affin.io/v1/uses_term`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
api_key | n/a | Your API Key
term | n/a | The term you're looking for
metric | 'hashtag' | The type of metric you're looking up

<aside class="notice">
Possible metrics include "hashtag", "mention", "keyword", "bio_keyword", "location", "url", and "domain".
</aside>













# Cross-Platform API

## All known Social Media accounts of a given user

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.cross_platform.get("phil_renaud")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.cross_platform.get("phil_renaud")
```

```shell
curl "https://api.affin.io/v1/cross_platform/phil_renaud"
  -H "Authorization: YOUR_API_KEY"
```

> ###Expected Return

> ```json
{
  "twitter":"phil_renaud",
  "twitter_id":"123456789",
  "instagram":"phil_renaud",
  "email":"phil@affin.io",
  "flickr":"philrenaud1984",
  "youtube":null,
  "facebook":"philrenaud",
  "dribbble":"phil_renaud",
  "pinterest":"phil_renaud",
  "angellist": "philrenaud0"
}
```

Finds all social media account handles / IDs for a given user. Assumes twitter by search.

### HTTP Request

`GET http://api.affin.io/v1/cross_platform/phil_renaud`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
handle | n/a | The handle of the user you're looking for
network | "twitter" | The network that the aforementioned handle belongs to











# Classification API

## Profile a user by analyzing content and favorites

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.classify.get("phil_renaud")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.classify.get("phil_renaud")
```

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

```javascript
require 'affinio'
api = Affinio.APIClient.authorize('YOUR_API_KEY')
api.citify.get("Canada")
```

```python
import affinio
api = affinio.authorize('YOUR_API_KEY')
api.citify.get("Canada")
```

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

