---
title: API Reference

language_tabs:

- http
- shell

toc_footers:
  - Have Questions? <br><a href='http://github.com/GRESB/api-docs/issues'>Open an Issue on Github</a>

  - or contact <a href='mailto:u.scharf@gresb.com'>u.scharf@gresb.com</a>

  - <a href='http://github.com/tripit/slate' style='position:absolute; bottom:5px;'>Documentation Powered by Slate</a>


includes:
  - errors
  - data-dictionary.md

search: true
---

# Introduction

<aside class="warning">
The GRESB API is in the early stages of development.  All documentation below is subject to change.  If you have any feedback regarding our API or the documentation, we would love to hear it! Feel free to submit your comments or questions [here](https://github.com/GRESB/api-docs/issues)
</aside>

>**Endpoint:**

```
https://api.gresb.com
```

GRESB will provide an Application Programming Interface (API) web service to facilitate the entry of asset level data into the GRESB survey.  

The API complies with [REST](http://en.wikipedia.org/wiki/Representational_state_transfer) protocol.  It uses HTTP response codes to report errors, and responds to the HTTP methods detailed below.  Data is accepted in JSON format and responses are returned in JSON.  The API uses [OAuth 2.0](http://tools.ietf.org/html/rfc6749) for authorization.  It will also provide test-mode access keys to allow users to explore the API without altering data.  

GRESB strives to keep asset level data consistent, whether it is received via the API, the excel upload, or is entered directly.  For this reason, GRESB offers a data dictionary to assist with formatting.  In rare cases, GRESB may restrict access to specific data fields to avoid redundant data.  

The GRESB API uses four basic HTTP verbs; each communicates a unique request

     <table>      <tbody>        <tr>          <th class="methodCell">Methods:</th>        </tr>        <tr>          <th>POST</th>          <td>This method creates an item </td>        </tr>        <tr>          <th>PUT</th>          <td>This method updates an item</td>        </tr>        <tr>          <th class="changelogCell">GET</th>          <td>This method reads an item</td>        </tr>        <tr>          <th class="changelogCell">DELETE</th>          <td>This method deletes an item. </td>        </tr>      </tbody>    </table>
# Authorization

>The key below can be used to access the API test environment. 

The GRESB API uses OAuth 2.0 protocol to securely authorize accounts.  The general OAuth protocol is listed below:

1. Before you client can make a request on a resource owner's behalf, the resource owner must grant your client access.  
2. If your client is granted access, you will receive an authorization code.  
3. The code will then be exchanged with our server for an access token.  This access token must be used each time your client makes a request.  


<aside class="notice">
Detailed information on authorization will be coming soon.
</aside>

HTTP Basic Auth is used for our test environment.  You can access the API test environment at any point using our test key.

 
# Basic Operations

**The API gives you the ability to upload all asset level data in bulk, using JSON.  After uploading, every asset will be assigned an unique ID and can be edited individually 
using this ID.  All commands are based on the HTTP methods outlined by rest protocol.**  

## Parameters


<table>
  <thead>
    <tr>
       <th>Parameter</th>
       <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>response_id</td>
      <td>Integer. Specific to the survey response.  Each company/fund will receive a new response id at the beginning of a survey period </td>
    </tr>
    <tr>
      <td>building_id</td>
      <td>Integer.  The designated identifier for an asset. When updating an asset, this id <u>must</u> be included.</td>
    </tr>
    <tr>
      <td>partner’s_id</td>
      <td>String. Client-side identifier for an asset.  This is ignored by the GRESB database, and is solely intended for your identification.</td>  
    </tr>
      <td>Data Fields: e.g. asset_name, EN_RSM_MAN_TLE_ABS2014</td>
      <td>Integer or String.  Designates data to its correct field.  See the <a href=#data-dictionary>data dictionary</a> for a complete reference.
  </tbody>
</table>


## Account Level

>Request returns a list of companies or funds associated with your account.

```shell
$ curl https://api.gresb.com/api/responses \
-u *auth key*
```


```http
GET /api/responses HTTP/1.1
Host: api.gresb.com
```

>Response


```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "object": "list",
  "has_more": false,
  "data": [
    {
      "country": "Australia",
      "created_at": "2014-06-04T01:17:42Z",
      "response_id": 9987,
      "legal_status": "Non-listed",
      "manager": "Real Estate Group",
      "name": "The Real Estate Residential Fund",
      "property_type": "Residential",
      "region": "Oceania",
      "submitted_at": "2014-06-30T03:58:39Z",
      "survey_date": "2014",
      "updated_at": "2014-08-29T01:52:46Z"
    },
    {
      "country": "Australia",
      "created_at": "2014-06-04T01:17:44Z",
      "id": 9988,
      "legal_status": "Listed",
      "manager": "Real Estate Group",
      "name": "The Real Estate Diversified Fund",
      "property_type": "Diversified - Office/Retail",
      "region": "Oceania",
      "submitted_at": "2014-10-09T20:27:18Z",
      "survey_date": "2014",
      "updated_at": "2014-10-09T20:27:18Z"
    }
  ]
}
	
```


After authentication, you will receive a list of funds/companies associated with your account, each with a survey response ID.  The response IDs apply to the current response period.  You can access your account’s companies/funds and response IDs at any point using the method below.

<table>
  <thead>
    <tr>
       <th>Parameter</th>
    </tr>
  </thead>
  <tbody>
    <tr>
	<td>none</td>
    </tr>
  </tbody>
</table>

<table>
  <thead>
    <tr>
       <th>Attribute</th>
       <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>response_id</td>
      <td>Integer. Specific to the survey response.  </td>
    </tr>
    </tbody>
</table>





## Response Level

###GET
Using the GET method, you can receive a list of all assets uploaded to a response or a list of all asset level data uploaded to a response.  Both requests will return building_id codes.

>This request returns a list of assets uploaded to a survey response

```shell
$ curl https://api.gresb.com/api/responses/2315/asset_level_data/buildings \
-u *auth key*
```

```http
GET  /api/responses/(response_id)/asset_level_data/buildings HTTP/1.1
Host: api.gresb.com

```

>Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "object": "list",
  "has_more": false,
  "data": [
   {
	"building_id":2355745342,
	"partner’s identifier": "asset 1",
	"asset_name": "A Building"
	"asset_address": "12345 Main Street New York, NY, USA",
	"error": "none"
   },
   {
	"building_id": 5756453454,
	"partner's_identifier": "asset 44",
	"asset_name":"Another Building",
	"asset_address": "900 Flat Street, Antarctica",
	"error": "none"
    } 
  ]
}
```


>This request returns all asset level data uploaded to a survey response, organized by asset

```shell
$ curl https://api.gresb.com/api/responses/2315/asset_level_data \
-u *auth key*
```

```http
GET /api/responses/(response_id)/asset_level_data HTTP/1.1
Host: api.gresb.com
```

>Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "object": "list",
  "has_more": false,
  "data": [
   {
	"building_id":2355745342,
	"partner’s identifier": "asset 1",
	"asset_name": "A Building",
	"asset_address": "12345 Main Street New York, NY, USA",
	"EN_DWH_MAN_BCF_ABS2013": 45657,
	"EN_DWH_MAN_BCF_ABS2014": 589708,
	"EN_DWH_MAN_BCF_COV2014": 45435,
	"EN_DWH_MAN_BCF_TOT2014": 96783,
	"EN_DWH_MAN_BCD_ABS2013": 678865
   },
   {
	"building_id": 5756453454,
	"partner's_identifier": "asset 44",
	"asset_name": "Another Building",
	"asset_address": "900 Flat Street, Antarctica",
	"EN_DWH_MAN_BCF_ABS2013": 35342,
	"EN_DWH_MAN_BCF_ABS2014": 978675,
	"EN_DWH_MAN_BCF_COV2014": 12376,
	"EN_DWH_MAN_BCF_TOT2014": 45444,
	"EN_DWH_MAN_BCD_ABS2013": 15678,
    } 
  ]
}
```

<table>
  <thead>
    <tr>
       <th>Parameter</th>
	<th>Description</th> 
     </tr>
 </thead>
 <tbody>
    <tr>
      <td>response_id</td>
      <td>Integer. Specific to the survey response.  </td>
    </tr>
    </tbody>
</table>


>This request will update or add asset level data to a response.  When updating an asset, be sure to use the correct building_id

----

###PUT & POST

The PUT method is used to update, or add to a survey responses asset data.  It is important to avoid uploading duplicate assets when using this method.  The best way to avoid this is to ensure that each asset is assigned the correct building id code when updating.  The POST method will replace all previous data uploaded by your account with the current upload.  This will create new building ids for all assets.  


```shell
$ curl https://api.gresb.com/api/responses/2315/asset_level_data \
-u *auth key*: \ 
-X PUT \
-H "Content-Type: application/json" \
-d [ 
      {
	"building_id": 5756453454,
	"partner's_identifier": "asset 44",
	"asset_name": "Another Building",
	"asset_address": "900 Flat Street, Antarctica",
	"EN_DWH_MAN_BCF_ABS2013": 35342,
	"EN_DWH_MAN_BCF_ABS2014": 978675,
	"EN_DWH_MAN_BCF_COV2014": 12376,
	"EN_DWH_MAN_BCF_TOT2014": 45444,
	"EN_DWH_MAN_BCD_ABS2013": 15678,
       },
       {
	"building_id":2355745342,
	"partner’s identifier": "asset 1",
	"asset_name": "A Building",
	"asset_address": "12345 Main Street New York, NY, USA",
	"EN_DWH_MAN_BCF_ABS2013": 45657,
	"EN_DWH_MAN_BCF_ABS2014": 589708,
	"EN_DWH_MAN_BCF_COV2014": 45435,
	"EN_DWH_MAN_BCF_TOT2014": 96783,
	"EN_DWH_MAN_BCD_ABS2013": 678865,
       }
]


```

```http
PUT /api/responses/(response_id)/asset_level_data HTTP/1.1
Host: api.gresb.com
Content-Type: application/json
Accept: application/json
Authorization: *example key*
[ 
	{
	"building_id": 5756453454,
	"partner's_identifier": "asset 44",
	"asset_name": "Another Building",
	"asset_address": "900 Flat Street, Antarctica",
	"EN_DWH_MAN_BCF_ABS2013": 35342,
	"EN_DWH_MAN_BCF_ABS2014": 978675,
	"EN_DWH_MAN_BCF_COV2014": 12376,
	"EN_DWH_MAN_BCF_TOT2014": 45444,
	"EN_DWH_MAN_BCD_ABS2013": 15678,
	},
	{
	"building_id":2355745342,
	"partner’s identifier": "asset 1",
	"asset_name": "A Building",
	"asset_address": "12345 Main Street New York, NY, USA",
	"EN_DWH_MAN_BCF_ABS2013": 45657,
	"EN_DWH_MAN_BCF_ABS2014": 589708,
	"EN_DWH_MAN_BCF_COV2014": 45435,
	"EN_DWH_MAN_BCF_TOT2014": 96783,
	"EN_DWH_MAN_BCD_ABS2013": 678865,
	}
]
```

**PUT**

<table>
  <thead>
    <tr>
       <th>Parameter</th>
       <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>response_id</td>
      <td>Integer. Specific to the survey response. </td>
    </tr>
    <tr>
      <td>building_id</td>
      <td>Integer. When updating an asset, this id <u>must</u> be included.</td>
    </tr>
    <tr>
      <td>partner’s_id</td>
      <td>String. Client-side identifier for an asset.</td>  
    </tr>
      <td>Data Fields: e.g. asset_name, EN_RSM_MAN_TLE_ABS2014</td>
      <td>Integer or String.  Designates data to its correct field.  See the <a href=#data-dictionary>data dictionary</a> for a complete reference.
  </tbody>
</table>

>This request will override all previous data with the JSON upload. Each asset in this request will receive a unique building_id


```shell
$ curl https://api.gresb.com/api/responses/2315/asset_level_data \
-u *auth key*: \ 
-X POST \
-H "Content-Type: application/json" \
-d [
      {
	"partner's_identifier": "asset 44",
	"asset_name": "Another Building",
	"asset_address": "900 Flat Street, Antarctica",
	"EN_DWH_MAN_BCF_ABS2013": 35342,
	"EN_DWH_MAN_BCF_ABS2014": 978675,
	"EN_DWH_MAN_BCF_COV2014": 12376,
	"EN_DWH_MAN_BCF_TOT2014": 45444,
	"EN_DWH_MAN_BCD_ABS2013": 15678,
       },
       {
	"partner’s identifier": "asset 1",
	"asset_name": "A Building",
	"asset_address": "12345 Main Street New York, NY, USA",
	"EN_DWH_MAN_BCF_ABS2013": 45657,
	"EN_DWH_MAN_BCF_ABS2014": 589708,
	"EN_DWH_MAN_BCF_COV2014": 45435,
	"EN_DWH_MAN_BCF_TOT2014": 96783,
	"EN_DWH_MAN_BCD_ABS2013": 678865,
       }
]
```


```http
POST /api/responses/(response_id)/asset_level_data HTTP/1.1
Host: api.gresb.com
Content-Type: application/json
Accept: application/json
Authorization: *example key*
[
	{
	"partner's_identifier": "asset 44",
	"asset_name": "Another Building",
	"asset_address": "900 Flat Street, Antarctica",
	"EN_DWH_MAN_BCF_ABS2013": 35342,
	"EN_DWH_MAN_BCF_ABS2014": 978675,
	"EN_DWH_MAN_BCF_COV2014": 12376,
	"EN_DWH_MAN_BCF_TOT2014": 45444,
	"EN_DWH_MAN_BCD_ABS2013": 15678,
	},
	{
	"partner’s identifier": "asset 1",
	"asset_name": "A Building",
	"asset_address": "12345 Main Street New York, NY, USA",
	"EN_DWH_MAN_BCF_ABS2013": 45657,
	"EN_DWH_MAN_BCF_ABS2014": 589708,
	"EN_DWH_MAN_BCF_COV2014": 45435,
	"EN_DWH_MAN_BCF_TOT2014": 96783,
	"EN_DWH_MAN_BCD_ABS2013": 678865,
	}
]
```

**POST**

<table>
  <thead>
    <tr>
       <th>Parameter</th>
       <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>response_id</td>
      <td>Integer. Specific to the survey response. </td>
    </tr>
     <tr>
      <td>partner’s_id</td>
      <td>String. Client-side identifier for an asset.</td>  
    </tr>
      <td>Data Fields: e.g. asset_name, EN_RSM_MAN_TLE_ABS2014</td>
      <td>Integer or String.  Designates data to its correct field.  See the <a href=#data-dictionary>data dictionary</a> for a complete reference.
  </tbody>
</table>



----

###Status

At any point, you can check the status of your response.



>This request returns the status of your response on the asset level


```shell
$curl https://api.gresb.com/api/responses/2315/asset_level_data/status \
-u *auth key*
```

```http
GET /responses/(response_id)/asset_level_data/status HTTP/1.1
Host: api.gresb.com
```
>Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "object": "list",
  "has_more": false,
  "error": [
   {
	"building_id":2355745342,
	"partner’s identifier": "asset 1",
	"EN_[pt]_MAN_BCD_TOT2014": "a3433543",
	"message": "value must be a positive integer"
   },
   {
	"building_id": 5756453454,
	"partner's_identifier": "asset 44",
	"asset_name":"",
	"message": "mandatory field, must be a string",
	"asset_address":"",
	"message": "mandatory field, must be a string"
    } 
  ]
}
```

<table>
  <thead>
    <tr>
       <th>Parameter</th>
       <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>response_id</td>
      <td>Integer. Specific to the survey response. </td>
    </tr>
   </tbody>
</table>

## Asset Level



# Versioning
Your api version will be set the first time you use the GRESB API.  All updates to our API will be documented in a changelog, and you will have the ability to upgrade your API to the newest version at any point.  If a backwards-incompatible update is released, all API users will be notified in advanced and be given ample time to upgrade.  



<table>
  <thead>
    <tr>
      <th>Version</th>
      <th>Changes</th>
      <th>Date Released</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0.1</td>
      <td></td>
      <td>Coming Soon!</td
    </tr>
  </tbody>
</table>

