# Cancer Research REST API Guidelines
## 1 Abstract
The Cancer Research REST API Guidelines, as a design principle, encourages application developers to have resources accessible to them via a RESTful HTTP interface. To provide the smoothest possible experience for developers on platforms following the Cancer Research REST API Guidelines, REST APIs SHOULD follow consistent design guidelines to make using them easy and intuitive.  

This document establishes the guidelines Cancer Research REST APIs SHOULD follow so RESTful interfaces are developed consistently.

## 2 Table of contents
<!-- TOC depthFrom:1 depthTo:3 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Cancer Research REST API Guidelines 2.3](#Cancer Research-rest-api-guidelines-23)
	- [Cancer Research REST API Guidelines Working Group](#Cancer Research-rest-api-guidelines-working-group)
- [Cancer Research REST API Guidelines](#Cancer Research-rest-api-guidelines)
	- [1 Abstract](#1-abstract)
	- [2 Table of contents](#2-table-of-contents)
	- [3 Introduction](#3-introduction)
		- [3.1 Recommended reading](#31-recommended-reading)
	- [4    Interpreting the guidelines](#4-interpreting-the-guidelines)
		- [4.1    Application of the guidelines](#41-application-of-the-guidelines)
		- [4.2    Guidelines for existing services and versioning of services](#42-guidelines-for-existing-services-and-versioning-of-services)
		- [4.3    Requirements language](#43-requirements-language)
		- [4.4    License](#44-license)
	- [5 Taxonomy](#5-taxonomy)
		- [5.1    Errors](#51-errors)
		- [5.2    Faults](#52-faults)
		- [5.3    Latency](#53-latency)
		- [5.4    Time to complete](#54-time-to-complete)
		- [5.5    Long running API faults](#55-long-running-api-faults)
	- [6    Client guidance](#6-client-guidance)
		- [6.1    Ignore rule](#61-ignore-rule)
		- [6.2    Variable order rule](#62-variable-order-rule)
		- [6.3    Silent fail rule](#63-silent-fail-rule)
	- [7    Consistency fundamentals](#7-consistency-fundamentals)
		- [7.1    URL structure](#71-url-structure)
		- [7.2    URL length](#72-url-length)
		- [7.3    Canonical identifier](#73-canonical-identifier)
		- [7.4    Supported methods](#74-supported-methods)
		- [7.5    Standard request headers](#75-standard-request-headers)
		- [7.6    Standard response headers](#76-standard-response-headers)
		- [7.7    Custom headers](#77-custom-headers)
		- [7.8    Specifying headers as query parameters](#78-specifying-headers-as-query-parameters)
		- [7.9    PII parameters](#79-pii-parameters)
		- [7.10   Response formats](#710-response-formats)
		- [7.11   HTTP Status Codes](#711-http-status-codes)
		- [7.12   Client library optional](#712-client-library-optional)
	- [8    CORS](#8-cors)
		- [8.1    Client guidance](#81-client-guidance)
		- [8.2    Service guidance](#82-service-guidance)
	- [9    Collections](#9-collections)
		- [9.1    Item keys](#91-item-keys)
		- [9.2    Serialization](#92-serialization)
		- [9.3    Collection URL patterns](#93-collection-url-patterns)
		- [9.4    Big collections](#94-big-collections)
		- [9.5    Changing collections](#95-changing-collections)
		- [9.6    Sorting collections](#96-sorting-collections)
		- [9.7    Filtering](#97-filtering)
		- [9.8    Pagination](#98-pagination)
		- [9.9    Compound collection operations](#99-compound-collection-operations)
	- [10    Delta queries](#10-delta-queries)
		- [10.1    Delta links](#101-delta-links)
		- [10.2    Entity representation](#102-entity-representation)
		- [10.3    Obtaining a delta link](#103-obtaining-a-delta-link)
		- [10.4    Contents of a delta link response](#104-contents-of-a-delta-link-response)
		- [10.5    Using a delta link](#105-using-a-delta-link)
	- [11    JSON standardizations](#11-json-standardizations)
		- [11.1    JSON formatting standardization for primitive types](#111-json-formatting-standardization-for-primitive-types)
		- [11.2    Guidelines for dates and times](#112-guidelines-for-dates-and-times)
		- [11.3    JSON serialization of dates and times](#113-json-serialization-of-dates-and-times)
		- [11.4    Durations](#114-durations)
		- [11.5    Intervals](#115-intervals)
		- [11.6    Repeating intervals](#116-repeating-intervals)
	- [12    Versioning](#12-versioning)
		- [12.1    Versioning formats](#121-versioning-formats)
		- [12.2    When to version](#122-when-to-version)
		- [12.3    Definition of a breaking change](#123-definition-of-a-breaking-change)
	- [13    Long running operations](#13-long-running-operations)
		- [13.1    Resource based long running operations (RELO)](#131-resource-based-long-running-operations-relo)
		- [13.2    Stepwise long running operations](#132-stepwise-long-running-operations)
		- [13.3    Retention policy for operation results](#133-retention-policy-for-operation-results)
	- [14    Push notifications via webhooks](#14-push-notifications-via-webhooks)
		- [14.1    Scope](#141-scope)
		- [14.2    Principles](#142-principles)
		- [14.3    Types of subscriptions](#143-types-of-subscriptions)
		- [14.4    Call sequences](#144-call-sequences)
		- [14.5    Verifying subscriptions](#145-verifying-subscriptions)
		- [14.6    Receiving notifications](#146-receiving-notifications)
		- [14.7    Managing subscriptions programmatically](#147-managing-subscriptions-programmatically)
		- [14.8    Security](#148-security)
	- [15    Unsupported requests](#15-unsupported-requests)
		- [15.1    Essential guidance](#151-essential-guidance)
		- [15.2    Feature allow list](#152-feature-allow-list)
	- [16     Appendix](#16-appendix)
		- [16.1    Sequence diagram notes](#161-sequence-diagram-notes)
	

<!-- /TOC -->

## 3 Introduction
Developers access most Cancer Research Web Services resources via HTTP interfaces. Although each service typically provides language-specific frameworks to wrap their APIs, all of their operations eventually boil down to HTTP requests. Cancer Research must support a wide range of clients and services and cannot rely on rich frameworks being available for every development environment. Thus a goal of these guidelines is to ensure Cancer Research REST APIs can be easily and consistently consumed by any client with basic HTTP support.

To provide the smoothest possible experience for developers, it's important to have these APIs follow consistent design guidelines, thus making using them easy and intuitive. This document establishes the guidelines to be followed by Cancer Research REST API developers for developing such APIs consistently.

The benefits of consistency accrue in aggregate as well; consistency allows teams to leverage common code, patterns, documentation and design decisions.

These guidelines aim to achieve the following:
- Define consistent practices and patterns for all API endpoints across Cancer Research.
- Adhere as closely as possible to accepted REST/HTTP best practices in the industry at-large.*
- Make accessing Cancer Research Services via REST interfaces easy for all application developers.
- Allow service developers to leverage the prior work of other services to implement, test and document REST endpoints defined consistently.

*Note: The guidelines are designed to align with building services which comply with the REST architectural style, though they do not address or require building services that follow the REST constraints. The term "REST" is used throughout this document to mean services that are in the spirit of REST rather than adhering to REST by the book.*

### 3.1 Recommended reading
Understanding the philosophy behind the REST Architectural Style is recommended for developing good HTTP-based services. If you are new to RESTful design, here are some good resources:

[REST on Wikipedia][rest-on-wikipedia] -- Overview of common definitions and core ideas behind REST.

[REST Dissertation][fielding] -- The chapter on REST in Roy Fielding's dissertation on Network Architecture, "Architectural Styles and the Design of Network-based Software Architectures"

[RFC 7231][rfc-7231] -- Defines the specification for HTTP/1.1 semantics, and is considered the authoritative resource.

[REST in Practice][rest-in-practice] -- Book on the fundamentals of REST.

## 4 Interpreting the guidelines
### 4.1 Application of the guidelines
These guidelines are applicable to any REST API exposed publicly by Cancer Research or any partner service. Private or internal APIs SHOULD also try to follow these guidelines because internal services tend to eventually be exposed publicly.  Consistency is valuable to not only external customers but also internal service consumers, and these guidelines offer best practices useful for any service.

There are legitimate reasons for exemption from these guidelines. Obviously a REST service that implements or must interoperate with some externally defined REST API must be compatible with that API and not necessarily these guidelines. Some services MAY also have special performance needs that require a different format, such as a binary protocol.

### 4.2 Guidelines for existing services and versioning of services
We do not recommend making a breaking change to a service that pre-dates these guidelines simply for compliance sake. The service SHOULD try to become compliant at the next version release when compatibility is being broken anyway. When a service adds a new API, that API SHOULD be consistent with the other APIs of the same version. So if a service was written against version 1.0 of the guidelines, new APIs added incrementally to the service SHOULD also follow version 1.0. The service can then upgrade to align with the latest version of the guidelines at the service's next major release.

### 4.3 Requirements language
The keywords "MUST," "MUST NOT," "REQUIRED," "SHALL," "SHALL NOT," "SHOULD," "SHOULD NOT," "RECOMMENDED," "MAY," and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt). 

### 4.4 License

This work is licensed under the Creative Commons Attribution 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.

## 5 Taxonomy
As part of onboarding to Cancer Research REST API Guidelines, services MUST comply with the taxonomy defined below.

### 5.1 Errors
Errors, or more specifically Service Errors, are defined as a client passing invalid data to the service and the service _correctly_ rejecting that data. Examples include invalid credentials, incorrect parameters, unknown version IDs, or similar. These are generally "4xx" HTTP error codes and are the result of a client passing incorrect or invalid data.

Errors do _not_ contribute to overall API availability.

### 5.2 Faults
Faults, or more specifically Service Faults, are defined as the service failing to correctly return in response to a valid client request. These are generally "5xx" HTTP error codes.

Faults _do_ contribute to the overall API availability.

Calls that fail due to rate limiting or quota failures MUST NOT count as faults. Calls that fail as the result of a service fast-failing requests (often for its own protection) do count as faults.

### 5.3 Latency
Latency is defined as how long a particular API call takes to complete, measured as closely to the client as possible. This metric applies to both synchronous and asynchronous APIs in the same way. For long running calls, the latency is measured on the initial request and measures how long that call (not the overall operation) takes to complete.

### 5.4 Time to complete
Services that expose long operations MUST track "Time to Complete" metrics around those operations.

### 5.5 Long running API faults
For a Long Running API, it's possible for both the initial request to begin the operation and the request to retrieve the results to technically work (each passing back a 200), but for the underlying operation to have failed. Long Running faults MUST roll up as Faults into the overall Availability metrics.

## 6 Client guidance
To ensure the best possible experience for clients talking to a REST service, clients SHOULD adhere to the following best practices:

### 6.1 Ignore rule
For loosely coupled clients where the exact shape of the data is not known before the call, if the server returns something the client wasn't expecting, the client MUST safely ignore it.   

Some services MAY add fields to responses without changing versions numbers. Services that do so MUST make this clear in their documentation and clients MUST ignore unknown fields.

### 6.2 Variable order rule
Clients MUST NOT rely on the order in which data appears in JSON service responses. For example, clients SHOULD be resilient to the reordering of fields within a JSON object. When supported by the service, clients MAY request that data be returned in a specific order. For example, services MAY support the use of the _$orderBy_ querystring parameter to specify the order of elements within a JSON array. Services MAY also explicitly specify the ordering of some elements as part of the service contract. For example, a service MAY always return a JSON object's "type" information as the first field in an object to simplify response parsing on the client. Clients MAY rely on ordering behavior explicitly identified by the service.

### 6.3 Silent fail rule
Clients requesting OPTIONAL server functionality (such as optional headers) MUST be resilient to the server ignoring that particular functionality.

## 7 Consistency fundamentals
### 7.1 URL structure
Humans SHOULD be able to easily read and construct URLs.  

This facilitates discovery and eases adoption on platforms without a well-supported client library.

An example of a well-structured URL is:

```
https://api.example.com/v1/people/jdoe@example.com/inbox
```

An example URL that is not friendly is:

```
https://api.example.com/EWS/OData/Users('jdoe@Cancer Research.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')
```

A frequent pattern that comes up is the use of URLs as values. Services MAY use URLs as values. For example, the following is acceptable:

```
https://api.example.com/v1/items?url=https://resources.example.com/shoes/fancy
```

URLs SHOULD be lower cased with conjoined words being hyphen separated and query string parameters (especially ones that map to resource attributes) SHOULD be camelCased

```
https://api.example.com/v1/brass-razoos?
```

### 7.2 URL length
The HTTP 1.1 message format, defined in RFC 7230, in section [3.1.1][rfc-7230-3-1-1], defines no length limit on the Request Line, which includes the target URL. From the RFC:

> HTTP does not place a predefined limit on the length of a
   request-line. [...] A server that receives a request-target longer than any URI it wishes to parse MUST respond
   with a 414 (URI Too Long) status code.

Services that can generate URLs longer than 2,083 characters MUST make accommodations for the clients they wish to support. Here are some sources for determining what target clients support:

 * [http://stackoverflow.com/a/417184](http://stackoverflow.com/a/417184)
 * [https://blogs.msdn.Cancer Research.com/ieinternals/2014/08/13/url-length-limits/](https://blogs.msdn.Cancer Research.com/ieinternals/2014/08/13/url-length-limits/)
 
Also note that some technology stacks have hard and adjustable url limits, so keep this in mind as you design your services.

### 7.3 Canonical identifier
In addition to friendly URLs, resources that can be moved or be renamed SHOULD expose a URL that contains a unique stable identifier. It MAY be necessary to interact with the service to obtain a stable URL from the friendly name for the resource, as in the case of the "/my" shortcut used by some services.

The stable identifier is not required to be a GUID.  

An example of a URL containing a canonical identifier is:

```
https://api.example.com/v1/people/7011042402/inbox
```

### 7.4 Supported methods
Operations MUST use the proper HTTP methods whenever possible, and operation idempotency MUST be respected. HTTP methods are frequently referred to as the HTTP verbs. The terms are synonymous in this context, however the HTTP specification uses the term method.

Below is a list of methods that Cancer Research REST services SHOULD support. Not all resources will support all methods, but all resources using the methods below MUST conform to their usage.  

Method  | Description                                                                                                                | Is Idempotent
------- | -------------------------------------------------------------------------------------------------------------------------- | -------------
GET     | Return the current value of an object                                                                                      | True
PUT     | Replace an object, or create a named object, when applicable                                                               | True
DELETE  | Delete an object                                                                                                           | True
POST    | Create a new object based on the data provided, or submit a command                                                        | False
PATCH   | Apply a partial update to an object                                                                                        | False

<small>Table 1</small>

#### 7.4.1 POST
POST operations SHOULD support the Location response header to specify the location of any created resource that was not explicitly named, via the Location header.

As an example, imagine a service that allows creation of hosted servers, which will be named by the service:

```http
POST http://api.example.com/account1/servers
```

The response would be something like:

```http
201 Created
Location: http://api.example.com/account1/servers/server321
```

Where "server321" is the service-allocated server name.

Services MAY also return the full metadata for the created item in the response.

#### 7.4.2 PATCH
PATCH has been standardized by IETF as the method to be used for updating an existing object incrementally (see [RFC 5789][rfc-5789]). Cancer Research REST API Guidelines compliant APIs SHOULD support PATCH.  

### 7.5 Standard request headers
The table of request headers below SHOULD be used by Cancer Research REST API Guidelines services. Using these headers is not mandated, but if used they MUST be used consistently.

All header values MUST follow the syntax rules set forth in the specification where the header field is defined. Many HTTP headers are defined in [RFC7231][rfc-7231], however a complete list of approved headers can be found in the [IANA Header Registry][IANA-headers]."  

Header                            | Type                                  | Description
--------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Authorization                     | String                                           | Authorization header for the request
Date                              | Date                                             | Timestamp of the request in [RFC 3339][rfc-3339] format
Accept                            | Content type                                     | The requested content type for the response such as: <ul><li>application/xml</li><li>text/xml</li><li>application/json</li><li>text/javascript (for JSONP)</li></ul>Per the HTTP guidelines, this is just a hint and responses MAY have a different content type, such as a blob fetch where a successful response will just be the blob stream as the payload. For services following OData, the preference order specified in OData SHOULD be followed.
Accept-Encoding                   | Gzip, deflate                                    | REST endpoints SHOULD support GZIP and DEFLATE encoding, when applicable. For very large resources, services MAY ignore and return uncompressed data.
Accept-Language                   | "en", "es", etc.                                 | Specifies the preferred language for the response. Services are not required to support this, but if a service supports localization it MUST do so through the Accept-Language header.
Accept-Charset                    | Charset type like "UTF-8"                        | Default is UTF-8, but services MAY also be able to handle ISO-8859-1.
Content-Type                      | Content type                                     | Mime type of request body (PUT/POST/PATCH)
Prefer                            | return=minimal, return=representation            | If the return=minimal preference is specified, services SHOULD return an empty body in response to a successful insert or update. If return=representation is specified, services SHOULD return the created or updated resource in the response. Services SHOULD support this header if they have scenarios where clients would sometimes benefit from responses, but sometimes the response would impose too much of a hit on bandwidth.
X-CRUK-Session-ID                 | GUID                                             | A unique identifier tied to a user session. Used to log interactions for debugging
X-CRUK-Request-ID                 | GUID                                             | A unique identifier for a particular request. Used to log interactions for debugging

### 7.6 Standard response headers
Services SHOULD return the following response headers, except where noted in the "required" column.

Response Header    | Required                                      | Description
------------------ | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Date               | All responses                                 | The date the request was processed, in [RFC 3339][rfc-3339] format
Content-Type       | All responses                                 | The content type
Content-Encoding   | All responses                                 | GZIP or DEFLATE, as appropriate
Preference-Applied | When specified in request                     | Whether a preference indicated in the Prefer request header was applied

### 7.7 Custom headers
Custom headers MUST NOT be required for the basic operation of a given API.

Some of the guidelines in this document prescribe the use of nonstandard HTTP headers. In addition, some services MAY need to add extra functionality, which is exposed via HTTP headers. The following guidelines help maintain consistency across usage of custom headers.

Headers that are not standard HTTP headers MUST have one of two formats:

1. A generic format for headers that are registered as "provisional" with IANA ([RFC 3864][rfc-3864])
2. A scoped format for headers that are too usage-specific for registration

These two formats are described below.

### 7.8 PII parameters
Consistent with their organization's privacy policy, clients SHOULD NOT transmit personally identifiable information (PII) parameters in the URL (as part of path or query string) because this information can be inadvertently exposed via client, network, and server logs and other mechanisms.

Consequently, a service SHOULD accept PII parameters transmitted as headers.  

However, there are many scenarios where the above recommendations cannot be followed due to client or software limitations. To address these limitations, services SHOULD also accept these PII parameters as part of the URL consistent with the rest of these guidelines.  

Services that accept PII parameters -- whether in the URL or as headers -- SHOULD be compliant with privacy policy specified by their organization's engineering leadership. This will typically include recommending that clients prefer headers for transmission and implementations adhere to special precautions to ensure that logs and other service data collection are properly handled.

### 7.9 Response formats
For organizations to have a successful platform, they must serve data in formats developers are accustomed to using, and in consistent ways that allow developers to handle responses with common code.

Web-based communication, especially when a mobile or other low-bandwidth client is involved, has moved quickly in the direction of JSON for a variety of reasons, including its tendency to be lighter weight and its ease of consumption with JavaScript-based clients.

JSON property names MUST be camelCased.

Services SHOULD provide JSON as the default encoding.

The message data returned SHOULD be 'pretty printed' to make the API more usable for development/debugging. Adding some whitespace will not appreciably increase transfer data, especially where gzip encoding is also supported.

#### 7.9.1 Clients-specified response format
In HTTP, response format SHOULD be requested by the client using the Accept header. This is a hint, and the server MAY ignore it if it chooses to, even if this isn't typical of well-behaved servers. Clients MAY send multiple Accept headers and the service MAY choose one of them.

The response format MAY be specified in the URL after the dot. (e.g. .json/.xml) This MUST be used to override the Accept header, if specified.

The default response format (no Accept header provided) SHOULD be application/json, and all services MUST support application/json.

Accept Header    | Response type                      | Notes
---------------- | ---------------------------------- | -------------------------------------------
application/json | Payload SHOULD be returned as JSON | Also accept text/javascript for JSONP cases

```http
GET https://api.example.com/v1/products/user
Accept: application/json
```

#### 7.9.1.1 Expanded Nested Resources

With more complex data structures, you SHOULD allow the use of the _expand_ query string parameter when using the GET method that tells the server to return a 'sensible' expanded subset of data. By setting _expand_ to true, the server is told to return nested resources of the resource that has been requested.

You MAY also implement it for other types of request such as PUT, PATCH, POST.

The depth and complexity of the nesting when expanded SHOULD be defined by the service.

##### Examples

A non-expanded request

```json
{
  "eventCode": "N16RLN",
  "bla": "..."
}
```

An expanded request

```json
{
  "id": "evt-8f8yuwre9frweofwre",
  "eventCode": "N16RLN",
  "waves": [
    {
      "wav-oeiurhgiouhgreuih",
      "waveName": "Glen",
      "capacityGroups": [
        {
          "id": "cap-uherwgiuerh",
          "name": "sausage"
        }
      ]
    }
  ]
  "bla": "..."
}
```

#### 7.9.2 Error condition responses

For non-success conditions, developers SHOULD be able to write one piece of code that handles errors consistently across different Cancer Research REST API Guidelines services. This allows building of simple and reliable infrastructure to handle exceptions as a separate flow from successful responses.

The error response MUST be a single JSON object. This object MUST contain name/value pairs with the names "error" and "errorDescription," and it MAY contain name/value pairs with the names "debug" and "data".

The value for the "error" name/value pair is a language-independent string. Its value is a service-defined error code that SHOULD be human-readable. This code serves as a more specific indicator of the error than the HTTP error code specified in the response. Services SHOULD have a relatively small number (about 20) of possible values for "error," and all clients SHOULD be capable of handling all of them.

The value for the "errorDescription" name/value pair MUST be a human-readable representation of the error. It is intended as an aid to developers and is not suitable for exposure to end users.

The value for the "debug" name/value pair MAY be present and is used to relay more specific debugging information. This MUST NOT be present on production systems.

The value for "data" MUST be used for 409 validation errors to provide specific feedback on what field(s) have been submitted incorrectly.

This gives us two similar types of error document. One that is specifically for data validation errors and one for everything else, both of which share a common root. Below are their definitions in Swagger notation:

```json
        "ResponseError": {
            "required": [
                "error",
                "errorDescription"
            ],
            "properties": {
                "error": {
                    "type": "string",
                    "readOnly": true,
                    "example": "validation"
                },
                "errorDescription": {
                    "type": "string",
                    "readOnly": true,
                    "example": "There's something wrong with your data"
                }
            },
            "xml": {
                "name": "error"
            }
        },
        "ResponseErrorValidation": {
            "description": "Standard Error Response structure where validations have been tripped",
            "required": [
                "error",
                "errorDescription",
                "data"
            ],
            "properties": {
                "error": {
                    "type": "string",
                    "readOnly": true,
                    "example": "validation"
                },
                "errorDescription": {
                    "type": "string",
                    "readOnly": true,
                    "example": "There's something wrong with your data"
                },
                "data": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/ResponseErrorValidationItem"
                    },
                    "maxItems": 30,
                    "minItems": 1,
                    "readOnly": true
                }
            },
            "xml": {
                "name": "errorValidation"
            }
        },
        "ResponseErrorValidationItem": {
            "description": "A lovely chunk of error",
            "required": [
                "field",
                "errorMessages"
            ],
            "properties": {
                "field": {
                    "type": "string",
                    "readOnly": true,
                    "example": "participant.address.postalCode"
                },
                "errorMessages": {
                    "type": "array",
                    "items": {
                        "type": "string",
                        "readOnly": true,
                        "example": "Invalid format"
                    },
                    "maxItems": 30,
                    "minItems": 1
                }
            },
            "xml": {
                "name": "item"
            }
        }
```

##### Examples

Example of a standard error

```json
{
  "error": "timed_out",
  "errorDescription": "Access to this resource has been timed out",
  "debug": [
    {}
  ]
}
```

Example of a validation error

```json
{
  "error": "validation",
  "errorDescription": "There's something wrong with your data",
  "data": [
    {
      "field": "participant.address.postalCode",
      "errorMessages": [
        "Invalid format"
      ]
    }
  ]
}
```

### 7.10 HTTP Status Codes
Standard HTTP Status Codes MUST be used. Below are a list of the ones that MUST be used and the circumstances in which they MUST be used

Code | Usage |
---- | ----- |
400  | A 'catch all' for all caught exceptions
401  | Authentication failure. For example, an invalid oauth2 token has been sent
403  | Authorization failure. For example, you are not allowed to access a resource.
404  | Resource does not exist
409  | Validation errors

### 7.11 Client library optional
Developers MUST be able to develop on a wide variety of platforms and languages, such as Windows, MacOS, Linux, C#, Python, Node.js, and Ruby.  

Services SHOULD be able to be accessed from simple HTTP tools such as curl/Postman without significant effort.

## 9 Collections
### 9.1 Item keys
Services MAY support durable identifiers for each item in the collection, and that identifier SHOULD be represented in JSON as "id". These durable identifiers are often used as item keys.

### 9.2 Serialization
Collections should be represented in the following format when returned:

```json
{
  "metadata": {
    "count": 238,
    "offset": 0,
    "limit": 50
  },
  "results": [
    {
      .. result object ..
    }
  ]
}
```

In the ```metadata``` section the fields are defined as:

* count: To total number of records in the record set (NOT the total number returned in the query). This is handy for paging
* offset: The number of records into the collection this result set is from. 0 is the first item.
* limit: The maximum possible number of results in the result set. (NOTE it is possible for the limit number therefore to be higher than the total number of results)

In the ```results``` section we simply have an array of each item. If there are no items returned in the search, the ```results``` key should still exist, containing an empty array. 

### 9.3 Collection URL patterns
Collections are located directly under the service root when they are top level, or as a segment under another resource when scoped to that resource.

For example:

```http
GET https://api.example.com/v1/people
```

#### 9.3.1 Nested collections and properties
Collection items MAY contain other collections. For example, a user collection MAY contain user resources that have multiple addresses:

```http
GET https://api.example.com/v1/people/123/addresses
```

```json
{
  "metadata": {
    "count": 10,
    "offset": 0,
    "limit": 50
  },
  "results": [
    {
      "id": 123,
      "name": "John",
      "addresses": [
        {"street":"1st Avenue","city":"Seattle"},
        {"street":"124th Ave NE","city":"Redmond"}
      ]
    }
  ]
}
```

Nested collections do not require metadata.

### 9.5 Changing collections
POST requests are not idempotent. This means that two POST requests sent to a collection resource with exactly the same payload MAY lead to multiple items being created in that collection. This is often the case for insert operations on items with a server-side generated id.

For example, the following request:

```http
POST https://api.example.com/v1/people
```

Would lead to a response indicating the location of the new collection item:

```http
201 Created
Location: https://api.example.com/v1/people/123
```

And once executed again, would likely lead to another resource:

```http
201 Created
Location: https://api.example.com/v1/people/124
```

While a PUT request would require the indication of the collection item with the corresponding key instead:

```http
PUT https://api.example.com/v1/people/123
```

### 9.6 Sorting collections
The results of a collection query MAY be sorted based on property values. The property is determined by the value of the _orderBy_ query parameter.

The value of the _orderBy_ parameter contains a comma-separated list of expressions used to sort the items. A special case of such an expression is a property path terminating on a primitive property.

The expression MAY include the suffix "asc" for ascending or "desc" for descending, separated from the property name by one colon (:). If "asc" or "desc" is not specified, the service MUST order by the specified property in ascending order.

NULL values MUST sort as "less than" non-NULL values.

Items MUST be sorted by the result values of the first expression, and then items with the same value for the first expression are sorted by the result value of the second expression, and so on. The sort order is the inherent order for the type of the property.

For example:

```http
GET https://api.example.com/v1/people?orderBy=name
```

Will return all people sorted by name in ascending order.

For example:

```http
GET https://api.example.com/v1/people?orderBy=name:desc
```

Will return all people sorted by name in descending order.

Sub-sorts can be specified by a comma-separated list of property names with OPTIONAL direction qualifier.

For example:

```http
GET https://api.example.com/v1/people?orderBy=name:desc,hireDate
```

Will return all people sorted by name in descending order and a secondary sort order of hireDate in ascending order.

#### 9.6.1 Interpreting a sorting expression
Sorting parameters MUST be consistent across pages, as both client and server-side paging is fully compatible with sorting.

### 9.7 Filtering

Filtering of collections SHOULD be done via the query string by adding the attribute name, an equals sign, and the value that it should equal. Filtering by every possible attribute in a resource MAY NOT be supported. The attributes that it is possible to filter by MUST BE documented.

Example: returning all closed events

```http
GET https://api.example.com/v1/events?statusCode=closed
```

Filtering by multiple values of an attribute should be achieved by comma separated query string values.

Example: returning all closed or cancelled events

```http
GET https://api.example.com/v1/events?statusCode=closed,cancelled
```

Filtering by multiple attributes is achieved by multiple key/value pairs. This SHOULD always result in an AND query.

Example: returning all male only events that have been closed or cancelled

```http
GET https://api.example.com/v1/events?statusCode=closed,cancelled&gender=male
```

> DISCUSS
> NOTE: We don't want to force all API developers to support a complex query syntax so for more complex requirements (such as matching up OR and AND queries, the onus is the API developer to be as bespoke as is required)

A logical NOT MAY BE represented as a minus sign before the value

Example: returning all non-male events

```http
GET https://api.example.com/v1/events?gender=-male
```

Ranges MAY BE represented by adding the following to the end to the attribute:

* GT = Greater Than
* GTE = Greater Than or Equals to
* LT = Less Than
* LTE = Less Than or Equals to

Example: returning all events created on 5th September 2016

```http
GET https://api.example.com/v1/events?createdGTE=2016-09-05T00:00:00&createdLT=2016-09-06T00:00:00
```

> DISCUSS
> I've tried to err on the side of simplicity and cater for the 90% of simple-case searches that we'll actually do rather than force API writers to support an entire query language.

### 9.8 Paging

#### 9.8.1 Server-driven paging

For all collections that are returned, if not specified by the client, the server MUST implement a hard limit that cannot be exceeded to stop DOS attacks (both intentional or otherwise). The limit SHOULD BE based on the collection itself. For example, if the collection is a simple list of values, the limit may be high, whereas for complex, nested collections, the limit might need to be low. Plan for success and ensure that the limit is in place even if there isn't much data in the collection today.

As mentioned above, all collections should come with a metadata block to allow the called to page throw the results (see Client-driven paging)

#### 9.8.2 Client-driven paging

Collections MUST support the _offset_ and _limit_ query string parameters that allow the client to page through the collection. _offset_ MUST start at 0 for the first record. If the _limit_ exceeds the collection's hard limit it MUST NOT result in an error being thrown, the hard limit MUST be used as the limit value instead.

Example: Requesting the second page of 10 results from a collection of events

```http
GET http://api.example.com/v1/events?offset=10&limit=10
```

#### 9.8.3 Additional considerations
**Stable order prerequisite:** Both forms of paging depend on the collection of items having a stable order. The server MUST supplement any specified order criteria with additional sorts (typically by key) to ensure that items are always ordered consistently.

**Missing/repeated results:** Even if the server enforces a consistent sort order, results MAY be missing or repeated based on creation or deletion of other resources. Clients MUST be prepared to deal with these discrepancies.

**Combining client- and server-driven paging:** Note that client-driven paging does not preclude server-driven paging. If the page size requested by the client is larger than the default page size supported by the server, the expected response would be the number of results specified by the client, paginated as specified by the server paging settings.

**Paginating embedded collections:** This SHOULD NOT be supported to cut down on system complexity. The API designer SHOULD be mindful of resources that contain large nested connections and expose them in their own endpoint.

### 9.9 Compound collection operations
Filtering, Sorting and Pagination operations MAY all be performed against a given collection. When these operations are performed together, the evaluation order MUST be:

1. **Filtering**. This includes all range expressions performed as an AND operation.
2. **Sorting**. The potentially filtered list is sorted according to the sort criteria.
3. **Pagination**. The materialized paginated view is presented over the filtered, sorted list. This applies to both server-driven pagination and client-driven pagination.

## 11 JSON standardizations
### 11.1 JSON formatting standardization for primitive types
Primitive values MUST be serialized to JSON following the rules of [RFC4627][rfc-4627].

### 11.2 Guidelines for dates and times
#### 11.2.1 Producing dates
Services MUST produce dates using the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard (e.g. 2017-02-26T20:00:00+00:00). [Week dates](https://en.wikipedia.org/wiki/ISO_8601#Week_dates) and [Ordinal dates](https://en.wikipedia.org/wiki/ISO_8601#Ordinal_dates) MUST NOT be returned.

#### 11.2.2 Consuming dates
Services MUST accept dates using the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) standard (e.g. 2017-02-26T20:00:00+00:00). [Week dates](https://en.wikipedia.org/wiki/ISO_8601#Week_dates) and [Ordinal dates](https://en.wikipedia.org/wiki/ISO_8601#Ordinal_dates) MAY BE supported.

### 11.3 Durations
[Durations][wikipedia-iso8601-durations] need to be serialized in conformance with [ISO 8601][wikipedia-iso8601-durations]. Durations are "represented by the format `P[n]Y[n]M[n]DT[n]H[n]M[n]S`." From the standard:
- P is the duration designator (historically called "period") placed at the start of the duration representation.
- Y is the year designator that follows the value for the number of years.
- M is the month designator that follows the value for the number of months.
- W is the week designator that follows the value for the number of weeks.
- D is the day designator that follows the value for the number of days.
- T is the time designator that precedes the time components of the representation.
- H is the hour designator that follows the value for the number of hours.
- M is the minute designator that follows the value for the number of minutes.
- S is the second designator that follows the value for the number of seconds.

For example, "P3Y6M4DT12H30M5S" represents a duration of "three years, six months, four days, twelve hours, thirty minutes, and five seconds."

### 11.4 Intervals
[Intervals][wikipedia-iso8601-intervals] are defined as part of [ISO 8601][wikipedia-iso8601-intervals].
- Start and end, such as "2007-03-01T13:00:00Z/2008-05-11T15:30:00Z"
- Start and duration, such as "2007-03-01T13:00:00Z/P1Y2M10DT2H30M"
- Duration and end, such as "P1Y2M10DT2H30M/2008-05-11T15:30:00Z"
- Duration only, such as "P1Y2M10DT2H30M," with additional context information

### 11.5 Repeating intervals
[Repeating Intervals][wikipedia-iso8601-repeatingintervals], as per [ISO 8601][wikipedia-iso8601-repeatingintervals], are:

> Formed by adding "R[n]/" to the beginning of an interval expression, where R is used as the letter itself and [n] is replaced by the number of repetitions. Leaving out the value for [n] means an unbounded number of repetitions.

For example, to repeat the interval of "P1Y2M10DT2H30M" five times starting at "2008-03-01T13:00:00Z," use "R5/2008-03-01T13:00:00Z/P1Y2M10DT2H30M."

## 12 Versioning
**All APIs compliant with the Cancer Research REST API Guidelines MUST support explicit versioning.** It's critical that clients can count on services to be stable over time, and it's critical that services can add features and make changes.

### 12.1 Versioning formats
Services are versioned using a MAJOR versioning scheme as defined by [Semantic Versioning](http://semver.org/). The version is to be specified in the URL, prefixed with a 'v':

```http
GET https://api.example/v1/things
```

### 12.2 When to version
Services MUST increment their version number in response to any breaking API change. See the following section for a detailed discussion of what constitutes a breaking change. Services MAY increment their version number for nonbreaking changes as well, if desired.

Use a new major version number to signal that support for existing clients will be deprecated in the future. When introducing a new major version, services MUST provide a clear upgrade path for existing clients and develop a plan for deprecation that is consistent with their business group's policies.

Online documentation of versioned services MUST indicate the current support status of each previous API version and provide a path to the latest version.

### 12.3 Definition of a breaking change
Changes to the contract of an API are considered a breaking change. Changes that impact the backwards compatibility of an API are a breaking change.

Clear examples of breaking changes:

1. Removing or renaming APIs or API parameters
2. Changes in behavior for an existing API
3. Changes in Error Codes and Fault Contracts
4. Anything that would violate the [Principle of Least Astonishment][principle-of-least-astonishment]

## 13 Long running operations

Long running operations, sometimes called async operations, refer to operations where the client is not expected to wait for the final answer. 

There are two obvious ways that you might then get the result. One is by polling the server and asking "are we nearly there yet?" like an annoying child on a car journey, or you can use callbacks, so when the server as completed its task, it tells the client that it's done.

This document only outlines a method for the latter as it's a more sensible way to handling inter-service communication. The former is useful when handling requests from a Client that has no fixed, always-on endpoint to call back to (like a direct connection from a web browser via an AJAX request.)

### 13.1 Pre-Flighting

Services MUST perform as much synchronous validation as practical on requests. Services MUST prioritize returning errors in a synchronous way, with the goal of having only "Valid" operations processed using the long running operation.

### 13.2 The Callback mechanism

If a client cares about the result of a long running operation they MUST provide a callback URL as part of the request header (so as not to muddy the request body).

The callback URL SHOULD NOT be behind oauth2 authentication. It MUST contain a unique token in the URL is what will be used to allow access to the callback.

The callback URL token MUST provide one-time-only access to the callback.

The callback token SHOULD be time restricted and reject access if that time limit has been exceeded.

Example:

```http
POST https://api.example.com/v1/long-running-thing HTTP/1.1
Host: api.example.com
X-CRUK-Callback-Url: https://api2.example.com/v2/callbacks/long-running-thing/909062ec-6009-4dbe-a2be-3b080d5376dc
Content-Type: application/json
Accept: application/json

{
  "id": "8be4885a-36c8-4b93-be42-0ab19721a1ac"
}
```

The server MUST then return an accepted header to let you know that all pre-flighting has happened and happened and it's started processing.

```http
HTTP/1.1 202 Accepted
```

Once the long running process has completed, the server should perform a POST to the callback url it was sent with any payload data that is required.

```http
POST https://api2.example.com/v2/callbacks/long-running-thing/909062ec-6009-4dbe-a2be-3b080d5376dc HTTP/1.1
Host: api2.example.com
Content-Type: application/json
Accept: application/json

{
  "id": "8be4885a-36c8-4b93-be42-0ab19721a1ac",
  "status": "done"
}
```

The server MAY log if the callback is rejected (for example, if the token has timed out) but MUST NOT break.

### 14.8 Security
All service URLs must be HTTPS (that is, all inbound calls MUST be HTTPS). Services that deal with Callbacks MUST accept HTTPS.

Services MUST NOT redirect HTTP requests to their equivalent HTTPS resource. They MUST return a 404 response code. The reason for this is that your POST to an HTTP end point would get redirected to a GET on the HTTPS version of the URL which would certainly produce different results from those expected.

We recommend that services that allow client defined Callback URLs SHOULD NOT transmit data over HTTP. This is because information can be inadvertently exposed via client, network, server logs and other mechanisms.

## 15 Unsupported requests
RESTful API clients MAY request functionality that is currently unsupported. RESTful APIs MUST respond to valid but unsupported requests consistent with this section.

### 15.1 Essential guidance
RESTful APIs will often choose to limit functionality that can be performed by clients. For instance, auditing systems allow records to be created but not modified or deleted. Similarly, some APIs will expose collections but require or otherwise limit filtering and ordering criteria, or MAY not support client-driven pagination.

### 15.2 Feature allow list
If a service does not support any of the below API features, then an error response MUST be provided if the feature is requested by a caller. The features are:
- Key Addressing in a collection, such as: `https://api.example.com/v1/people/user1@example.com`
- Filtering a collection by a property value, such as: `https://api.example.com/v1/people?name=david`
- Filtering a collection by range, such as: `http://api.example.com/v1/people?hireDateGTE=2014-01-01&hireDateLTE=2014-12-31`
- Client-driven pagination via offset and limit, such as: `http://api.example.com/v1/people?offset=5&limit=2`
- Sorting by orderBy, such as: `https://api.example.com/v1/people?orderBy=name:desc`

#### 15.2.1 Error response
Services MUST provide an error response if a caller requests an unsupported feature found in the feature allow list. The error response MUST be an HTTP status code from the 4xx series, indicating that the request cannot be fulfilled. Unless a more specific error status is appropriate for the given request, services SHOULD return "400 Bad Request" and an error payload conforming to the error response guidance provided in the Cancer Research REST API Guidelines. Services SHOULD include enough detail in the response message for a developer to determine exactly what portion of the request is not supported.

Example:

```http
GET https://api.example.com/v1/people?orderBy=name HTTP/1.1
Accept: application/json
```

```http
HTTP/1.1 409 Conflict
Content-Type: application/json

{
  "error": "validation",
  "errorDescription": "One or more submitted fields have errors with the data.",
  "data": [
    {
      "field": "orderDirection",
      "errorMessages": [
        "This value is not valid."
      ]
    }
  ]
}
```

## 16 Appendix
[fielding]: https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
[IANA-headers]: http://www.iana.org/assignments/message-headers/message-headers.xhtml
[rfc7231-7-1-1-1]: https://tools.ietf.org/html/rfc7231#section-7.1.1.1
[rfc-7230-3-1-1]: https://tools.ietf.org/html/rfc7230#section-3.1.1
[rfc-7231]: https://tools.ietf.org/html/rfc7231
[rest-in-practice]: http://www.amazon.com/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829/
[rest-on-wikipedia]: http://en.wikipedia.org/wiki/Representational_state_transfer
[rfc-5789]: http://tools.ietf.org/html/rfc5789
[rfc-5988]: http://tools.ietf.org/html/rfc5988
[rfc-3339]: https://tools.ietf.org/html/rfc3339
[cors-preflight]: http://www.w3.org/TR/cors/#resource-preflight-requests
[rfc-3864]: http://www.ietf.org/rfc/rfc3864.txt
[odata-json-annotations]: http://docs.oasis-open.org/odata/odata-json-format/v4/os/odata-json-format-v4.0-os.html#_Instance_Annotations
[cors]: http://www.w3.org/TR/access-control/
[cors-user-credentials]: http://www.w3.org/TR/access-control/#user-credentials
[cors-simple-headers]: http://www.w3.org/TR/access-control/#simple-header
[rfc-4627]: https://tools.ietf.org/html/rfc4627
[iso-8601]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15
[clr-time]: https://msdn.Cancer Research.com/en-us/library/System.DateTime(v=vs.110).aspx
[ecmascript-time]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.1
[ole-date]: https://msdn.Cancer Research.com/en-us/library/windows/desktop/ms221199(v=vs.85).aspx
[ticks-time]: https://msdn.Cancer Research.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[unix-time]: https://msdn.Cancer Research.com/en-us/library/1f4c8f33.aspx
[windows-time]: https://msdn.Cancer Research.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[excel-time]: http://support.Cancer Research.com/kb/214326?wa=wsignin1.0
[wikipedia-iso8601-durations]: http://en.wikipedia.org/wiki/ISO_8601#Durations
[wikipedia-iso8601-intervals]: http://en.wikipedia.org/wiki/ISO_8601#Time_intervals
[wikipedia-iso8601-repeatingintervals]: http://en.wikipedia.org/wiki/ISO_8601#Repeating_intervals
[principle-of-least-astonishment]: http://en.wikipedia.org/wiki/Principle_of_least_astonishment
[odata-breaking-changes]: http://docs.oasis-open.org/odata/odata/v4/errata02/os/complete/part1-protocol/odata-v4.0-errata02-os-part1-protocol-complete.html#_Toc406398209
[websequencediagram-firehose-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDogTWFudWFsAIFzEQoKCgCDAgo8LS0-AIIqCiA6IExvZ2luIGludG8Agj8JAII1ECBVWCAKACoKLT4gKwCCWBM6AIQGBU5hbWUgZXRjLgCDFQ4AGxJDb25maXJtAIEBCEFjY2VzcyBUb2tlbgoKAIM3EyAtPiAtAINkCQBnBklEAIEMCwCBVQUAhQIMAIR3CmNvcGllcwArCACCIHAAhHMMAIMKDwCDABg6IHdlYmhvb2sgcgCCeg4AgnUSAIVQDToAhXYHZXIAgwgGAIcTBgBECVVSTCwgU2NvcGUAhzIGSUQKTgCGPQwAhhwNIACDBh4AHhEAgxEPbgCBagwAgxwNAIMaDiAAgx0MbWF5IGNvcHkALREAhVtqAIZHB0F1dGhvcml6AIY7BwCGXQctPiArAIEuDVJlcXVlc3QgYQCFOQZ0byBEQiBwcm90ZWN0ZWQgaW5mb3IAiiQGCgCDBQstPiAtAIctCVJlZGlyZWN0ADYHAGwNIGVuZHBvaW50AIoWBmEADw1yAHYGAIEQDACJVAcASwtlZAAYHgCICAgAMAcAcA4AhGoGAE0FAIEdFmJhY2sgdG8AhF8NaXRoIGNvZGUAghoaaQCBagcAgToHAD0JAII-B3MAPgsAglEHAEsFAIIzDgCBXw0Agn8GdG9rZW5zACcSAI0_BXJpZ2h0IG9mAItpDUNhY2hlIHRoYXQgdGhpcyBVc2VyIElEIHByb3ZpZGVkAINNCwCIZgoAggcJAIN7D3Nwb25zAI0_BwCECgYsIHJlZnJlc2gsIGFuZCBJRACBHAcAgQMPAIYADQCBDAcAgUUGYnkAjFkFIElEAIQkG3R1cm4AhF4MIHRvIGMAjR8FAIwRagCJVw1GbG93AIYqCQCMaQgAgmoKaGFuZ2UAj3YFAIFXBWRhdGEgLSB0eXBpY2FsIHZpYQCQDgVyYWN0aW5nAJAPBgCJQQt2aWEAjnsHCgCPNgogAIhDEACKZw0AkFMFAIkBDwCDDAUAgkYWKwBNCwCHWApjAIEyBQCHRg0AhWUHYWNoAIQeDACEfwVhbmQgInNpbmNlIgCFEQYAkSQOAIR3CgCNfwcAhHQFAIpQEACBUgsAhFAcAII8BWFuZCBuZXcAYRQAhFUTOiBVcGRhdGUgc3RhdHUAgSkGAIFDBQAxEwoKCg&s=mscgen
[websequencediagram-user-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDoAgWwRCgphbHQAgyUIAIEHBiByABQMICAAgxsLPC0tPgCDTws6IENvbmZpZ3VyZQogIACDaAsgLT4gKwCCWBMAegZOYW1lIGV0Yy4AhAgFAIMaDQAfEgBdBXJtAIQ_BUFjY2VzcyBUb2tlAIETBgCDOxIgLT4gLQCBFgxBcHAgSUQAhHwIY3JldACBGxAtPgCFFgsgOiBFbWJlZAAkFGVsc2UgTWFudWFsAIIEJACEbQkgOiBMb2dpbiBpbnRvAIUBCQCBKRFVWACGGAUALQoAgh8mAIIZKwCBCAcAgjoNAIIsHACGLwkAgj8IAIESDgCECAYAh1ELAIdFCmNvcGllcwAuCGVuZACEeGoAhWQHQXV0aG9yaXoAhV8HAIV6By0-ICsAg2ANUmVxdWVzdCBhAIRVBnRvIERCIHByb3RlY3RlZCBpbmZvcgCJQQYKAIQaCy0-IC0AhkoJUmVkaXJlY3QANgcAbA0gZW5kcG9pbnQAiTMGYQAPDXIAdgYAgRAMAIhxBwBLC2VkABgeAIRjCAAwB0EAcQxVWAoASQgAgRwWYmFjayB0bwCFdAwAilwFY29kZQCCGRppAIFpBwCBOQcAPQkAgj0HcwA-CwCCUAcASwUAgjIOAIFeDQCCfgZ0b2tlbnMAJxIAjFsFcmlnaHQgb2YAiwUNQ2FjaGUgdGhhdCB0aGlzIFVzZXIgSUQgcHJvdmlkZWQAg0wLAIU6BwCCBAwAg3oPc3BvbnMAjFsHAIQJBiwgcmVmcmVzaCwgYW5kIElEAIEcBwCBAw8AiDENAIEMBwCBRQZieQCLdQUgSUQAhCMbdHVybgCEXQwgdG8gYwCMOwUKCgCLL2oAjXUMAIwTDwCPNQotPisAjhwQOgCORQdlcgCMVwYAg3YIZWJob29rIFVSTCwgU2NvcGUAkAEGSUQAjwoOAI5rDSAAi2UKAINFBQCLYw0AHBEAgzUOOiBuAIE2DABgCACDCB1oZQCBaQ5JRACDYwUAahIAghB4RmxvdwCJMwkAjE0IAIV0CmhhbmdlAJIcBQCEYQVkYXRhIC0gdHlwaWNhbCB2aWEAkjQFcmFjdGluZwCSNQYAjV8LdmlhAJEhBwoAkVwKIACNfhAAhAsNAJJ5BQCCWQ8AhhYFAIVQFisATQsAimEKYwCBMgUAik8NAIhvB2FjaACHKAwAiAkFYW5kICJzaW5jZSIAiBsGAJNKDgCIAQoAhB0cAIFSCwCHWhwAgjwFYW5kIG5ldwBhFACHXxM6IFVwZGF0ZSBzdGF0dQCBKQYAgUMFADETCgoK&s=mscgen
