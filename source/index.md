---
title: Today's Plan API Reference

language_tabs:
  - json

toc_footers:
  - <a href='http://www.todaysplan.com.au'>http://www.todaysplan.com.au</a>

includes:
  - files
  - companies
  - events

search: true
---

# Introduction

Welcome to the Today's Plan API.

You can use this API to upload activity files to Today's Plan, as well as to manage of of your account and data.

Create your own workouts, download activity files, query for historical, summary and aggregate data.

The Today's Plan API is a REST style interface using JSON.

All of the Today's Plan web site and associated tools (Desktop Agent, Android app) are all built on this API. We are slowly bringing the documentation of the full API up to speed. If you would like to perform a certain operation or data query and you can't find it in this API doc, please don't hesitate to contact us and we will get you the info you need.

For support, please email <a href='mailto:support@todaysplan.com.au'>support@todaysplan.com.au</a>

The main API endpoing server is located at https://whats.todaysplan.com.au

A staging/test API server can be made available at https://staging.todaysplan.com.au - please contact us to arrange access time to this endpoint.

# Conventions

All unique object identifiers are signed 64bit numbers.

All dates and times are stored in UTC as ms since epoch values.

All units of measurement are presented in metric (unless otherwise stated).

To assist in dealing with various timezones, locales and units of measurement preferences, all json responses are augmented with localisation data.  These “decorated” values will be in the user’s specific timezone, locale, language etc.

ie in the following json fragment, the due date is a ms timestamp, and the _dueDate-display has been converted into the user’s timezone.

"dueDate":1450676721830,"_dueDate-dst":true,"_dueDate-display":"21/12/15"

The decorated fields are identifiable via the _ prefix and often have a -display in the name.

A variety of other decorated fields exist - such as converting enumerated values into the user’s local language, and in formating values (such as currency).


# Authentication

The Today's Plan API requires either a manual authentication using a username and password, or a pre-negotiated OAuth2 token can be presented.

On login, a JSESSIONID cookie is returned. This should be set in the HTTP client for all subsequent requests.

`Set-Cookie = JSESSIONID=TR4t0yRVTH8esA54fX3-78yi.undefined; Path=/`

When using an OAuth token, the bearer token should be placed into the header of each request. No explicit login call is needed.

`Authorization: Bearer <token>`

The JSESSIONID cookie should still be set after the first request using the OAuth token.

## OAuth tokens

Before an access token can be requested for a user, the 3rd party application who will be using the API needs to be registered with Today’s Plan.

A unique client_id and client secret will be assigned to the application developer.

Depending on the type of OAuth access request type being used, the application will also need to provide an authorization code URI redirect target.

ie when an authorization is requested, and the user accepts the access request, an authorization code is returned to the application. This is returned in a query string appended to the registered URI.

Once an access token has been allocated for a user, this should be placed into the Authorization Bearer header when making HTTP request.

`Authorization: Bearer <token>`

Two grant type modes are supported;

* grant_type = password
* grant_type = code

The *password* grant type allows for a token to be allocated with a single command - whereby both the user’s Today’s Plan credentials and the app’s client id and client secret are presented in a single command.

The *code* grant type is a two phase process, and represents the more traditional oauth token establishment whereby the app does not ever see the user’s credentials.

### Token grant by password

> Response

```json
{
        "access_token" : "2319e97c9bd9578f768b37519edc8e22"
}

```

`POST https://whats.todaysplan.com.au/rest/oauth/access_token`

*Multipart Form Parameters*

Content Type: application/x-www-form-urlencoded

Field | Example | Description
--------- | ------- | -----------
username | andrew@todaysplan.com.au | The user's Today’s Plan email
password | <password> | The user's Today's Plan password
client_id | example | The client_id assigned to the app by Today's Plan
client_secret | abc123 | The client secret assigned to the app by Today's Plan
grant_type | password |
redrect_uri | https://www.example.com | The app's URL

The response to this request will be a json object with an access_token.

The expiry of the token will be determined based on the client_id / app, and if set the expiry date will be included in this response.

### Token grant by authorization code

```json
{
        "access_token" : "2319e97c9bd9578f768b37519edc8e22"
}

```

The code grant type is a two phase process, and represents the more traditional oauth token establishment whereby the app does not ever see the user’s credentials.

The first step is to redirect the user to `https://whats.todaysplan.com.au/authorize/<client_id>`

If required, the user will be required to authenticate to Today’s Plan, and will then be represented with an option accept or deny the app’s access request.

If the user accepts the request, the user will be redirected back to the app’s URI (as registered with Today’s Plan), with the code appended as a query parameter.

ie `https://example.com/authorize?code=2319e97c9bd9578f768b37519edc8e22`

The second step is for the app's site (ie example.com) to extract the code from the query string, and issue a new multipart form POST request to Today's Plan;

*Multipart Form Parameters*

Content Type: application/x-www-form-urlencoded

Parameter | Example | Description
--------- | ------- | -----------
code | 2319e97c9bd9578f768b37519edc8e22 | The code as passed back to the application's website
client_id | example | The client_id assigned to the app by Today's Plan
client_secret | abc123 | The client secret assigned to the app by Today's Plan
grant_type | authorization_code | 
redrect_uri | https://www.example.com | The app's URL

The response to this request will be a json object with an access_token.

The expiry of the token will be determined based on the client_id / app, and if set the expiry date will be included in this response.



# API - Authentication

## Login via username/password

> Request

```json
{
	"username": "andrew@todaysplan.com.au",
	"password": "mypassword",
	"token": true
}
```

> Response

```json
{
  "services": {
    "subscriptions": [],
    "plans": []
  },
  "canswitch": true,
  "token": "f291db13-b96b-46da-856d-ebf1f7229dc5",
  "tokenExp": 1443223617015,
  "dateformat": "d/MM/yy",
  "roles": [
    "user",
    "premium"
  ],
  "user": {
    "id": 28405,
    "email": "andrew@todaysplan.com.au",
    "firstname": "Andrew",
    "lastname": "Hall",
    "locale": "en_AU",
    "timezone": "Australia/Sydney"
  },
  "timeformat": "h:mm a"
}
```

`POST https://whats.todaysplan.com.au/rest/auth/login`

*Request Parameters*

Parameter | Default | Description
--------- | ------- | -----------
username | | Your username - this is the email address you registered to Today's Plan with
password | | Your password
token | false | If set to true, then a bearer token will be returned in the login response. The token expiry will also be returned in the response.

<aside class="success">
Remember — keep your token in a secure location!
</aside>

*Response Parameters*

A variety of information relating to the user is returned. This includes the user profile, some preferences, their current services and plans. Some useful parameters are listed below;

Parameter | Description
--------- | -------
services.subscriptions | Details of current analytic or coaching services
services.plans | Details of current training plans
token | A authorization bearer token (if requested)
tokenExp | The expiry date of the issued token
roles | The roles which this user is a member of. This will control what API calls are available to this user
user.* | Various meta data about the user
user.locale | The user's prefered locale
user.timezone | The user's timezone

## Logout ##

> Response

```json
true
```

`GET https://whats.todaysplan.com.au/rest/auth/logout`

<aside class="notice">
If a token is presented in the header on a logout it will be invalidated.
</aside>
