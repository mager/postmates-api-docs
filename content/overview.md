## Overview

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
