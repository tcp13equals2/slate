# API - Events

## Introduction

```json
{
  "id": 6234813,
  "name": "My big event",
  "ts": 1.4538996E12,
  "_ts-dst": true,
  "_ts-display": "28/01/16",
  "_ts-tz": "Australia/Sydney",
  "media": "http://someyoutubelink.com",
  "bannerUrl": "https://s3.com/company/6234779/2015/08/27/tplogo.jpeg",
  "descr": "This is a massive 100km race",
  "isStage": false,
  "isSeries": false,
  "company": {
    "id": 6234779.0,
    "name": "ACME"
  },
  "courses": [
    {
      "id": 6234814,
      "eventId": 6234813,
      "eventName": "My big event",
      "venue": {
        "id": 6234811,
        "name": "Majura Pines - New",
        "address": "Federal Hyw",
        "url": "http://www.majurapines.act.gov.au",
        "lat": -35.239188,
        "lon": 149.193666,
        "country": "Australia",
        "state": "Australian Capital Territory"
      },
      "name": "My 100km",
      "type": "xc",
      "_type-display": "XC",
      "media": "http://someyoutubelink.com",
      "ts": 1.453932E12,
      "_ts-dst": true,
      "_ts-display": "28/01/16 9:00 AM",
      "_ts-tz": "Australia/Sydney",
      "distance": 10000,
      "_distance-display": "10.0 km",
      "duration": 14400,
      "_duration-display": "4h",
      "categories": [
        "two",
        "one"
      ]
    }
  ],
  "planBuilderAvailable": false
}
```

The event record is used for a adding races and events to the system.

Users of the system can add these event's to their calendar, and in most cases they can also build a training plan from the event.

The basic model for the event is that the event has 1 or more courses. 

If the event is a stage race, then each day would have a course entry.

If the event is a series races, then each race in the series would have a course entry.

For single day races, multiple courses are possible - ie there is a 50km and 100km option.

<aside class="notice">
The ability to add, edit and remove events is limited to those who have a *company* role, or *power* role.
</aside>

The following sequence should be used for creating events;

* Create the event promotor company (if it does not yet exist in the system)
* Create the event, associating it with the company
* Create the venues where each course / stage will occur (if they do not yet exist in the system)
* Create the courses for the event, associating the course with the event and the venue


## Add event

> Add event

````json
{
  "id": 6234813,
  "name": "Solo 24hr Nationals",
  "ts": 1440652950559,
  "media": "http://someyoutubelink.com",
  "descr": "This is my event description which could be in HTML markup",
  "isStage": false,
  "isSeries": false,
  "company": {
    "id": 6234779
  }
}
````

`POST https://whats.todaysplan.com.au/rest/events/add`

`Content Type: application/json`

The following fields can be set on an *event* record;

Field | Required | Type | Description
--------- | ------- | ----------- | -------
name        | yes  | String     | This is the name of the event
ts          | yes  | timestamp  | Timestamp for when this event starts. If this is a stage race or series, this is the date of the first stage/round
media       | no   | String      | A external media URL - ie a youtube link
descr       | yes  | String      | A description of the event. This can be in HTML markup
isStage     | no   | Boolean     | If true, this event will be treated as a Stage race
isSeries    | no   | Boolean     | If true, this event will be treated as a multirace series
url         | no   | String      | The event's website URL
company.id  | yes  | Long        | The company that this event is associated with

## Add venue

> Add venue

````json
{
  "name": "Majura Pines - New",
  "address": "Federal Hyw",
  "url": "http://www.majurapines.act.gov.au",
  "lat": -35.239188,
  "lon": 149.193666,
  "country": "Australia",
   "state": "Australian Capital Territory"
}
````

`POST https://whats.todaysplan.com.au/rest/venues/add`

`Content Type: application/json`

The following fields can be set on a course's *venue*;

Field | Required | Type | Description
--------- | ------- | ----------- | -------
name        | yes  | String     | This is the name of the venue - ie Stromlo Forest Park
address     | no   | String     | A street address of the venue
url         | no   | String     | A URL relevant to this venue
lat         | no   | Double     | The latitude of the event venue
lon         | no   | Double     | The longitude of the event venue
country     | no   | String     | The country where the venue is
state       | no   | String     | The state or county where the venue is


## Add course

A course can be added to an event once you have created an event and you have a venue for your course.

> Add course

````json
{
  "event": {
    "id": 6234813
  },
  "course": {
    "venue": {
      "id": 6234811
    },
    "type": "xc",
    "name": "My 100km",
    "descr": "This is a description of my course which could be in HTML markup",
    "distance": 10000,
    "ascent": 2500,
    "descent": 2500,
    "media": "http://someyoutubelink.com"
  },
  "ts": 1453932000000,
  "categories": [
    "two",
    "one"
  ]
}
````

`POST https://whats.todaysplan.com.au/rest/eventcourses/add`

`Content Type: application/json`

