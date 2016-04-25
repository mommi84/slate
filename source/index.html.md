---
title: BuzzTweet - Private API Reference

language_tabs:
  - json

toc_footers:
  - API for &copy; BuzzTweet v0.2
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to BuzzTweet API! The source code for this API documentation is available [here](https://github.com/mommi84/slate).

# Authentication

To authorize, just append the API key provided via email to each request:

`https://<endpoint>?<query>&api_key=<apikey>`


# Endpoints

## Add new customer

> The returned data is structured like this:

```json
{
  "status": "success",
  "data": {
    "customer_id": 81
  }
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer name already in use."
}
```

Create a new customer. An assigned identifier is returned as JSON data.

### HTTP Request

`POST https://<endpoint>/addcustomer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_name | - | The customer name, which must be unique.
search_terms | - | Terms for the search on Twitter, separated by `\n`.
api_key | - | The BuzzTweet API key.

## Edit customer

> The returned data is structured like this:

```json
{
  "status": "success"
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer identifier not found."
}
```

Edit customer details.

### HTTP Request

`POST https://<endpoint>/editcustomer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id | - | The customer internal identifier.
customer_name | null | The customer name. If `null`, the old value will be preserved.
search_terms | null | Terms for the search on Twitter, separated by `\n`. If `null`, the old value will be preserved.
api_key | - | The BuzzTweet API key.

## Delete customer

> The returned data is structured like this:

```json
{
  "status": "success"
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer identifier not found."
}
```

Delete customer and all data associated with it.

### HTTP Request

`DELETE https://<endpoint>/deletecustomer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id | - | The customer internal identifier.
api_key | - | The BuzzTweet API key.

## Upload JSON tweets into DB

> The returned data contains the identifier associated with the run; it is structured like this:

```json
{
  "status": "success",
  "data": {
    "run_id": 123
  }
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "JSON file could not be parsed."
}
```

Upload JSON file containing tweets into the database. Sentiment will be predicted using the default model.

### HTTP Request

This is a `POST` request using `multipart/form-data` content type.

`POST https://<endpoint>/uploadjson`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
target_file | - | The target file in `multipart/form-data` encryption. The file extension can be either `.json` or `.zip` (a zip file containing compressed JSON files).
customer_id | - | The customer internal identifier.
api_key | - | The BuzzTweet API key.

## Start crawler

> The returned data is structured like this:

```json
{
  "status": "success",
  "data": {
    "run_id": 234
  }
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Crawler could not start."
}
```

Start the crawler for a given customer, loading crawled tweets into the database.

### HTTP Request

`GET https://<endpoint>/startcrawler`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id | - | The customer internal identifier.
api_key | - | The BuzzTweet API key.

## Stop crawler

> The returned data is structured like this:

```json
{
  "status": "success"
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Crawler was not running."
}
```

Stop the crawler for a given run.

### HTTP Request

`GET https://<endpoint>/stopcrawler`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
run_id | - | The run internal identifier.
api_key | - | The BuzzTweet API key.

## Compute indexes

> The returned data is structured like this:

```json
{
  "status": "success"
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer not found."
}
```

Start the index computation for a given customer, on a given language. Indexes are then saved into the database.

### HTTP Request

`GET https://<endpoint>/indexes`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id | - | The customer internal identifier.
language | en | The language identifier (e.g. "en", "de", "it").
count_tweets | true | Perform tweet count by label during hashtag analysis.
interval | 604800 | Index time interval in seconds. Values: 1 week (default) = 604800; 1 day = 86400; 1 hour = 3600.
api_key | - | The BuzzTweet API key.

## Check index computation

> The returned data is structured like this:

```json
{
  "status": "success",
  "data": {
    "computation": "done"
  }
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer not found."
}
```

Check whether an index computation is done or still running.

### HTTP Request

`GET https://<endpoint>/indexcomputation`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id | - | The customer internal identifier.
api_key | - | The BuzzTweet API key.

## Compute influencers within an interval

> The returned data is structured like this:

```json
{
  "status": "success"
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer not found."
}
```

Compute influencers for a given customer within an interval.

### HTTP Request

`GET https://<endpoint>/influencers`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
customer_id | - | The customer internal identifier.
language | en | The language identifier (e.g. "en", "de", "it").
start_time | - | Start time in format 'YYYY-MM-DD HH:MM:SS'.
end_time | - | End time in format 'YYYY-MM-DD HH:MM:SS'.
api_key | - | The BuzzTweet API key.

## Upload and correlate time series

> The returned data is structured like this:

```json
{
  "status": "success"
}
```

> In case of exceptions, a message is attached. Example:

```json
{
  "status": "error",
  "message": "Customer not found."
}
```

Upload file with outcomes as time series and correlate them with the general sentiment trend for a given customer. This endpoint triggers the general sentiment trend computation for a customer.

### HTTP Request

This is a `POST` request using `multipart/form-data` content type.

`POST https://<endpoint>/correlate`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
outcome_file | - | The outcome file in `multipart/form-data` encryption. The file must be in `.txt` format and have a header like the following: `length<tab>#` (where `length` is the number of data points).
customer_id | - | The customer internal identifier.
api_key | - | The BuzzTweet API key.

