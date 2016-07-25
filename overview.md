# API Introduction

The 10Kft API provides programmatic access to projects, users and time entries in your account, commonly referred to as resources in the rest of this document. The API implements a standard HTTP REST pattern and allows callers with appropriate authentication and authorization to programmatically create, read, update and delete these resources.

**Register your integration and sign up for important Developer API Email Announcements [here](http://eepurl.com/ZvuOb).**

## Test vs. Production Environments

There is one test environment for developers to experiment with custom integrations before activating it the live account.

We strongly recommend you test before moving your integration to production. This will reduce the chances of accidentally compromising the data in your live account.

The <span style="background-color: initial;">steps for setting up your developer account, as well as the</span><span style="background-color: initial;"></span> <span style="background-color: initial;">base URLs for the test environment, are located on the</span> [Custom Integrations](/plans/reference/integration/custom-integrations) <span style="background-color: initial;">page.</span>

## Data Collections

The API provides access to the following data collections available in your account.

*   Users
*   Projects
*   Phases
*   Time Entries
*   Assignments
*   Tags
*   Project Budget Items
*   User Statuses

## Collections and Objects

The API, in typical RESTful fashion, lets you access collections of objects. These Collections are paginated, and are always delivered in a structure that has a 'data' array and 'paging' meta-data, as shown below. This is true top level as well as nested collections.

Some objects returned by the API may have sub-collections. For example, a tags collection for a given user object. These sub-collections are paginated in the same manner as top level collections.

## Authentication

The API expects an API authentication token (api-token) to be passed in with every API request. This can be sent as a query parameter named `auth`, or as an HTTP header with the same name.

Account administrators can find the API key under _Settings >_ _Developer API_. Note that you will get separate API tokens for development/test purposes vs final integration with your account on 10000ft.com.

## Pagination

The pagination section in collections provide mechanisms to fetch additional data. The `previous` and `next` links provide access to the corresponding pages in the collection.

You can override the default `per_page` value by providing an appropriate value in the URL query parameter. For example,

<pre>GET /api/v1/users?per_page=100
</pre>

## Date Formatting

The API handles or expects dates and datetime values in various scenarios, and expects the values to be in specific formats.

Date values provided as query parameters when making API calls must be in the `YYYY-MM-DD` format or `DD/MM/YYYY` format.

## Error Handling

Respond with standard HTTP error messages, with 4XX codes to indicate client (caller) errors. There may be an optional response body with a message that has details for debugging / error logging. This message is not meant to end user display or consumption.

<pre>HTTP/1.1 400 Bad Request
{ "message" : "The request body had invalid json" }
</pre>

# Reporting Problems

When contacting support for assistance with using the API, please provide example API calls where possible. This greatly speeds up the time to resolve support requests. You can use cURL, available on unix platforms as well as windows, to create an example of your scenario, and including the curl command(s) along with your request for support.

Here is an example curl command for invoking the users API via curl.

<pre># get the users collection
curl -H "Content-Type: application/json" -X GET https://vnext-test.10000ft.com/api/v1/users?auth=TOKEN
</pre>

# General API Use Examples

## Fetching a Collection

<pre>GET /api/v1/users
response:
{
  "data" : [
    { "id" : 1, "first_name" : "claes", ... },
     ...
  ],
  "paging": {
    "next": null,
    "page": 1,
    "per_page": 20,
    "previous": null,
    "self": "/api/v1/users/1/statuses?user_id=1&per_page=20&page=1"
  }
}
</pre>

## Fetching an Object

<pre>GET /api/v1/users/5
response:
{
  "id" : 5,
  "first_name" : "claes",
  ...
},
</pre>

## Fetching an Object With Optional Fields

<pre>GET /api/v1/users/5?fields=tags
response:
{
  "id" : 5,
  "first_name" : "claes",
  ...,
  "tags" : {
    "data" : [ "developer", "seattle", ... ],
    "paging" : { ... }
  }
}
</pre>

## Updating an Object

<pre>PUT /api/v1/users/5
{
  "id" : 5,
  "first_name" : "Claes"
}
response:
{
  "id" : 5,
  "first_name" : "Claes",
  ...
}
</pre>

## Creating an Object

<pre>POST /api/v1/users
{
  "first_name" : "Nick"
  "first_name" : "Pants"
}
response:
{
  "id" : 6,
  "first_name" : "Nick",
  ...
}
</pre>

## Deleting an Object

<pre>DELETE /api/v1/users/5
</pre>