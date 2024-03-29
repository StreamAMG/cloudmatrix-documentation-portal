# Search and Feed APIs

Cloud Matrix allows you to return video content via two feeds.

- **Feed API** uses predefined queries that are setup via the Cloud Matrix UI
- **Search API** allows you to pass in query parameters to return specific content

The feed API represents a more plug and play route using the Cloud Matrix UI  to define the queries.  In addition it offers preview functionality to test the response against publication settings such as hold backs and geo restrictions.

The search API represents a more flexible integration that lends itself well to personalisation and video archive pages with multiple filters.

## Base URL
Your Cloud Matrix URL will be created as part of the platform setup.

```https://{client-cloudmatrix-instance}/api/v1/```

## Path parameters

The following are setup via the Cloud Matrix UI and relate to path parameters for both the Feed API and Search API.

**Access**

**apiUserId** (UUID): required

- A value identifying requesting client
- Example value: e166df2b-4128-4557-9985-525ec361d009

```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/```

**apiKey** (string) : required

- An encrypted value identifying requesting client
- Example value: qj5UGTIibwpJyVldA4Caaj9geXFwpDntEGnLp2UcR95U08fjbo

```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/```

---
**Publication, holdbacks and geo restrictions**

**targetId** (UUID) : optional

- A value identifying the target that videos are published against.  These include publishing holdbacks and regional restriction.   A target could be view as either a location on a page such as highlights carousel or as an application such as website, App, Connected TV.  If no target is applied then the videos will be unrestricted
- Example value: 7086g6f6-e6gy-2621-9e62-305d9eb2bf01

```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/{targetId}/```

---
**Language**

**languageCode** (string) : required

- Two chars ASCII 639-1 language code
- Example value: en

```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/{targetId}/{languageCode}/```

---
**Preconfigured feeds (Feed API)**

**feedId** (UUID) : required

- A  value identifying the predefined feed via the Cloud Matrix UI which contains the query to retrieve videos
- Example value: 76h756w0-0471-828s-r17e-dy732790fadf

```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{targetId}/{languageCode}/feed/{feedId}```



## query parameters

**pageIndex** (integer)

- The page number to be retrieved
- Example value: 0

```?pageIndex=0&pagesize=10```

---
**pageSize** (integer)

- Number of items per page
- Example value: 10

```?pageIndex=0&pagesize=10```

---
**query** (string)

- Elastic search query string query.
- The field must be qualified with the section in the body
- Example value:  metadata.title

<!-- theme: info -->

> #### Search API
> When using the search API include the 'search' path before the query that follows the language code path parameter


```search/?query=mediaData.MediaType:(OnDemand)```

---
**sortBy** (string)

- The field must be qualified with the section in the body
- Example value: publicationData.releaseFrom

```?sortBy={sortby}&sortOrder={sortorder}```

---
**sortOrder** (string)

- Should be ascending or descending
- Example value: asc

```?sortBy={sortby}&sortOrder={sortorder}```


## Some example calls

- Search API (Unrestricted) with query parameters
```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/{languageCode}/search?query=mediaData.MediaType:(OnDemand)```

- Search API (Restricted) with query parameters
```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/{targetId}/{languageCode}/search?query=mediaData.MediaType:(OnDemand)```

- Feed API (Unrestricted) with query parameters
```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/{languageCode}/feed/{feedId}/?pageIndex=0&pageSize=10```

- Feed API (Restricted) with query parameters
```https://{client-cloudmatrix-instance}/api/v1/{apiUserId}/{apiKey}/{targetId}/{languageCode}/feed/{feedId}/?pageIndex=0&pageSize=10```