---
title: API Reference

language_tabs:
  - cURL


toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - HTTPStatusCodes

search: true
---

# Introduction

Welcome to the SailPoint IdentityIQ API! The IdentityIQ API provides access to the IdentityIQ 
platform, allowing new opportunities for expanded innovation.  The IdentityIQ API is standards-
based, built upon the SCIM 2.0 specification (http://www.simplecloud.info/).  You can use our 
API to access IdentityIQ API endpoints, which allows you to programmatically interact with objects within IdentityIQ.

##Getting Started
1. Read the IdentityIQ API Terms of Use (https://community.sailpoint.com/docs/DOC-6078)
2. If you are unfamiliar with the SCIM 2.0 specification, or need a refresher, we suggest you 
start here: SCIM Overview (http://www.simplecloud.info/#overview).
3. Ensure you have IdentityIQ 7.0 Patch 2 installed.
4. Read our documentation.  All you have to do is keep scrolling!
5. Participate on the forums.  Ask questions, read about requested and upcoming functionality, and provide assistance to others.
6. Send us feedback! We want to hear from you.

##SCIM Protocol
SCIM stands for System for Cross-Domain Identity Management, and it is an HTTP-based protocol 
that makes managing identities in multi-domain scenarios easier to support through a 
standardized RESTful API service.  It provides a platform neutral schema and extension model for 
representing users, groups and other resource types in JSON format.

The core schema consists of five resource types, as described below:

Resource Type | Endpoint | Description | Supported Methods  
------------- | -------- | ----------- | -----------------
User | /Users | Various attributes to describe the identity | GET, PUT, POST, DELETE
Group | /Groups | Currently not supported | N/A
Service Provider Configuration | /ServiceProviderConfig | Provides SCIM specification features and implementation details to clients | GET
Resource Types | /ResourceTypes | Defines attributes and metadata on supported resource types | GET
Schemas | /Schemas | Schema definitions and attributes for all schemas | GET

##Supported HTTP Methods

GET
POST
PUT
DELETE

#Authentication

##Basic Authentication
In IdentityIQ version 7.0, Patch 2, Basic Authentication is used to allow access to the API.  Basic authentication is a simple technique for enforcing access controls to API resources because it doesn't require cookie, session IDs, and instead uses the standard fields available in an HTTP header.  Please see the IETF protocol for more information on basic authentication.  https://www.ietf.org/rfc/rfc2617.txt and https://tools.ietf.org/html/rfc1945#section-11.

##OAuth 2.0

<aside class="notice">
OAuth 2.0 Authentication will be supported in IdentityIQ Version 7.1.  Versions prior to 7.1 only support Basic Authentication.
</aside>

After configuring an OAuth2 API Credential you can access the token endpoint using your favorite client.  
>**Sample Request**

```cURL
curl --user clientid:secret -d grant_type=client_credentials 
"http://localhost:8080/identityiq/oauth2/token"
```

>**Sample Response**
```
{"expires_in":1200,"token_type":"bearer","access_token":"bHRiYWVUVk5ERzFrSjdzUHNFNUllWFFjM1NOTHZVbW0uODFyVkZlVC8rcnB1bVpGNHBVZ2grWWMrdVA0bk9idjJwMUhuTE83QzR3MUJWb2pCc2VFbU5LTnZXaEt6MDNVeVIzZlBpams2WGg2cwpaSzJITnVNUEVvaTVtRXVTenJVWDNnSzFvSjNndnM5eWRKcTh3aVNyeW12MzdhN3BZRFpJOWtyY2NaSXM4a3lIY0xnQU1IYnFIZz09"
}
```

Using the access_token value you can then make requests to any SCIM endpoint using Authorization: Bearer in the header.
```curl -H "Authorization: Bearer bHRiYWVUVk5ERzFrSjdzUHNFNUllWFFjM1NOTHZVbW0uODFyVkZlVC8rcnB1bVpGNHBVZ2grWWMrdVA0bk9idjJwMUhuTE83QzR3MUJWb2pCc2VFbU5LTnZXaEt6MDNVeVIzZlBpams2WGg2cwpaSzJITnVNUEVvaTVtRXVTenJVWDNnSzFvSjNndnM5eWRKcTh3aVNyeW12MzdhN3BZRFpJOWtyY2NaSXM4a3lIY0xnQU1IYnFIZz09" http://localhost:8080/iiq/scim/v2/Users?attributes=userName
```


#SCIM - Core Schema

##/ServiceProviderConfig
> **Sample Request**

```cURL
curl 
"http://localhost:8080/iiq/scim/v2/ServiceProviderConfig"
```

> **Sample Response (JSON)**

```
{
"filter": {
"maxResults": 1000,
"supported": true
},
"patch": {
"supported": false
},
"authenticationSchemes": [
{
"documentationUri": "https://community.sailpoint.com/community/identityiq/product-downloads",
"name": "HTTP Basic",
"description": "Authentication Scheme using the Http Basic Standard",
"specUri": "http://www.ietf.org/rfc/rfc2617.txt",
"type": "httpbasic"
}
],
"documentationUri": "https://community.sailpoint.com/community/identityiq/product-downloads",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/ServiceProviderConfig",
"resourceType": "ServiceProviderConfig"
},
"schemas": [
"urn:ietf:params:scim:schemas:core:2.0:ServiceProviderConfig"
],
"etag": {
"supported": true
},
"sort": {
"supported": true
},
"bulk": {
"maxPayloadSize": 0,
"maxOperations": 0,
"supported": false
},
"changePassword": {
"supported": true
}
}
```

The SCIM protocol provides a schema that represents the service provider's configuration.  The service provider configuration gives the developer SCIM specifications and 
additional implementation details in a standardized format.  It is recommended that first time users make a call to /ServiceProviderConfig before using other endpoints.  /ServiceProviderConfig is read-only and does not require any authentication.

### HTTP Request

`GET http://example.com/identityIQ/scim/v2/ServiceProviderConfig`

### Response Format

Parameter | Description | Default Value | Configurable
--------- | ----------- | ------------- | ------------
documentationURI | An HTTP-addressable URL pointing to the service provider's human-consumable help documentation. | https://community.sailpoint.com/community/identityiq/product-downloads | No
patch | A complex type that specifies whether the operation is supported.  'supported' is a boolean value indicating support for patch. | False | No
bulk | A complete type that specifies bulk configuration options. 'supported' is a boolean value indicating support for bulk. maxOperations and maxPayloadSize further specify bulk constraints| False | No
filter| A complex type that specifies filter options.  'supported' is a boolean value indicating support for filter.  maxResults is an integer specifying the maximum # of results returned. | True - 1000 Max | Yes
changePassword | A complex type that specifies password change options.  'supported' is a boolean value indicating support for password change. | True | No
sort | A complex type that specifies sort options.  'supported' is a boolean value indicating support for sort. | True | No
etag | A complex type that specifies ETag options.  'supported' is a boolean value indicating support for ETag. | True | No
authenticationSchemes (multi-valued) | A multi-valued complex type that specifies supported authentication scheme properties.The following sub-attributes have been defined: type, name, description, specUri, and documentationUri. | HTTP Basic (OAuth 2.0 Support coming) | No

##/Schemas

> **Sample Request**

```cURL
curl -u "<username>:<password>" 
"http://localhost:8080/iiq/scim/v2/Schemas"
```

> **Sample Response (JSON)**

```
{
"meta": {
"location": "",
"version": "",
"resourceType": "Schema"
},
"schemas": [
"urn:ietf:params:scim:schemas:core:2.0:Schema"
],
"name": "SailPoint User",
"description": "Additional attributes of the SailPoint User",
"attributes": [
{
"uniqueness": "none",
"name": "entitlements",
"description": "extended attribute description",
"mutability": "readOnly",
"type": "complex",
"multiValued": true,
"caseExact": false,
"returned": "default",
"required": false,
"subAttributes": [
{
"uniqueness": "none",
"name": "value",
"description": "The value of the entitlement",
"mutability": "readOnly",
"type": "string",
"multiValued": false,
"caseExact": false,
"returned": "request",
"required": false
}
]
}]
}
```

The /Schemas endpoint specifies defined attributes and characteristics of the core SCIM schema and all subsequent extended schemas.  /Schemas is read-only.

### HTTP Request

`GET http://example.com/identityIQ/scim/v2/Schemas`

### Response Format

Parameter | Description | Default Value
--------- | ----------- | --------
id| Unique URI of the schema| Varies
name | Human-readable name of schema (e.g. "User") | Varies
description | Human-readable description | Varies
attributes | Complex type with many sub-attributes including name, type, subAttributes, multiValued, description, required, and others.  See section 7 of rfc7643 for full details.

##/ResourceTypes

> **Sample Request**

```cURL
curl -u "<user>:<password>" 
"http://localhost:8080/identityiq/scim/v2/ResourceTypes"
```

> **Sample Response (JSON)**

```
{
"totalResults": 1,
"schemas": [
"urn:ietf:params:scim:api:messages:2.0:ListResponse"
],
"Resources": [
{
"schema": "urn:ietf:params:scim:schemas:core:2.0:User",
"endpoint": "/Users",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/ResourceTypes/User",
"resourceType": "ResourceType"
},
"schemas": [
"urn:ietf:params:scim:schemas:core:2.0:ResourceType"
],
"name": "User",
"description": "User Account",
"schemaExtensions": [
{
"schema": "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User",
"required": true
},
{
"schema": "urn:ietf:params:scim:schemas:sailpoint:1.0:User",
"required": true
}
],
"id": "User"
}
]
}
```

The /ResourceType endpoint provides metadata and details for endpoints.  This includes information such as an resource ID, name, description, endpoint, base URI, schemas 
and schema extensions.  /ResourceTypes is read-only.

### HTTP Request

`GET http://example.com/identityIQ/scim/v2/ResourceTypes`

### Response Format

Parameter | Description 
--------- | ----------- 
id | Resource type's server unique id 
name | Name of resource type 
description | Human-readable description of resource type
endpoint | The HTTP-addressable endpoint
schema | The primary/base schema URI. URI MUST match the id associated with the Schema resource.
schemaExtensions | A list of URIs for the resource types extentions.

# Identity (/users)

The Identity endpoint allows implementors full read, write, and delete capabilities to the Identities within IdentityIQ.  The Identity resource has most SCIM parameters and has been extended to support parameters that are specific to IdentityIQ.  The API supports getting one or more identities, updating an identity, setting a password, deleting an identity and many other usecases.  Please continue reading below for additional information.

### Response Format
Parameter | Description | Schema (SCIM or Extended)
--------- | ----------- | ----------------
id | Unique alpha-numeric ID of the identity | SCIM
externalId | An identifier for the resource as defined by the provisioning client | SCIM
userName | Unique identifier for the User typically used by the user to directly authenticate to the service provider | SCIM
name | Supports formatted, familyName (last name), middleName,  honorificPrefix, honorificSuffix, and given (first) name | SCIM
displayName | The user's display name | SCIM
active | Boolean status of identity.  (Active/Inactive) | SCIM
password | The User's clear text password.  This attribute is intended to be used as a means to specify an initial password when creating a new User or to reset an existing User's password. | SCIM
emails | Email address of user | SCIM
entitlements | Entitlements on source system.  Entitlements are not returned by default. | Extended
roles | Role(s) of the user | Extended
capabilities | Users' capabilities.  I.E. System Administrator | Extended
riskScore | Composite risk score of a user | Extended
isManager | Is this user a manager? | Extended
lastRefresh | When was the last time this user was refreshed? | Extended
manager | The user's manager, referencing the 'id' attribute of another User | Extended


## Get All Identities

> **Sample Request**

```cURL
curl -u "<user>:<password>" 
"http://localhost:8080/iiq/scim/v2/Users"
```

> **Sample Response** (in JSON)

```json
{
"totalResults": 2,
"startIndex": 1,
"schemas": [
"urn:ietf:params:scim:api:messages:2.0:ListResponse"
],
"Resources": [
{
"urn:ietf:params:scim:schemas:sailpoint:1.0:User": {
"entitlements": [],
"capabilities": [
"SystemAdministrator"
],
"roles": [],
"isManager": false
},
"emails": [
{
"type": "work",
"value": "spadmin@sailpointdemo.com",
"primary": "true"
}
],
"displayName": "The Administrator",
"meta": {
"created": "2016-01-29T14:43:09.165-06:00",
"location": "http://localhost:8080/iiq/scim/v2/Users/2c908cbf528f1fd001528f200feb00fc",
"lastModified": "2016-02-18T16:00:26.165-06:00",
"version": "W/\"1455832826165\"",
"resourceType": "User"
},
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:User",
"urn:ietf:params:scim:schemas:core:2.0:User",
"urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"
],
"name": {
"formatted": "The Administrator",
"familyName": "Administrator",
"givenName": "The"
},
"active": true,
"id": "2c908cbf528f1fd001528f200feb00fc",
"userName": "spadmin",
"urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
"manager": {}
}
},
{
"urn:ietf:params:scim:schemas:sailpoint:1.0:User": {
"entitlements": [],
"capabilities": [
"SCIMExecutor"
],
"roles": [],
"isManager": false
},
"emails": [
{
"type": "work",
"value": "spadmin@sailpointdemo.com",
"primary": "true"
}
],
}
```

This endpoint retrieves all identities.  As a performance consideration, roles and entitlements are not returned by default and must be requested explicitly.

### HTTP Request

`GET http://localhost:8080/iiq/scim/v2/Users/`


## Get a Single Identity
> **Sample Request**

```cURL
curl -u "<user>:<password>" 
"http://localhost:8080/iiq/scim/v2/Users/<id>"
```

This endpoint retrieves a specific identity, where ID in the request is the ID of the identity.


### HTTP Request

`GET http://localhost:8080/iiq/scim/v2/Users/<ID>`

## Get Identity with Roles & Entitlements
> **Sample Request**

```cURL
curl -u "<user>:<password>" 
"http://localhost:8080/identityiq/scim/v2/Users/andy.dwyerattributes:userName,urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager,urn:ietf:params:scim:schemas:sailpoint:1.0:User:entitlements,urn:ietf:params:scim:schemas:sailpoint:1.0:User:roles"
```

This endpoint retrieves a specific identity and its role and entitlement information.


### HTTP Request

`http://localhost:8080/identityiq/scim/v2/Users/andy.dwyerattributes:userName,urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager,urn:ietf:params:scim:schemas:sailpoint:1.0:User:entitlements,urn:ietf:params:scim:schemas:sailpoint:1.0:User:roles`

## Filter Identities
> **Sample Request**

```cURL
curl -u "<user>:<password>" 
"http://localhost:8080/iiq/scim/v2/Users?filter=urn:ietf:params:scim:schemas:sailpoint:1.0:User:capabilities eq "SCIMExecutor"&sortBy=displayName"
```

This endpoint retrieves identities that meet the filter criteria as specified in the request. 


### HTTP Request

`GET http://localhost:8080/iiq/scim/v2/Users?filter=urn:ietf:params:scim:schemas:sailpoint:1.0:User:capabilities eq "SCIMExecutor"&sortBy=displayName`

##Create an Identity
> **Sample Request**

```cURL
curl -X POST -u "<user>:<password>" -H "Content-Type: application/scim+json"
 -d ' {
"userName": "mouseRat", 
"name": {
"familyName":"Dwyer",
"givenName":"Andy",
"displayName":"Andy Dwyer"},
"active": true,
"password": "xyzzy",
"capabilities": "SystemAdministrator"
} ' "http://localhost:8080/iiq/scim/v2/Users/"
```


This request creates a single, new identity using the parameters passed in the request.

### HTTP Request
`POST http://localhost:8080/iiq/scim/v2/Users/`


##Edit an Identity

> **Sample Request** 

```cURL
curl -X PUT -u "<user>:<password>" 
-d '    {
"urn:ietf:params:scim:schemas:sailpoint:1.0:User": {
},
"emails": [
{
"type": "work",
"value": "spadmin@sailpointdemo.com",
"primary": "true"
}
],
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:User",
"urn:ietf:params:scim:schemas:core:2.0:User",
"urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"
],
"name": {
"formatted": "Andy Dwyer",
"familyName": "Dwyer",
"givenName": "Andrew"
},
"active": true,
"userName": "mouseRat",
"password": "xyzzy"
}  ' "http://localhost:8080/iiq/scim/v2/Users/2c909180534353fe0153574354ea0104"
```

> **Sample Response** (in JSON)

```json
{
"urn:ietf:params:scim:schemas:sailpoint:1.0:User":{
"entitlements":[
],
"capabilities":[
],
"roles":[
],
"isManager":false
},
"emails":[
{
"type":"work",
"value":"spadmin@sailpointdemo.com",
"primary":"true"
}
],
"displayName":"Andrew Dywer",
"meta":{
"created":"2016-03-08T11:25:43.786-06:00",
"location":"http://localhost:8080/iiq/scim/v2/Users/2c909180534353fe0153574354ea0104",
"lastModified":"2016-03-08T12:04:40.111-06:00",
"version":"W/\"1457460280111\"",
"resourceType":"User"
},
"schemas":[
"urn:ietf:params:scim:schemas:sailpoint:1.0:User",
"urn:ietf:params:scim:schemas:core:2.0:User",
"urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"
],
"name":{
"formatted":"Andrew Dywer",
"familyName":"Dywer",
"givenName":"Andrew"
},
"active":true,
"id":"2c909180534353fe0153574354ea0104",
"userName":"mouseRat",
"urn:ietf:params:scim:schemas:extension:enterprise:2.0:User":{
"manager":{
}
}
}
```

This request makes one or more changes on an existing Identity.  This can be used for actions like updating a user's name or email address, or changing a password.  In the example on the right, the first name of user "Andy Dwyer" is changed from "Andy" to "Andrew".
### HTTP Request
`PUT http://localhost:8080/iiq/scim/v2/Users/<ID>`


##Delete an Identity

> **Sample Request**

```cURL
curl -X DELETE -u "<user>:<password>" 
-H "Content-Type: application/scim+json" 
"http://localhost:8080/iiq/scim/v2/Users/2c909180534353fe0153574354ea0104"
```


This endpoint deletes a single identity.  To delete an identity, the authenticated SCIM user must have DELETE rights, and must pass the identity ID of the identity to be deleted.

### HTTP Request
`DELETE http://localhost:8080/iiq/scim/v2/Users/<ID>`

# Applications (/applications)

The Applications endpoint allows implementors to get information for a single application.  When making a request to the application endpoint, the application ID must be included. 

### Response Format

Parameter | Description
--------- | ----------- 
id | Unique application identifier
schema | Schema definition
applicationSchemas | Type, value, and $ref 
name | Name of the application
features | Supported features of the application
owner | Identity ID of the application owner
type | Application type
meta | Application metadata, such as date created, location, resource type, and version

##Get a Single Application

This request is used to get application details when creating, editing, or deleting an account for an identity.  To get application information using this request, the application id MUST be included.

> **Sample Request**

```cURL
curl -u "<user>:<password>"
"http://localhost:8080/iiq/scim/v2/Applications/<applicationID>"
```

> **Sample Response** (in JSON)

```json
{
"id": "2c9084ee5571ab87015571ac44810319",
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application"
],
"identAttr": {},
"applicationSchemas": [
{
"value": "2c9084ee5571ab87015571ac4482031b",
"$ref": "http://localhost:8080/iiq/scim/v2/Schemas/urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9084ee5571ab87015571ac4482031b",
"type": "account"
}
],
"name": "HR_Employees",
"features": [
"DIRECT_PERMISSIONS",
"NO_RANDOM_ACCESS",
"DISCOVER_SCHEMA"
],
"owner": {
"value": "2c9084ee5571ab87015571ac426d0316",
"$ref": "http://localhost:8080/iiq/scim/v2/Users/2c9084ee5571ab87015571ac426d0316",
"displayName": "HR_Employees App Owners"
},
"type": "Delimited File Parsing Connector",
"meta": {
"lastModified": "2016-06-21T01:42:49.362-05:00",
"created": "2016-06-21T01:36:03.074-05:00",
"location": "http://localhost:8080/iiq/scim/v2/Applications/2c9084ee5571ab87015571ac44810319",
"resourceType": "Application",
"version": "W/\"1466491369362\""
}
}
```

### HTTP Request

`GET http://localhost:8080/iiq/scim/v2/Applications/<applicationID>`

# Accounts (/accounts)

The Accounts resource allows for retrieving, updating, and deleting of accounts on target systems.

Parameter | Description 
--------- | -----------
id | The unique identifier for the Account object associated with IdentityIQ
nativeIdentity | The Account unique identifier associated with the native application
displayName | The name of the Account, suitable for display to end-users
instance | The instance identifier of the Account
uuid | The UUID of the Account
password | The password of the account, used in creating or changing an account password.  Write-only, and never returned
lastRefresh |  The last refresh date of the Account
lastTargetAggregation | The date aggregation was last targeted of the Account
manuallyCorrelated | Flag to indicate this account has been manually correlated in the UI
hasEntitlements | Flag to indicate this account has one or more entitlement attributes
identity | The corresponding User object of the Account
application | The corresponding Application object of the Account
inactive | The status of the account


## Get All Accounts

The request retrieves all accounts for all identities within IdentityIQ.

> **Sample Request**

```cURL
curl -u "<user>:<password>"
"http://localhost:8080/iiq/scim/v2/Accounts"
```

> **Sample Response** (in JSON)

```json
{
"id": "2c9084ee5576d46f015576d4a7620003",
"identity": {
"value": "2c9084ee5576d164015576d271be05f4",
"$ref": "http://localhost:8080/iiq/scim/v2/Users/2c9084ee5576d164015576d271be05f4",
"displayName": "James Smith"
},
"hasEntitlements": false,
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account",
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9084ee5576d164015576d20b60031b"
],
"manuallyCorrelated": false,
"application": {
"value": "2c9084ee5576d164015576d20b5f0319",
"$ref": "http://localhost:8080/iiq/scim/v2/Applications/2c9084ee5576d164015576d20b5f0319",
"displayName": "HR_Employees"
},
"nativeIdentity": "1a",
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9084ee5576d164015576d20b60031b": {
"employeeId": "1a",
"region": "Americas",
"lastName": "Smith",
"email": "James.Smith@demoexample.com",
"location": "Austin",
"department": "Executive Management",
"managerId": "NULL",
"costcenter": [
"R03",
"L07",
"L08",
"L09"
],
"inactiveIdentity": "FALSE",
"fullName": "James.Smith",
"firstName": "James"
},
"lastRefresh": "2016-06-22T01:38:15.917-05:00",
"displayName": "James.Smith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d4a7620003",
"resourceType": "Account",
"version": "W/\"1466577495995\""
}
}
```

### HTTP Request

`GET http://localhost:8080/iiq/scim/v2/Accounts`


## Get a Single Account

This request returns a single account.  To retrieve account information on a single account, simply add the account ID to the end of the request.

> **Sample Request**

```cURL
curl -u "<user>:<password>"
"http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d4a7620003"
```

> **Sample Response** (in JSON)

```json
{
"id": "2c9084ee5576d46f015576d4a7620003",
"identity": {
"value": "2c9084ee5576d164015576d271be05f4",
"$ref": "http://localhost:8080/iiq/scim/v2/Users/2c9084ee5576d164015576d271be05f4",
"displayName": "James Smith"
},
"hasEntitlements": false,
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account",
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9084ee5576d164015576d20b60031b"
],
"manuallyCorrelated": false,
"application": {
"value": "2c9084ee5576d164015576d20b5f0319",
"$ref": "http://localhost:8080/iiq/scim/v2/Applications/2c9084ee5576d164015576d20b5f0319",
"displayName": "HR_Employees"
},
"nativeIdentity": "1a",
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9084ee5576d164015576d20b60031b": {
"employeeId": "1a",
"region": "Americas",
"lastName": "Smith",
"email": "James.Smith@demoexample.com",
"location": "Austin",
"department": "Executive Management",
"managerId": "NULL",
"costcenter": [
"R03",
"L07",
"L08",
"L09"
],
"inactiveIdentity": "FALSE",
"fullName": "James.Smith",
"firstName": "James"
},
"lastRefresh": "2016-06-22T01:38:15.917-05:00",
"displayName": "James.Smith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d4a7620003",
"resourceType": "Account",
"version": "W/\"1466577495995\""
}
}
```

### HTTP Request

`GET http://localhost:8080/iiq/scim/v2/Accounts/accountID`

## Filter Accounts

This request retrieves accounts that meet the filter criteria.  The following fields are filterable or searchable:  displayName, lastRefresh, nativeIdentity, uuid, lastTargetAgg, identity, and application.  Search on application schema specific attributes is not supported.

> **Sample Request**

```cURL
curl -u "<user>:<password>"
"http://localhost:8080/iiq/scim/v2/Accounts?attributes=displayName&filter=(displayName co "Smith")"
```

> **Sample Response** (in JSON)

```
{
"schemas": [
"urn:ietf:params:scim:api:messages:2.0:ListResponse"
],
"startIndex": 1,
"totalResults": 5,
"Resources": [
{
"id": "2c9084ee5576d46f015576d62cce0fdd",
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account"
],
"nativeIdentity": "CN=James Smith,OU=Austin,OU=Americas,OU=DemoData,DC=test,DC=sailpoint,DC=com",
"displayName": "James Smith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d62cce0fdd",
"resourceType": "Account",
"version": "W/\"1466577595603\""
}
},
{
"id": "2c9084ee5576d46f015576d4a7620003",
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account"
],
"nativeIdentity": "1a",
"displayName": "James.Smith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d4a7620003",
"resourceType": "Account",
"version": "W/\"1466577495995\""
}
},
{
"id": "2c9084ee5576d46f015576d5f31d0e98",
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account"
],
"nativeIdentity": "CN=James Smith,OU=Austin,OU=Americas,OU=DemoData,DC=test,DC=sailpoint,DC=com",
"displayName": "James.Smith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d5f31d0e98",
"resourceType": "Account",
"version": "W/\"1466577648646\""
}
},
{
"id": "2c9084ee5576d46f015576d6689a1103",
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account"
],
"nativeIdentity": "1a",
"displayName": "James.Smith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d6689a1103",
"resourceType": "Account",
"version": "W/\"1466577648646\""
}
},
{
"id": "2c9084ee5576d46f015576d525d408ad",
"schemas": [
"urn:ietf:params:scim:schemas:sailpoint:1.0:Account"
],
"nativeIdentity": "100",
"displayName": "JamesSmith",
"meta": {
"location": "http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee5576d46f015576d525d408ad",
"resourceType": "Account",
"version": "W/\"1466577648646\""
}
}
]
}
```
### HTTP Request
`http://localhost:8080/iiq/scim/v2/Accounts?attributes=displayName&filter=(displayName co "Smith")`

## Create an Account

This request is a basic request that creates an Active Directory account.  Account creation depends greatly on the application schema, so requests must be modified accordingly.

> ** Sample Request**

```
curl -X POST -u "<user>:<password>" -H "Content-Type: application/scim+json"
{  
  "identity": {
    "value": "2c9091cb5512cd85015512d0071f001f"
  },
  "application": {
    "value": "2c9091cb5512cd85015512ce25150004",
  },
  "nativeIdentity": "CN=James3 Smith3,OU=OrganzationalGroup2,OU=OrganzationalGroup1,OU=JamesSmith,DC=test,DC=sailpoint,DC=com",
  "displayName": "James.Smith",
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9091cb5512cd85015512ce25240006": {
    "sn": "Smith3",
    "cn": "James3 Smith3",
    "department": "Accounting",
    "sAMAccountName": "James.Smith",
    "givenName": "James3",
    "distinguishedName": "CN=James3 Smith3,OU=OrganzationalGroup2,OU=OrganzationalGroup1,OU=duketest,DC=test,DC=sailpoint,DC=com"
  }
}
 "http://localhost:8080/ideiq/scim/v2/Accounts"
```
### HTTP Request
`POST http://localhost:8080/iiq/scim/v2/Accounts`


## Edit an Account

This request is a basic request that edits an Active Directory account.  In this example, the identity's account name is being changed from "James.Smith" to "James.Smith.New"  Account creation depends greatly on the application schema, so requests must be modified accordingly.

> ** Sample Request**

```
curl -X POST -u "<user>:<password>" -H "Content-Type: application/scim+json"
{  
  "identity": {
    "value": "2c9091cb5512cd85015512d0071f001f"
  },
  "application": {
    "value": "2c9091cb5512cd85015512ce25150004",
  },
  "nativeIdentity": "CN=James3 Smith3,OU=OrganzationalGroup2,OU=OrganzationalGroup1,OU=JamesSmith,DC=test,DC=sailpoint,DC=com",
  "displayName": "James.Smith",
"urn:ietf:params:scim:schemas:sailpoint:1.0:Application:Schema:2c9091cb5512cd85015512ce25240006": {
    "sn": "Smith3",
    "cn": "James3 Smith3",
    "department": "Accounting",
    "sAMAccountName": "James.Smith",
    "givenName": "James3",
    "lastName": "Smith3",
    "distinguishedName": "CN=James3 Smith3,OU=OrganzationalGroup2,OU=OrganzationalGroup1,OU=duketest,DC=test,DC=sailpoint,DC=com"
  }
}
"http://localhost:8080/idiq/scim/v2/Accounts"
```
### HTTP Request
`POST http://localhost:8080/iiq/scim/v2/Accounts/<ID>`

## Enable/ Disable Account

This request is used to enable or disable an account.  In this example, Adam Kennedy's account status is changed from active to inactive, effectively disabling his account.

## Change Account Password

Coming soon!

## Delete Account

This request is used to delete a valid account on a target application for a given identity.  In this example, Adam Kennedy's Active Directory account is deleted, preventing Adam from accessing the application in the future.

> ** Sample Request**

```
curl -X DELETE - u "<user>:<password>" -H "Content-Type: application/scim_json
"http://localhost:8080/iiq/scim/v2/Accounts/2c9084ee56c0905f0156c090b4d800b8"
```
### HTTP Request
`DELETE http://localhost:8080/iiq/scim/v2/Accounts/<ID>`










