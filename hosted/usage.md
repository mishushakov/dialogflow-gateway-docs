# Dialogflow Gateway by Ushakov (Hosted) Usage

In this guide we will code our own CLI-Integration for Dialogflow using [Dialogflow Gateway API](./../README.md#api) and [Dialogflow Gateway Realtime API](./../README.md#realtime-api).

![](images/1*Sax2IOrqX6_09FPS-kqLdA.gif)

It’s a badass idea, because sometimes you can’t have access to your Google Assistant, Web Browser or a Messenger, but still want to process your Messages through Dialogflow 💪

Let’s do it!

## Preparing our Integration

### Requirements

* NodeJS

* NPM or Yarn

* Agent, that is connected to Dialogflow Gateway

* 5–10 Minutes of time

### Initialising Project

* Create new directory

* Run `npm init` or `yarn init`

* Create new file, called `index.js`

* Open the file using your favourite code editor

* Install the `node-fetch` dependency

## Coding the Integration

### Step 1: Require the dependencies

```js
/* Require dependencies */
const fetch = require('node-fetch')
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
})
```

### Step 2: Define some variables

```js
/* Define variables */
const appid = 'dialogflow-web-v2' // <- Google Cloud Project ID
const session = 'dialogflow-cli' // <- Session ID
const lang = 'en' // <- Language
const endpoint = `https://${appid}.gateway.dialogflow.cloud.ushakov.co` // <- endpoint
```

### Step 3: Define the messages loop

```js
/* Define the loop */
let ask = () => {
    /* Ask for your message */
    readline.question('You: ', (message) => {
        /* Build a request */
        let request = {
            session: session,
            queryInput: {
                text: {
                    text: message,
                    languageCode: lang
                }
            }
        }

        /* Talk to endpoint and return the results */
        fetch(endpoint, {method: 'POST', body: JSON.stringify(request), headers: {'Content-Type': 'application/json'}})
        .then(res => res.json())
        .then(res => {
            let messages = res.queryResult.fulfillmentMessages
            for (let message in messages){
                /* Display Dialogflow/Webhook Messages */
                if (messages[message].text) console.log('Bot: ' + messages[message].text.text[0])

                /* Display Actions on Google Simple Response */
                else if (messages[message].simpleResponses)  console.log('Bot: ' + messages[message].simpleResponses.simpleResponses[0].textToSpeech)

                ask() // <- Restart the messages loop
            }
        })
    })
}
```

### Step 4: Start the loop

```js
ask() // <- Start the messages loop
```

### Final Code

```js
/* Require dependencies */
const fetch = require('node-fetch')
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
})

/* Define variables */
const appid = 'dialogflow-web-v2' // <- Google Cloud Project ID
const session = 'dialogflow-cli' // <- Session ID
const lang = 'en' // <- Language
const endpoint = `https://${appid}.gateway.dialogflow.cloud.ushakov.co` // <- endpoint

/* Define the loop */
let ask = () => {
    /* Ask for your message */
    readline.question('You: ', (message) => {
        /* Build a request */
        let request = {
            session: session,
            queryInput: {
                text: {
                    text: message,
                    languageCode: lang
                }
            }
        }

        /* Talk to endpoint and return the results */
        fetch(endpoint, {method: 'POST', body: JSON.stringify(request), headers: {'Content-Type': 'application/json'}})
        .then(res => res.json())
        .then(res => {
            let messages = res.queryResult.fulfillmentMessages
            for (let message in messages){
                /* Display Dialogflow/Webhook Messages */
                if (messages[message].text) console.log('Bot: ' + messages[message].text.text[0])

                /* Display Actions on Google Simple Response */
                else if (messages[message].simpleResponses)  console.log('Bot: ' + messages[message].simpleResponses.simpleResponses[0].textToSpeech)

                ask() // <- Restart the messages loop
            }
        })
    })
}

ask() // <- Start the messages loop
```

### Step 5: Go test it!

Execute node index.js and you should see this:

![](images/1*g7d0ZMyE_ODPiwGqalT_wQ.png)

Type in your message and press Enter:

![](images/1*0zPrr0SOtY9ulOXmWHhXig.png)

Your Agent will reply with Dialogflow Response, Webhook or Actions on Google Simple Response! That works 🤘

## Coding the Integration (with Realtime API)

1. Add [ws](https://www.npmjs.com/package/ws) package

   `npm install ws` or `yarn add ws`
2. Change your code to this:
   ```js
    /* Require dependencies */
    const WebSocket = require('ws')
    const readline = require('readline').createInterface({
        input: process.stdin,
        output: process.stdout
    })

    /* Define variables */
    const appid = 'dialogflow-web-v2' // <- Google Cloud Project ID
    const session = 'dialogflow-cli' // <- Session ID
    const lang = 'en' // <- Language
    const endpoint = `wss://${appid}.gateway.dialogflow.cloud.ushakov.co/` // <- endpoint
    const wss = new WebSocket(endpoint)
    wss.on('message', message => {
        let res = JSON.parse(message)
        let messages = res.queryResult.fulfillmentMessages
        for (let message in messages){
            /* Display Dialogflow/Webhook Messages */
            if (messages[message].text) console.log('Bot: ' + messages[message].text.text[0])
            /* Display Actions on Google Simple Response */
            else if (messages[message].simpleResponses)  console.log('Bot: ' + messages[message].simpleResponses.simpleResponses[0].textToSpeech)
            ask() // <- Restart the messages loop
        }
    })

    /* Define the loop */
    let ask = () => {
        /* Ask for your message */
        readline.question('You: ', (message) => {
            /* Build a request */
            let request = {
                session: session,
                queryInput: {
                    text: {
                        text: message,
                        languageCode: lang
                    }
                }
            }

            /* Talk to endpoint and return the results */
            wss.send(JSON.stringify(request))
        })
    }

    ask() // <- Start the messages loop
   ```
3. Run test it!

I hope you have learned some basics on how to easily create cool and stunning Dialogflow Integrations using Dialogflow Gateway APIs! Now is your turn to make some 😄