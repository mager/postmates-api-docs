## Introduction

To get started using the Postmates Delivery API:

*   [Sign up for a developer account](https://postmates.com/developer/register).
*   Locate your customer ID and test API key in the [developer dashboard](https://postmates.com/developer/apikey).
*   Check out a few tutorials on how to use the API: [cURL](http://engineering.postmates.com/Postmates-API/), [Postman](http://engineering.postmates.com/Testing-Postmates-API-Postman/).


### What can the API do?

You can use the Postmates API to utilize our fleet of couriers to deliver your products within our geographic zones.

Here is the flow of events that developers most commonly follow when using our API:

*   To see if a delivery is possible, request a [delivery quote](https://postmates.com/developer/docs/endpoints#get_quote). This endpoint takes in two addresses and returns a fee, ETA, and other information about a potential delivery.
*   After you have evaluated whether the quoted price and delivery estimate meets your needs, you can [create a delivery](https://postmates.com/developer/docs/endpoints#create_delivery).
*   While a delivery is in progress, you can track its status in real-time in the developer dashboard, by [polling the API](https://postmates.com/developer/docs/endpoints#list_deliveries), or with [webhooks](https://postmates.com/developer/docs#webhooks).


## Authentication

The Postmates API requires authentication by [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication) headers. Your API key should be included as the username. The password should be left empty.

The actual header that is used will be a base64-encoded string like this:

<pre>Basic Y2YyZjJkNmQtYTMxNC00NGE4LWI2MDAtNTA1M2MwYWYzMTY1Og==</pre>

Most of the [endpoints](https://postmates.com/developer/docs/endpoints) provided in the Postmates API are in relation to a specific customer. You'll need to provide your `customer id` and include it in the URL.


## Requests

The base URL for all requests to the Postmates API is:

<pre>https://api.postmates.com/</pre>

Our API is REST-based. This means:

*   It make use of standard HTTP verbs like GET, POST, DELETE.
*   The API uses standard HTTP error responses to describe errors.
*   Authentication is specified with HTTP Basic Authentication.

All requests use standard query encoding.

POST data should be encoded as standard `application/x-www-form-urlencoded`.

### Versioning

Versioning allows us to provide developers a consistent experience. We provide two levels of versioning:

*   Resource: All endpoints are prefixed with a version such as `/v1`. This version refers to the overall layout of the endpoints and response standards.
*   Client: Developers can ensure consistent fields and formats by specifying a version as a HTTP header.
    *   `X-Postmates-Version` header such as `X-Postmates-Version: 20140825`


## Responses

The Postmates API uses HTTP status codes to indicate the status of your requests. This includes successful and unsuccessful responses.

All responses are in [JSON](http://www.json.org/).

### Status Codes

*   200 - OK: Everything went as planned.
*   304 - Not Modified: Resource hasn't been updated since the date provided. See Caching below.
*   400 - Bad Request: You did something wrong. Often a missing argument or parameter.
*   401 - Unauthorized: Authentication was incorrect.
*   404 - Not Found
*   500 - Internal Server Error: We had a problem processing the request.
*   503 - Service Unavailable: Try again later.

### Errors

Error responses include details about what went wrong.

```json
{
    "kind": "error",
    "code": "invalid_params",
    "message": "The parameters of your request were invalid.",
    "params": {
        "dropoff_name": "Dropoff name is required.",
        "dropoff_phone_number": "Dropoff phone number must be valid phone number."
    }
}
```

### Error Codes

This list will likely become more comprehensive as features are implemented.

*   `invalid_params` - The indicated parameters were missing or invalid.
*   `unknown_location` - We weren't able to understand the provided address. This usually indicates the address is wrong, or perhaps not exact enough.
*   `request_rate_limit_exceeded` - This API key has made too many requests.
*   `account_suspended`
*   `not_found`
*   `service_unavailable`
*   `delivery_limit_exceeded` - You have hit the maximum amount of ongoing deliveries allowed.
*   `customer_limited` - Your account's limits have been exceeded.
*   `couriers_busy` - All of our couriers are currently busy.

### Types

Responses often indicate a type of object being described. Though this can often be inferred from the resource being requested, but we like to be explicit:

```json
{
    "kind": "user",
    "name": "Alice"
}
```

### Currency

When a response involves a currency, it is described as follows:

```json
{
    "amount": 799,
    "currency": "usd"
}
```

This indicates a value of $7.99 in USD.

### Times

Times are formatted according to ISO8601 and reported only in UTC time. For example:

    {
      created: "2014-08-26T10:04:03Z",
    }

Objects will often have attributes like "created" and "updated".

### Large Result Sets

Some resources may have too many objects to include in a single response. To support pagination, we include attributes such as:

    {
      total_count: 100,
      next_href: "https://api.postmates.com/v1/...",
      data: [...]
    }

By requesting the `next_href`, the client will receive the next set of objects.

Clients can also request a specific number of response objects by using the `limit` query parameter.

### Locations

Some objects have a geographic location associated with them. This typically of the form:

    {
      "location" : {
        "lat" : 37.42291810,
        "lng" : -122.08542120
      }
    }
