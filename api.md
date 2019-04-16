# API Documentation

Contents

- Basics
    - Response Codes
    - Endpoints
        - Base Endpoint
        - Endpoint Params
        - Example Endpoint
- Making Requests
    - Retrieving Agents
        - Request
        - Request Params
        - Response Body
        - Example Request
        - Example Response
    - Detecting Intents
        - Request
        - Request Params
        - Request Options
        - Request Body
        - Request Fields
        - Response Body
        - Response Fields
        - Example Request
        - Example Response

## Basics

### Response Codes

| HTTP-Code | Reason                                                                 |
|-----------|------------------------------------------------------------------------|
| 200       | Request was successful                                                 |
| 400       | Request Body or Session ID is invalid and/or missing                   |
| 403       | Your Integration was blocked or Service Account Key is no longer valid |
| 404       | Your Integration was not found                                         |
| 500       | Internal Server Error                                                  |
| 503       | The service is unavailable at the moment                               |

### Endpoints

#### Base Endpoint

```http
https://<APP_ID>.gateway.dialogflow.cloud.ushakov.co
```

#### Endpoint Params

`APP_ID` - Required, Application Identifier

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

If successful, the response body contains an instance of [Agent](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/Agent)

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

#### Request Options

| Option | Values | Description |
|--------|-----------------|--------------------------------------------|
| format | `true`, `false` | Optional. Enable/Disable formatting option |

#### Request Body

```json
{
  "queryParams": {
    object(QueryParameters)
  },
  "queryInput": {
    object(QueryInput)
  },
  "outputAudioConfig": {
    object(OutputAudioConfig)
  },
  "inputAudio": string
}
```

#### Request Fields

| Field | Reference |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| queryParams | object([QueryParameters](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/projects.agent.sessions/detectIntent#QueryParameters)) Optional. The parameters of this query. |
| queryInput | object([QueryInput](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/projects.agent.sessions/detectIntent#QueryInput)) Required. The input specification. It can be set to: An audio config which instructs the speech recognizer how to process the speech audio, a conversational query in the form of text, or an Event that specifies which intent to trigger. |
| outputAudioConfig | object([OutputAudioConfig](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/projects.agent.sessions/detectIntent#OutputAudioConfig)) Optional. Instructs the speech synthesizer how to generate the output audio.If this field is not set and agent-level speech synthesizer is not configured, no output audio is generated. |
| inputAudio | string([bytes](https://developers.google.com/discovery/v1/type-format) format) Optional. The natural language speech audio to be processed. This field should be populated iff queryInput is set to an input audio config. A single request can contain up to 1 minute of speech audio data. A base64-encoded string. |

#### Response Body

```json
{
  "responseId": string,
  "queryResult": {
    object(QueryResult)
  },
  "webhookStatus": {
    object(Status)
  },
  "outputAudio": string,
  "outputAudioConfig": {
    object(OutputAudioConfig)
  }
}
```

#### Response Fields

| Field | Reference |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| responseId | string The unique identifier of the response. It can be used to locate a response in the training example set or for reporting issues. |
| queryResult | object([QueryResult](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/projects.agent.sessions/detectIntent#QueryResult)) The selected results of the conversational query or event processing. See alternativeQueryResults for additional potential results. |
| webhookStatus | object([Status](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/projects.operations#Status)) Specifies the status of the webhook request. |
| outputAudio | string ([bytes](https://developers.google.com/discovery/v1/type-format) format) The audio data bytes encoded as specified in the request. Note: The output audio is generated based on the values of default platform text responses found in the queryResult.fulfillment_messages field. If multiple default text responses exist, they will be concatenated when generating audio. If no default platform text responses exist, the generated audio content will be empty. A base64-encoded string. |
| outputAudioConfig | object([OutputAudioConfig](https://cloud.google.com/dialogflow-enterprise/docs/reference/rest/v2/projects.agent.sessions/detectIntent#OutputAudioConfig)) The config used by the speech synthesizer to generate the output audio. |

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
