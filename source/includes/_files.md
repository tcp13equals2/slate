# API - File upload

## Upload an activity file

> Request

```json
{
  "filename": "andrew-hall-20150823083057.fit",
  "activityType": "workouts",
  "activityId": 100045,
  "name": "Northside HOP",
  "thresholdWatts": 350,
  "equipment": "mtb",
  "workout": "training",
  "weight": 70.1
}

```

> Response

```json
{
  "id": 6234544,
  "resultId": 6234545.0,
  "filename": "andrew-hall-20150823083057.fit",
  "state": "finished",
  "type": "data",
  "result": {
    "id": 6234545.0,
    "workoutId": 100045,
    "activityId": 6234495.0,
    "activityType": "workouts",
    "url": "https://whats.todaysplan.com.au/calendar/workouts/6234495"
  }
}
```

`POST https://whats.todaysplan.com.au/rest/files/upload`

`Content Type: application/x-www-form-urlencoded`

*Multipart Form Parameters*

Field | Example | Description
--------- | ------- | -----------
attachment |  | The file content / input stream
json | { filename: "20150823083057.fit" } | The associated metadata object - see below

*Metadata Parameters*

Field | Required | Type | Description
--------- | ------- | ----------- | -------
filename | yes | String | The filename
activityType | no | ActivityResultType | Force this file to be associated with a given event or workout
activityId | no | Long | Force this file to be associated with a given event or workout
name | no | String | Alternate name for a ride - ie Northside HOP
thresholdWatts | no | Integer | Alternate threshold power (watts) to be used when processing this file
equipment | no | Equipment | Equipment type used for this ride
workout | no | WorkoutType | Type of ride
weight | no | Double | Rider weight (kg) to be used when processing this ride

The response will contain a status on the processing of this file. Usually, it will return with a pending state, meaning the file has been queued for processing.

If the file has been processed or an attempt is made to upload a duplicate file, then details of the processed file are included - such as the file's id and URL.

The *id* field in this response can be used to check for the processing status of the file. It can also be used to reprocess a file at a later date.

