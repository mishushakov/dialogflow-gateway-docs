# Dialogflow Gateway

![](https://svgur.com/i/EHv.svg)

Dialogflow Gateway is a backend enabling third-party integrations to securely access the Dialogflow V2 API. Any server, that implements the API (see below) can be refered as *Dialogflow Gateway*

## Contents

- [API](#api)
    - [Response Codes](#response-codes)
    - [Errors](#errors)
    - [Endpoints](#endpoints)
        - [Base Endpoint](#base-endpoint)
        - [Base Endpoint Params](#base-endpoint-variables)
        - [Base Endpoint Example](#base-endpoint-example)
- [Requests](#requests)
    - [Retrieving Agents](#retrieving-agents)
        - [Request](#request)
        - [Request Variables](#request-variables)
        - [Response Body](#response-body)
        - [Example Request](#example-request)
        - [Example Response](#example-response)
    - [Detecting Intents](#detecting-intents)
        - [Request](#request-1)
        - [Request Variables](#request-variables-1)
        - [Request Body](#request-body)
        - [Response Body](#response-body-1)
        - [Example Request](#example-request-1)
        - [Example Response](#example-response-1)
- [Implementations](#implementations)
- [Contact](#contact)

## API

### Response Codes

| HTTP-Code | Reason                                                                 |
|-----------|------------------------------------------------------------------------|
| 200       | Request was successful                                                 |
| 400       | Request Body or Session ID is invalid and/or missing                   |
| 403       | Integration was blocked or Service Account Key is no longer valid      |
| 404       | Integration was not found                                              |
| 500       | Internal Server Error                                                  |
| 503       | The service is unavailable                                             |

### Errors

Example JSON response, containing error

```json
{"error": "Could not find your Deployment"}
```

### Endpoints

#### Base Endpoint

Single-Agent scenario

```
https://<PROJECT_ID>.com
```

Multi-Agent scenario using wildcard subdomain

```
https://<PROJECT_ID>.example.com
```

Multi-Agent scenario using path parameter

```
https://example.com/<PROJECT_ID>
```

Multi-Agent scenario using query parameter

```
https://example.com?agent=<PROJECT_ID>
```

#### Base Endpoint Variables

| Variable   | Description                  |
|------------|------------------------------|
| PROJECT_ID | Required, Project Identifier |

#### Base Endpoint Example

Multi-Agent scenario using wildcard subdomain on Dialogflow Gateway by Ushakov (Hosted)

```
https://dialogflow-web-v2.gateway.dialogflow.cloud.ushakov.co
```

## Requests

#### Retrieving Agents

#### Request

```http
GET <BASE_ENDPOINT>
```

#### Request Variables

| Variable      | Description                 |
|---------------|-----------------------------|
| BASE_ENDPOINT | Required, Endpoint of Agent |

#### Response Body

[Agent](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.Agent)

#### Example Request

Multi-Agent scenario using wildcard subdomain on Dialogflow Gateway by Ushakov (Hosted)

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
POST https://<BASE_ENDPOINT>/<SESSION_ID>
```

#### Request Variables

| Variable | Description |
|----------|-------------|
| BASE_ENDPOINT | Required, Endpoint of Agent |
| SESSION_ID | Required. The name of the session this query is sent to. It can be a random number or some type of user identifier. The length of the session ID must not exceed 36 bytes |

#### Request Body

[DetectIntentRequest](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.DetectIntentRequest)

#### Response Body

[DetectIntentResponse](https://cloud.google.com/dialogflow/docs/reference/rpc/google.cloud.dialogflow.v2#google.cloud.dialogflow.v2.DetectIntentResponse)

#### Example Request

Multi-Agent scenario using wildcard subdomain on Dialogflow Gateway by Ushakov (Hosted)

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

```json
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

## Implementations

| Title                                   | Developer | Status                      | Homepage                                                     | Features                                                                                                                                                  | Cloud-based? |
|-----------------------------------------|-----------|-----------------------------|--------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Dialogflow Gateway (Hosted)             | Ushakov   | Stable, actively maintained | https://dialogflow.cloud.ushakov.co                          | Security Features, API Extensions, support, free updates, uptime guarantees, disaster recovery, dashboard, built-in integrations, pay-per-use, free quota | Yes          |
| Dialogflow Gateway (Enterprise Edition) | Ushakov   | TBA                         | https://dialogflow.cloud.ushakov.co                          | Self-hosted, premium support, API Extensions, free updates                                                                                                | No           |
| Fulfillment Tester                      | Ushakov   | Testing                     | https://github.com/mishushakov/dialogflow-fulfillment-tester | For testing purposes only                                                                                                                                 | No           |

Submit a PR, if you want to add your implementation to the list!

## Contact

If you have any questions/troubles, regarding the API, please [contact us](https://ushakov.co/#contact)