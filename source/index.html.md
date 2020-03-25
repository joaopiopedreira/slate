---
title: API Reference
  
toc_footers:
  - <a href='https://go.letsjobit.com/#!join' target='_blank'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

This reference helps you implement the RESTful LetsJobIt API v2. This API uses a JSON format for output and is capable of handling CORS (Cross-Origin Resource Sharing) requests. The API is stateless – all requests are validated against an API token. The API token can be obtained manually from the LetsJobIt app.

<aside class="warning">
Please be aware that any actions you perform through this reference testing functionality will involve operations with your production data. To test your applications against some sandbox data, we recommend creating a new, separate LetsJobIt account for sandboxing purposes.

This API is under active development and we are constantly adding new functionality.

All requests to the API must be made over SSL (https not http).
</aside>

# URL Naming
The API uses a straight-forward URL naming convention. Each request must be made to the API endpoint (https://app.letsjobit.com/api/v2/), followed by the type of object in a plural form, e.g. https://app.letsjobit.com/api/v2/visits.

When one resource is being requested, you have the option to append the ID of the item to the URL, e.g. https://app.letsjobit.com/v2/visits/yre78r7wer. If you don't append an ID of the item, then all the items for that resource will be returned (paginated, 100 at the time). The API_KEY must be provided as part of the query string for all requests.

# Response Format
Each response sent from the API contains a 'meta' object indicating whether the request was carried out or not. If the request was not successful, an optional 'error' parameter (string) may be given. If the request was successful, the response is always contained within a 'data' parameter, and additional metadata is contained inside a 'meta' parameter.

# Pagination and Lists
Most of the lists/item collections are paginated. The parameter that controls the pagination is 'pagination', indicating starting page of the next set of records. Within the response, a 'meta' object will be set upon pagination. The 'remainingRecords' parameter will contain remaining records and the parameter 'nextStart' will tell you where to start the next request. If 'nextStart' = 100, then the query parameter 'pagination' should be 100 in the next request you make for the same URI.

# How do I...
"How can I fetch all visits, leads, items, etc?" You can fetch all items of any kind from the LetsJobIt API. In order to fetch all visits (or any other resource), you can use the pagination data of our response objects to issue multiple requests in a loop to fetch all visits. You would have to check the 'remainingRecords' parameter in the response 'meta' object, and issue an additional request — only increasing the 'pagination' property by 100 (the hard limit we impose on our API). For example, if your first request ran against &pagination=0, you would have to make the next request with &pagination=100 to get the next 100 items, and continue making requests until the 'remainingRecords' parameter is 0 (zero) in the response.

# Request format
You must use JSON body format when performing API requests. In order to do a proper JSON-formatted request, make sure you provide the header "Content-Type": "application/json" as part of your HTTP request.

# Rate limiting
Rate limiting is is considered per API KEY. Our API allows you to perform 100 requests per 10 seconds. Every API response includes the following headers:

- X-RateLimit-Remaining: the amount of requests left for the 10 seconds window.
- X-Rate-Limit-Reset: the amount of seconds before the limit resets.

In case the limit is exceeded for the time window, the LetsJobIt API will return an error response with HTTP code 429 and the headers above.

# Visits

## Get All Visits

```shell
curl "https://app.letsjobit.com/api/v2/visits"
  -H "Authorization: Bearer MY_API_KEY"
  -H 'Content-Type: application/json'
```
> Make sure to replace MY_API_KEY with your API key

> The above command returns JSON structured like this:

```json
{
  "meta": {
    "totalRecords": 2000
    "returnedRecords": 100,
    "remainingRecords": 1900,
    "nextStart": 100
  },
  "data": [
    {
        "_id" : "yKQhJZ9B7PTT85kWc",
        "driverId" : "FEn9BJz93svSNPx7p",
        "name" : "Liam",
        "date" : "2020-03-31",
        "totalJobs" : 1,
        "id" : 10816703,
        "docType" : "CALL OUT",
        "docNo" : "H-017623",
        "serviceOrder" : 3,
        "customer" : "The Green Sleeve",
        "siteId" : "30101308",
        "lat" : 57.000000,
        "lng" : -2.000000,
        "site" : "The Green Sleeve",
        "address" : "999 West High Street",
        "postCode" : "AB51 3QR",
        "city" : "INVERURIE",
        "obs" : "Contact Info: The Green Sleeve, John: 01999999999 \r\n AQUARIUS TONE7/3 2001 - Side of the shop\r\nAccess 9am - 5.30pm\r\nNo parking onsite ",
        "contactPerson" : "John",
        "contactPhone" : "01999999999",
        "emailMandatory" : true,
        "priority" : "3",
        "showItemsOnOrderSection" : true,
        "hideEquipmentDetails" : false,
        "hideDeliveryItemsSection" : false,
        "signeeName" : "John",
        "signeeEmail" : "john@email.com",
        "subContractorIds" : [ 
            "My Subcontractor"
        ],
        "territory" : "AB-Aberdeen",
        "useStartPauseStopStyleButtonsInJobs" : true,
        "inMobileApp" : true,
        "NPSMandatory" : true,
        "precedencePriorityVisit" : false,
        "closeVisitOnLastJobClose" : false,
        "country" : "GB",
        "companyId" : "LPeaJNaLQT6RqcFdt",
        "time_windows" : [ 
            {
                "earliest" : 30600,
                "latest" : 64800
            }
        ],
        "required_skills" : [ 
            "Plumber",
            "Hot water technician",
            "Installer"
        ],
        "allowed_vehicles" : [],
        "confirmClose" : true,
        "inProgress" : false,
        "closed" : true,
        "skipped" : false,
        "geocoding" : null,
        "showDiscountAndBonusOnItemsOnOrder" : false,
        "duration" : 2100,
        "createdAt" : "2020-03-09 15:48:17",
        "createdUser" : "API",
        "hasChangesToSync" : false,
        "modifieddAt" : "2020-03-24 15:45:38",
        "modifyUser" : "2pjP3L3S8JtZhLPoN",
        "servicing" : false,
        "signeeRating" : "5",
        "signature" : "/9j/4AAQSkZJRgABAQEAAAAAA...",
        "appClientVersion" : "1.7.163",
        "closeDate" : "2020-03-24 15:45",
        "skippedReasonId" : ""
    }
  ]
}
```

This endpoint retrieves all visits. Visits are the main resource in our API, since they link all the other resources together. Within LetsJobIt, "visits" or "sites" refer to the same thing: it's a customer (address) you must visit to perform a service (repair, delivery, etc.).

### HTTP Request

`GET https://app.letsjobit.com/api/v2/visits`

### Query Parameters

Use the query parameters below to refine your request.

Parameter | Format |  Description | Required
--------- | ------- | ----------- | --------- 
**pagination** | string | start number (skip) for the query | optional
**_id** | string | Visit internal id | optional
**id** | string | Your ERP id | optional
**date** | string | visit date in "YYYY-MM-DD" format | optional
**driverId** | string | the id of the driver assigned to the visit | optional
**docNo** | string | user document number ("contains" search) | optional
**closed** | number | Visit status: 0 - opened, 1 - closed | optional
**confirmClose** | number | Determines if the visit was confirmed as closed by your ERP system. LetsJobIt is an offline first app and we like to let our users know if/when their info is synchronised. | optional
**inProgress** | number | Determines if the visit is in progress. The visit is in progress from the moment the user starts servicing a job until the moment the visit is closed. | optional
**priority** | number | Visit priority. 1 - high priority, 2 - normal priority, 3 - low priority | optional
**skipped** | number | If the visit was skipped (postponed): 0 - not skipped, 1 - skipped | optional

<aside class="notice">
Remember — your resultas are paginated and you have to manage that on your side!
</aside>

A visits response contains an object with the following properties:

Parameter | Format |  Description
--------- | ------ | ------------ 
**meta** | Object | metadata about this request
**data** | Object | the data returned

The **meta** object has the following properties:

Parameter | Format |  Description
--------- | ------ | ------------ 
**totalRecords** | number | The total records available
**returnedRecords** | number | Returned records count
**remainingRecords** | number | Remaining records count
**nextStart** | number | Start index of next record

The **data** object has the following properties:

Parameter | Format |  Description
--------- | ------ | ------------ 
**_id** | string | the internal id of the visit
**driverId** | string | The driver id for this visit
**name** | string | The driver name for this visit
**date** | string | The expected date for this service
**totalJobs** | integer | The total jobs (total service items - actionItem type items) attached to this visit
**id** | integer | Your ERP id for this visit
**docNo** | string | the document number associated with the visit
**serviceOrder** | integer | The sequential order in which the service must be done (relative to the other visits). If you use LetsJobIt routing, routes will be sequenced and this value will be overwritten.
**customer** | string | The customer name
**siteId** | integer | Your ERP customer address id. Useful for integrations.
**lat** | number | the address latitude
**lng** | number | the address longitude
**site** | string | the site (address) description. This can be an add-on to the address field.
**address** | string
**postCode** | string
**city** | string
**obs** | string | the visit observations.
**contactPerson** | string | the name of the contact person at the customer
**contactPhone** | string | the phone number of the contact person at the customer
**emailMandatory** | integer | Determines if the email field in the client signature screen is mandatory. 1 for true, 0 for false.
**showItemsOnOrderSection** | number | Determines if the items on order section in the job view is visible. 1 to show, 0 to hide.
**hideEquipmentDetails** | number | Determines if the equipment details section on the job view (top) is visible. 0 to show, 1 to hide.
**signeeName** | string | The name of the customer contact that signed on visit close.
**signeeEmail** | string | The email of the customer contact that signed on visit close.
**companyId** | string | your company ID as recorded at LetsJobIt
**subContractorIds** | string array | The sub-contractor name(s) related with this visit. If more than one, they'll be supplied in a comma separated list. The leftmost name is the sub-contractor to which the visit is intended (or the one that completed the visit). If there is no sub-contractor, this field is returned as null.
**confirmClose** | number | Determines if the visit was confirmed as closed by your ERP system. LetsJobIt is an offline first app and we like to let our users know if/when their info is synchronised.
**inProgress** | number | Determines if the visit is in progress. The visit is in progress from the moment the user starts servicing a job until the moment the visit is closed.
**closed** | number | If the visit is closed. 1 - closed, 0 - opened.
**skipped** | number | if the visit was skipped. A skipped visit is a visit that could not be accomplished for any reason (see unsuccessful reasons below - examples: client was closed, wrong address, etc.).
**hideDeliveryItemsSection** | number | If the delivery items sections is shown in the job view. 1 - hide, 0 - show.
**showDiscountAndBonusOnItemsOnOrder** | number | If 1 (true), a special section is shown below each item to order. The 2 fields shown are "discount" and "Bonus".
**priority** | number | the service/delivery priority, from 1 to 10: 1 - highest priority, 10 - lowest priority.
**useStartPauseStopStyleButtonsInJobs** | number | If the job shows start/pause/stop buttons as opposed to showing only a start/finish button. This option allows you to pause one job (lunch, have to start another job and come back to this one...) and resume it afterwards.
**modifieddAt** | string | the date/time the record was modified.
**modifyUser** | string | the user id that last modified the record
**duration** | number | The total duration in seconds of all the service jobs in the visit. This duration is only theoretical and corresponds to the durations you've eventually sent for every service job in the visit, for routing purposes.
**signeeRating** | number | The 1 to 5 NPS score from your customer. If the value is 0 (zero), then the customer didn't enter a score.
**closeDate** | string | The date-time the document was closed.
**skippedReasonId** | string or number | The skip reason id (if the visit was skipped). Skip reasons are documented under the "Static Tables" model and end points.
**lines** | array of objects | the line items included in the visit

The **lines** array is composed of objects with the following properties:

Parameter | Format |  Description
--------- | ------ | ------------ 
**actionItem** | boolean | Determines if the line item is a "job type" item. Please refer to our user documentation.
**allowRebrand** | boolean | Determines if the user is allowed to change the equipment brand in the job view. Please refer to the our user documentation.
**brand** | string | The equipment brand
**brandId** | integer | The equipment brand id from your backend system.
**description** | string | The description of the item (what shows in the interface).
**extraData** | array of objects | An array of objects containing custom field definitions. These field show in the job view, after the stardard fields.
**hasBatch** | boolean | Determines if the line-item
**hasReturn** | boolean | Determines if the line item has returned items (like empty pallets or empty packages)
**id** | number | Your ERP line id. Can be useful to cross check your responses and make the integration process easier.
**itemId** | number | Your ERP item id.
**jobType** | string | Arbitrary type to classify this job. Could be "delivery", "installation", "repair", etc
**location** | string | The location of the service, within your customer premisses. "bar 2nd floor", "kitchen", etc.
**model** | string | The model name (related with the equipment brand)
**modelId** | number | The model Id from your ERP
**qty** | number | The quantity for this item. Defaults to 1.
**serialNr** | string | The equipment serial number for this job.
**serialNrId** | number | Your ERP serialNr Id. Useful for integration purposes.

The **extraData** array is composed of objects with the following properties:

Parameter | Format |  Description
--------- | ------ | ------------ 
**caption** | string | The field name to show to the user.
**description** | string | An optional description to show above the caption, intructing the user on how to fill in that field.
**max** | integer | The max value for the field (only applies to numeric fields)
**min** | integer | The min value for the field (only applies to numeric fields)
**name** | string | The field name as saved to the database.
**pattern** | string | A regexp expression to validate the field against.
**step** | number | The html5 step value for the field (only applies to numeric fields)
**type** | string | The field html5 type.


## Get a specific visit

```shell
curl "https://app.letsjobit.com/api/v2/visits/1"
  -H "Authorization: Bearer MY_API_KEY"
  -H 'Content-Type: application/json'
```
> Make sure to replace MY_API_KEY by your API_KEY

> The above command returns a JSON representation of the visit with ID = 1 and with the same structure as the "get all visits" endpoint..

This endpoint retrieves a specific visit by ID.

### HTTP Request

`GET https://app.letsjobit.com/api/v2/visits/{ID}`

### URL Parameters

Parameter | Format | Description
--------- | ------ | -----------
ID | string | The ID of the visit to retrieve

## Add visits

```shell
curl "https://app.letsjobit.com/api/v2/visits/1"
  -X POST
  -H "Authorization: Bearer MY_API_KEY"
  -H 'Content-Type: application/json'
  --data $'{...}' # a visit object array
```
> Make sure to replace MY_API_KEY by your API_KEY

> The data object in the above command should be a "visits" array with a 
> JSON structure similar to the below:

```json
[
    {
        "driverId" : "FEn9BJz93svSNPx7p",
        "name" : "Liam",
        "date" : "2020-03-31",
        "id" : 10816703,
        "docType" : "CALL OUT",
        "docNo" : "H-017623",
        "serviceOrder" : 3,
        "customer" : "The Green Sleeve",
        "siteId" : "30101308",
        "lat" : 57.000000,
        "lng" : -2.000000,
        "site" : "The Green Sleeve",
        "address" : "999 West High Street",
        "postCode" : "AB51 3QR",
        "city" : "INVERURIE",
        "obs" : "Contact Info: The Green Sleeve, John: 01999999999 \r\n AQUARIUS TONE7/3 2001 - Side of the shop\r\nAccess 9am - 5.30pm\r\nNo parking onsite ",
        "contactPerson" : "John",
        "contactPhone" : "01999999999",
        "emailMandatory" : "1",
        "priority" : "3",
        "showItemsOnOrderSection" : "1",
        "hideEquipmentDetails" : "0",
        "hideDeliveryItemsSection" : "0",
        "signeeName" : "John",
        "signeeEmail" : "john@email.com",
        "subContractorIds" : [ 
            "My Subcontractor"
        ],
        "territory" : "AB-Aberdeen",
        "useStartPauseStopStyleButtonsInJobs" : "1",
        "inMobileApp" : "1",
        "NPSMandatory" : "1",
        "precedencePriorityVisit" : "0",
        "closeVisitOnLastJobClose" : "0",
        "country" : "GB",
        "companyId" : "LPeaJNaLQT6RqcFdt",
        "time_windows" : [ 
            {
                "earliest" : 30600,
                "latest" : 64800
            }
        ],
        "required_skills" : [ 
            "Plumber",
            "Hot water technician",
            "Installer"
        ],
        "allowed_vehicles" : [],
        "confirmClose" : "1",
        "inProgress" : "0",
        "closed" : "1",
        "skipped" : "0",
        "duration" : 2100
    }
  ]
```
  
> The request returns JSON similar to the below

```json
{
  "meta": {
    "recordsUpdated": 5,
    "recordsInserted": 3,
    "errors": [
        {
          "error": 404,
          "message": "error description"
        } 
    ]
  },
  "data" : null
}
```

This endpoint inserts/updates a new/existing visits.

### HTTP Request

`POST https://app.letsjobit.com/api/v2/visits`

### URL body

Pass an array of visit objects, each with the following properties:

Parameter | Format | Required | Description
--------- | ------ | -------- | -----------
**driverId** | string | yes | The driver id for this visit
**name** | string | no | The driver name for this visit
**date** | string | yes | The expected date for this service
**id** | number | yes | Your ERP id (or unique reference) for this visit. This field is mandatory because we need a unique reference to the visit to check if it already exists.
**docType** | string | no | the document type associated with the visit
**docNo** | string | yes | the document number associated with the visit
**serviceOrder** | number | no | The sequential order in which the service must be done (relative to the other visits). If you use LetsJobIt routing, routes will be sequenced and this value will be overwritten.
**customer** | string | yes | The customer name
**siteId** | number | no | Your ERP customer address id. Useful for integrations.
**lat** | number | no | the address latitude. If absent, the address will be geocoded.
**lng** | number | no | the address longitude. If absent, the address will be geocoded.
**site** | string | no | the site (address) description. This can be an add-on to the address field.
**address** | string | yes |
**postCode** | string | no |
**city** | string | yes 
**obs** | string | no | the visit observations.
**contactPerson** | string | no | the name of the contact person at the customer
**contactPhone** | string | no | the phone number of the contact person at the customer
**emailMandatory** | number | no | Determines if the email field in the client signature screen is mandatory. 1 for true, 0 for false.
**priority** | number | no | the service/delivery priority, from 1 to 10: 1 - highest priority, 10 - lowest priority.
**showItemsOnOrderSection** | number | no | Determines if the items on order section in the job view is visible. 1 to show, 0 to hide.
**hideEquipmentDetails** | number | no | Determines if the equipment details section on the job view (top) is visible. 0 to show, 1 to hide.
**hideDeliveryItemsSection** | number | no | If the delivery items sections is shown in the job view. 1 - hide, 0 - show.
**signeeName** | string | no | The name of the customer contact that signed on visit close.
**signeeEmail** | string | no | The email of the customer contact that signed on visit close.
**subContractorIds** | string array | no | The sub-contractor name(s) related with this visit. If more than one, they'll be supplied in a comma separated list. The leftmost name is the sub-contractor to which the visit is intended (or the one that completed the visit). If there is no sub-contractor, this field is returned as null.
**territory** | string | no | The geographic territory associated to the visit.
**useStartPauseStopStyleButtonsInJobs** | number | no | If the job shows start/pause/stop buttons as opposed to showing only a start/finish button. This option allows you to pause one job (lunch, have to start another job and come back to this one...) and resume it afterwards.
**inMobileApp** | number | no | Pass "1" if the visit is supposed to be assigned to an engineer/driver and, thus, available in her mobile app or "0" if it's not. Please refer to our [online docs](http://support.letsjobit.routesdirect.com/support/solutions).
**NPSMandatory** | number | no | Pass "1" to make the NPS mandatory or "0" to make it optional. Please refer to our [online docs](http://support.letsjobit.routesdirect.com/support/solutions).
**precedencePriorityVisit** | number | no | Pass "1" to make this visit a "priority visit" or "0" to make it a regular visit. Please refer to our [online docs](http://support.letsjobit.routesdirect.com/support/solutions).
**closeVisitOnLastJobClose** | number | no | Pass "1" to close the visit when you close the last job or "0" to force the engineer/driver to close the visit in the usual way. Please refer to our [online docs](http://support.letsjobit.routesdirect.com/support/solutions).
**country** | string | no | The 2 letters of the [country code top-level domain](https://en.wikipedia.org/wiki/Country_code_top-level_domain).
**companyId** | string | yes | your company ID as recorded at LetsJobIt
**time_windows** | array of objects | no | The opening/closing times of your customer. You can have several time slots for the same customer (for instance, a customer may be opened from 8am to 1pm and from 3pm to 5pm). Please see below.
**required_skills** | array of strings | no | The skills required for this visit.
**allowed_vehicles** | array of strings | no | The vehicles allowed for this visit (you may want to restrict the visit for only a few engineers/drivers).
**duration** | number | no | The total duration in seconds of all the service jobs in the visit. This duration is only theoretical and corresponds to the durations you've eventually sent for every service job in the visit, for routing purposes.
**lines** | array of objects | no | the line items included in the visit

The **time_windows** array is composed of objects with the following properties:

Parameter | Format |  Description
--------- | ------ | ------------ 
**earliest** | number | The opening time (in seconds from midnight).
**latest** | number | The closing time (in seconds from midnight).


The **lines** array is composed of objects with the following properties:

Parameter | Format | Required | Description
--------- | ------ | -------- | -----------
**actionItem** | boolean | yes | Determines if the line item is a "job type" item. Please refer to our user documentation.
**allowRebrand** | boolean | no | Determines if the user is allowed to change the equipment brand in the job view. Please refer to the our user documentation.
**brand** | string | no | The equipment brand
**brandId** | integer | no | The equipment brand id from your backend system.
**description** | string | yes | The description of the item (what shows in the interface).
**extraData** | array of objects | no | An array of objects containing custom field definitions. These field show in the job view, after the stardard fields.
**hasBatch** | boolean | no | Determines if the line-item
**hasReturn** | boolean | no | Determines if the line item has returned items (like empty pallets or empty packages)
**id** | number | no | Your ERP line id. Can be useful to cross check your responses and make the integration process easier.
**itemId** | number | no | Your ERP item id.
**jobType** | string | no | Arbitrary type to classify this job. Could be "delivery", "installation", "repair", etc
**location** | string | no | The location of the service, within your customer premisses. "bar 2nd floor", "kitchen", etc.
**model** | string | no | The model name (related with the equipment brand)
**modelId** | number | no | The model Id from your ERP
**qty** | number | no | The quantity for this item. Defaults to 1.
**serialNr** | string | no | The equipment serial number for this job.
**serialNrId** | number | no | Your ERP serialNr Id. Useful for integration purposes.

The **extraData** array is composed of objects with the following below. Custome fields defined here will show in the service line items. 

Parameter | Format | Required | Description
--------- | ------ | -------- | -----------
**type** | string | yes | The field html5 type. One of `text`,`date`,`number`,`email`,`color`,`range` or `select`. See [here](http://www.w3schools.com/html/html_form_input_types.asp).
**caption** | string | yes | The field name to show to the user.
**description** | string | no | An optional description to show above the caption, intructing the user on how to fill in that field.
**value** | string or number | no | The default value you want this field to have.
**required** | string | no | Pass "required" if you want this field to be required in the app.
**readonly** | string | no | Pass "readonly" if you want this field to be readonly in the app.
**options_values** | string | no | Only used if the **type** is `select`. The option values for the field, separated by pipes. Example: <code>"option1 &#124; option2"</code>.
**options_default** | string | no | Only used if the **type** is `select`. The default value for the select options.
**max** | integer | no | The max value for the field (only applies to numeric fields)
**min** | integer | no | The min value for the field (only applies to numeric fields)
**name** | string | yes | The field name as saved to the database.
**pattern** | string | no | A regexp expression to validate the field against. Example: `[0-9]+([\.,][0-9]+)?`
**step** | number | no | The html5 step value for the field (only applies to numeric fields)
 

## Update a specific visit

```shell
curl "https://app.letsjobit.com/api/v2/visits/1"
  -X PUT
  -H "Authorization: Bearer MY_API_KEY"
  -H 'Content-Type: application/json'
  --data $'{"date": "2018-12-20",
                "driverId": "12345",
                "obs": "No parking available"}'
```
> Make sure to replace MY_API_KEY by your API_KEY

> The above command updates the date, driverId and obs fields in the 
> visit with ID = 1. The returns JSON will be similar to the below:

```json
{
  "meta": {
    "updatedRecords": 1
  },
  "data" : null
}
```

This endpoint updates a specific visit.

### HTTP Request

`PUT https://app.letsjobit.com/api/v2/visits/{ID}`

### URL Parameters

Parameter | Format | Description
--------- | ------ | -----------
ID | string | The ID of the visit to delete


## Delete a specific visit

```shell
curl "https://app.letsjobit.com/api/v2/visits/1"
  -X DELETE
  -H "Authorization: Bearer MY_API_KEY"
```
> Make sure to replace MY_API_KEY by your API_KEY

> The above command deletes the visit with ID = 1 and returns JSON similar to the below:

```json
{
  "meta": {
    "deletedRecords": 1
  },
  "data" : null
}
```

This endpoint deletes a specific visit.

### HTTP Request

`DELETE https://app.letsjobit.com/api/v2/visits/{ID}`

### URL Parameters

Parameter | Format | Description
--------- | ------ | -----------
ID | string | The ID of the visit to delete

