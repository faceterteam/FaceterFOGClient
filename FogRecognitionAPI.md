# FOG Recognition API

## How to connect to CallbackAPI

First, you should set an URLs of your backend where callbacks request will be sent. This action is available on Sources section, you should choise your video source then setup two URL: 
1. Person meta info callback URL
2. Person visit callback URL

(images/SourceURLs.png)

## Event types

### `PersonChanged` - will be sent to the Person meta info callback URL.

```json
{
  "personId": "c15290a1-3e2e-457d-8d2f-21bd157615d0" (uuid),
  "sex": "male" (string | [male, female]),
  "age": 42 (int),
  "cameraId": "8c62b232-5cde-4d48-8a2c-67322671d12f" (uuid)
}
```

#### Fields description

| Title |  Description |
| ------ | ------ |
|  **PersonId**  | Unique person's identifier  |
|  **Sex**  | Person's gender (Male, Female) |
|  **Age**  | Person's age |
|  **CameraId**  | Video source identifier |

### `RecognitionEvent` - will be sent Person visit callback URL.

```json
{
  "cameraId": "afe78665-4398-4c5e-b2fa-31bf19ef1b4e" (uuid),
  "id": "4aec96fd-acdf-4f05-a904-87cc11edf9a4" (uuid),
  "projectId": "21b224ae-2759-434b-be96-1f82146b0a0c" (uuid),
  "taskId": "0fd6e9d5-8b51-498c-ad4a-615c372410b6" (uuid),
  "trackletId": "245bc02b-0ad6-445b-bea9-5f8125bdcb44" (uuid),
  "personId": "ac66866c-cb65-4a5a-be62-3fe3125e88d3" (uuid),
  "nearestPersons": ["eb849ce0-2e08-4042-bce1-ee70afec3413", "c38b06f8-9cfa-46cf-a928-0a8bb3000aee, "263d523a-4d4f-4ae0-8ecd-cbfc0e552f38" (uuid)],
  "isPersonified": true (bool),
  "avatarHash": -5467464835834843 (int, null),
  "tracks": [
      {
         "bbox":[
            {
               "x":1585 (int),
               "y":267 (int)
            },
            {
               "x":1641,
               "y":323
            }
         ],
         "timestamp":1565955460073 (int),
         "pts":7335336386.0 (double)
      },
      {
         "bbox":[
            {
               "x":1449,
               "y":327
            },
            {
               "x":1518,
               "y":396
            }
         ],
         "timestamp":1565955460113,
         "pts":7335339986.0
      },
      {
         "bbox":[
            {
               "x":1454,
               "y":335
            },
            {
               "x":1523,
               "y":403
            }
         ],
         "timestamp":1565955460193,
         "pts":7335347186.0
      }
   ],
  "fromTs": 1565955458713 (int),
  "toTs": 1565955460273 (int),
  "bestFaceScore": 0.7 (double | [0...1])
}
```

#### Fields description

| Title |  Description |
| ------ | ------ |
|  **CameraId**  | Video source identifier |
|  **Id**  | Event identifier |
|  **ProjectId**  | Project identifier |
|  **TaskId**  | Video stream identifier |
|  **TrackletId**  | Person's track identifier between video frames |
|  **PersonId**  | Person's identifier. Person will be created based on face tracklets |
|  **NearestPersons**  | List of similar person's identifiers |
|  **IsPersonified**  | Flag, shows the state of tracklet identification |
|  **AvatarHash**  | Hash of the best face image  |
|  **Tracks**  | List of the person detection events. Field contains: |
| | **Bbox** - List of the face coordinates (x,y pixels) on video frame. |
| | **Timestamp** - time of detection event |
| | **Pts** - Presentation Time Stamp of frame |
|  **FromTs**  | Event start time |
|  **ToTs**  | Event end time |
|  **BestFaceScore**  | Face image quality score (from 0 to 1) |

### Faceter Fog integration

#### RecognitionEvent processing

* When you need to get best face image (avatar) you're backend should return success response with HTTP code 200 (OK):

```json
	"avatarCallbackUrl": "http://localhost/avatar"
```

| Title |  Description |
| ------ | ------ |
|  AvatarCallbackUrl  | URL which will listen HTTP POST method of best face image |

The best face image will be sent to `avatarCallbackUrl` with query-parameters `avatarHash`, `personId`, `projectId`, which contains in `RecognitionEvent`.
You're backend shoukd return HTTP code 204 (NoContent).

* If you not needed in best face image just shoukd just return HTTP code 204 (NoContent).

(images/RecognitionEvent.png)

#### PersonChanged processing

* You're backend shoukd return HTTP code 204 (NoContent).

(images/PersonChanged.png)
