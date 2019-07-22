# API Documentation

- [Basics](#basics)
    - [Response Codes](#response-codes)
    - [Endpoints](#endpoints)
        - [Base Endpoint](#base-endpoint)
        - [Endpoint Params](#endpoint-params)
        - [Example Endpoint](#example-endpoint)
- [Making Requests](#making-requests)
    - [Retrieving Agents](#retrieving-agents)
        - [Request](#request)
        - [Request Params](#request-params)
        - [Response Body](#response-body)
        - [Example Request](#example-request)
        - [Example Response](#example-response)
    - [Detecting Intents](#detecting-intents)
        - [Request](#request-1)
        - [Request Params](#request-params-1)
        - [Request Body](#request-body)
        - [Response Body](#response-body-1)
        - [Example Request](#example-request-1)
        - [Example Response](#example-response-1)

## Basics

### Response Codes

| HTTP-Code | Reason                                                                 |
|-----------|------------------------------------------------------------------------|
| 200       | Request was successful                                                 |
| 400       | Request Body or Session ID is invalid and/or missing                   |
| 403       | Integration was blocked or Service Account Key is no longer valid      |
| 404       | Integration was not found                                              |
| 500       | Internal Server Error                                                  |
| 503       | The service is unavailable                                             |

### Endpoints

#### Base Endpoint

```http
https://<APP_ID>.gateway.dialogflow.cloud.ushakov.co
```

#### Endpoint Params

| Param | Description |
|--------|----------------------------------|
| APP_ID | Required, Application Identifier |

#### Example Endpoint

https://dialogflow-web-v2.gateway.dialogflow.cloud.ushakov.co

## Making Requests

**Note**: The server supports HSTS and HTTP/2, your insecure (http) requests will be automatically redirected to secure endpoint

### Retrieving Agents

#### Request

```http
GET https://<APP_ID>.gateway.dialogflow.cloud.ushakov.co
```

#### Request Params

| Param | Description |
|--------|----------------------------------|
| APP_ID | Required, Application Identifier |

#### Response Body

If successful, the response body contains an instance of [Agent](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.Agent)

#### Example Request

```http
GET https://dialogflow-web-v2.gateway.dialogflow.cloud.ushakov.co
```

#### Example Response

```json
{
  "parent": "projects/dialogflow-web-v2",
  "displayName": "DialogflowWebV2",
  "defaultLanguageCode": "en",
  "supportedLanguageCodes": [
    "ru"
  ],
  "timeZone": "Europe/Madrid",
  "description": "This is a unofficial Progressive Web Application for Dialogflow V2, with support for rich-responses and amazing features you need to check out. Choose your language and send Hello to get started",
  "avatarUri": "https://storage.googleapis.com/cloudprod-apiai/ce408f19-7966-487d-8614-f5b1f0474ba6_x.png",
  "enableLogging": true,
  "matchMode": "MATCH_MODE_HYBRID",
  "classificationThreshold": 0.3
}
```

### Detecting Intents

#### Request

```http
POST https://<APP_ID>.gateway.dialogflow.cloud.ushakov.co/<SESSION_ID>?format=<bool>
```

#### Request Params

| Param | Description |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| APP_ID | Required, Application Identifier |
| SESSION_ID | Required. The name of the session this query is sent to. It can be a random number or some type of user identifier. The length of the session ID must not exceed 36 bytes |

#### Request Body

[DetectIntentRequest](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.DetectIntentRequest)

#### Response Body

[DetectIntentRequest](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.DetectIntentResponse)

#### Example Request

```http
POST https://app-of-the-day-9a9f6.gateway.dialogflow.cloud.ushakov.co/test?format=true
Content-Type: application/json

{
    "queryInput": {
        "text": {
            "text": "Hello",
            "languageCode": "en"
        }
    }
}
```

#### Example Response

```http
HTTP/1.1 200 OK
Access-Control-Allow-Headers: Content-Type, Cache-Control
Access-Control-Allow-Methods: *
Access-Control-Allow-Origin: *
Cache-Control: max-age=86400
Content-Length: 2375
Content-Type: application/json
Date: Tue, 16 Apr 2019 10:54:54 GMT
Server: Dialogflow Gateway
Connection: close

{
  "responseId": "85c3d6bd-6f8c-4000-a1ec-fdd6f636b2e8",
  "queryResult": {
    "queryText": "Hello",
    "action": "input.welcome",
    "parameters": {},
    "allRequiredParamsPresent": true,
    "fulfillmentText": "Cannot display response in Dialogflow simulator. Please test on the Google Assistant simulator instead.",
    "fulfillmentMessages": [...],
    "webhookPayload": {...},
    "intent": {
      "name": "projects/app-of-the-day-9a9f6/agent/intents/1e00bd68-62f0-4b8e-9cdd-a81c19f73855",
      "displayName": "Default Welcome Intent"
    },
    "intentDetectionConfidence": 1,
    "diagnosticInfo": {
      "end_conversation": true,
      "webhook_latency_ms": 187
    },
    "languageCode": "en"
  },
  "webhookStatus": {
    "message": "Webhook execution successful"
  }
}
```

If you have any questions/troubles, regarding the API, please [contact us](https://ushakov.co/#contact)