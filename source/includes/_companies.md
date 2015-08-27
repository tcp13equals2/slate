# API - Companies

## Introduction

```json
{
  "id": 6234838,
  "name": "ACME Promotor",
  "url": "http://www.acmeco.com.au",
  "bannerUrl": "https://s3.com/company/6234838/2015/08/27/tplogo.jpeg",
  "isPromotor": true,
  "isTeam": false,
  "isManufacturer": false,
  "permissions": [
    "edit1",
    "edit2",
    "edit3"
  ],
  "isPrimary": false
}
```

The company record is used for a number of purposes within Today's Plan. Including;

* events - Events are associated with an event promotor company
* coaching - Coaches can have a company, allowing for associate coaches and custom branding

<aside class="notice">
The ability to add, edit and remove companies is limited to those who have a *coach* role, or *company* role.
</aside>

The following fields can be viewed on a company record;

Field | Description
------| ------- 
permissions | This is a collection of permissions that the current has has on the company. The permission set can be controlled via the CompanyUser object
bannerUrl | This will be a URL where the company's banner image can be accessed. 


## Add company

> Request

```json
{
  "name": "ACME Promotor",
  "url": "http://www.acmeco.com.au",
  "descr": "This is a sample event promotor company",
  "isCoach": false,
  "isPromotor": true,
  "isPrimary": false
}

```

> Response

```json
{
  "id": 6234838,
  "name": "ACME Promotor",
  "url": "http://www.acmeco.com.au",
  "isPromotor": true,
  "isTeam": false,
  "isManufacturer": false,
  "permissions": [
    "edit1",
    "edit2",
    "edit3"
  ],
  "isPrimary": false
}
```

`POST https://whats.todaysplan.com.au/rest/companies/add`

`Content Type: application/json`

The following fields can be set on a *company* record;

Field | Required | Type | Description
--------- | ------- | ----------- | -------
name        | yes | String  | This is the name of the company
url         | no  | String  | A URL for the company
descr       | no  | String  | A description which describes the company
isCoach     | no  | Boolean | A flag indicating if this company is a coaching company (default = false)
isPromotor  | no  | Boolean | A flag indicating if this company is an event promotor company (default = false)
isPrimary   | no  | Boolean | A flag indicating if this company is the user's primary company (default = true if this is the first company being added to the user's account)


## Edit company

> Request

```json
{
  "name": "ACME Promotor v2"
}
```

`POST https://whats.todaysplan.com.au/rest/companies/set/<company.id>`

`Content Type: application/json`


## Get company by id

> Response

```json
{
  "id": 6234838,
  "name": "ACME Promotor",
  "url": "http://www.acmeco.com.au",
  "bannerUrl": "https://s3.com/company/6234838/2015/08/27/tplogo.jpeg",
  "isPromotor": true,
  "isTeam": false,
  "isManufacturer": false,
  "permissions": [
    "edit1",
    "edit2",
    "edit3"
  ],
  "isPrimary": false
}
```
`GET https://whats.todaysplan.com.au/rest/companies/get/<company.id>`

## Find company 

> Request

```json
{
  "name": "ACME Promotor"
}
```

> Response

```json
{
  "cnt": 1,
  "offset": 0.0,
  "results": [
    {
      "id": 6234838,
      "name": "ACME Promotor"
    }
  ]
}
````

`POST https://whats.todaysplan.com.au/rest/companies/search/<offset>/<count>`

`Content Type: application/json`

*Search paramaters*

Field | Type | Description
--------- | ------- | ----------- 
name        | String  | Find all companies who have a name which contains this name. ie like %name%
isCoach     | Boolean | Only search for companies which have isCoach set to true or false
isPromotor  | Boolean | Only search for companies which have isPromotor set to true or false

*URL paramaters*

Field  | Description
------ | -------
offset | The pagination offset to start returning results from
count  | The number of results to return from the offset onwards

To get the next set of paginated results use;

`GET https://whats.todaysplan.com.au/rest/companies/next/<offset>/<count>`

## Upload company logo

`POST https://whats.todaysplan.com.au/rest/companies/upload/<company.id>`

`Content Type: application/x-www-form-urlencoded`

*Multipart Form Parameters*

Field | Example | Description
--------- | ------- | -----------
attachment |  | The file content / input stream
filename | logo.jpg | The filename. This will be exposed in the URL provided to access this image