The following fields can be set on a *course*'s associated with the event;

Field | Required | Type | Description
--------- | ------- | ----------- | -------
name        | yes  | String     | This is the name of the course - ie 100km course
type        | yes  | EventType  | This is the type of event. ie mountain bike, road, enduro etc
media       | no   | String     | A external media URL - ie a youtube link
ts          | yes  | timestamp  | Timestamp of when this race starts. ie 9am race start for the 100km
distance    | no   | Integer    | Distance in meters (ie 100km race)
duration    | no   | Integer    | Duration in seconds (ie 8hr enduro)
categories  | yes  | EventCategory | A collection of categories which are applicable to the event. ie solo, two, three etc team option
venue.id    | yes  | Long        | The venue that this course is associated with
event.id    | yes  | Long        | The event that this course is associated with


## Edit event

> Edit event

````json
{
  "name": "A new name and time for the event",
  "ts": 1440652950559
}
````

`POST https://whats.todaysplan.com.au/rest/events/set/<event.id>`

`Content Type: application/json`


## Edit venue

> Edit venue

````json
{
  "name": "A new name for my venue"
}
````

`POST https://whats.todaysplan.com.au/rest/venues/set/<venue.id>`

`Content Type: application/json`

## Edit course

> Edit course

````json
{
  "name": "A new name for my course"
}
````

`POST https://whats.todaysplan.com.au/rest/eventcourses/set/<course.id>`

`Content Type: application/json`

## Get event by id

`GET https://whats.todaysplan.com.au/rest/events/get/<event.id>`

`GET https://whats.todaysplan.com.au/rest/events/get/detailed/<event.id>`

## Get venue by id

`GET https://whats.todaysplan.com.au/rest/venues/get/<venue.id>`

`GET https://whats.todaysplan.com.au/rest/venues/get/detailed/<venue.id>`

## Get course by id

`GET https://whats.todaysplan.com.au/rest/eventcourses/get/<course.id>`

`GET https://whats.todaysplan.com.au/rest/eventcourses/get/detailed/<course.id>`


## Find event ##

> Find event

````json
{
  "name": "Solo 24hr Nationals",
   "fromTs": 1440652950559
}
````

> Event search response

````json
{
  "cnt": 1,
  "offset": 0,
  "results": [
    {
      "id": 6234813,
      "cls": "event",
      "name": "Solo 24hr Nationals"
    }
  ]
}
````

`POST https://whats.todaysplan.com.au/rest/events/search/<offset>/<count>`

`Content Type: application/json`

*Search paramaters*

Field | Type | Description
--------- | ----------- | -------
name    | String    | Find all events who have a name which contains this name. ie like %name%
events  | EventType | A collection of event types to search for. ie find all road and mtb events
within  | Integer   | Restrict search results to events which X km of me
fromTs  | timestamp | Restrict search to events which start after this date
toTs    | timestamp | Restrict search to events which start before this date
companyIds  | Long  | Restrict search to events related to the given company id’s (collection of ids


As per venue table above

*URL paramaters*

Field  | Description
------ | -------
offset | The pagination offset to start returning results from
count  | The number of results to return from the offset onwards

To get the next set of paginated results use;

`GET https://whats.todaysplan.com.au/rest/events/next/<offset>/<count>`

## Find venue ##

> Find venue

````json
{
  "name": "Majura Pines - New"
}
````

> Venue search response

````json
{
  "cnt": 1,
  "offset": 0,
  "results": [
    {
      "id": 6234811,
      "cls": "venue",
      "name": "Majura Pines - New",
      "address": "Federal Hyw",
      "url": "http://www.majurapines.act.gov.au",
      "lat": -35.239188,
      "lon": 149.193666,
      "country": "Australia",
      "state": "Australian Capital Territory"
    }
  ]
}
````

`POST https://whats.todaysplan.com.au/rest/venues/search/<offset>/<count>`

`Content Type: application/json`

*Search paramaters*

As per venue table above

*URL paramaters*

Field  | Description
------ | -------
offset | The pagination offset to start returning results from
count  | The number of results to return from the offset onwards

To get the next set of paginated results use;

`GET https://whats.todaysplan.com.au/rest/venues/next/<offset>/<count>`

## Upload event image

`POST https://whats.todaysplan.com.au/rest/events/upload/<event.id>`

`Content Type: application/x-www-form-urlencoded`

*Multipart Form Parameters*

Field | Example | Description
--------- | ------- | -----------
attachment |  | The file content / input stream
filename | logo.jpg | The filename. This will be exposed in the URL provided to access this image

## Upload course route

`POST https://whats.todaysplan.com.au/rest/eventcourses/upload/<course.id>`

`Content Type: application/x-www-form-urlencoded`

*Multipart Form Parameters*

Field | Example | Description
--------- | ------- | -----------
attachment |  | The file content / input stream
filename | route.gpx | The filename. This will be exposed in the URL provided to access this image













