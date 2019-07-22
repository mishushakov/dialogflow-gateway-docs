# Dialogflow Gateway Guide

Follow this guide, if you want to setup, configure and use Dialogflow Gateway.

# Installation

## Step 1: Get a Service Account

Service Accounts represent identities of a Service (much like user account represents identity of a user). Dialogflow Gateway requires a Service Account, to authenticate the requests, as they were triggered from your service. In this step, we will generate such Service Account for your Google Cloud project, that is connected to your Agent.

Visit [Google Cloud IAM](https://console.cloud.google.com/iam-admin/serviceaccounts)

Make sure you are on the “Service Accounts” Page and have selected the Project, which your Dialogflow Agent is associated with

Then, press on the “Create Service Account” button

Enter the name of your Service Account and press on “Create”

![](https://cdn-images-1.medium.com/max/5200/0*8iYHLAGC8Qc5APhY)

## Step 2: Grant permissions to the Service Account

You have now successfully generated a service account! You just need to give it some permissions, so it can access certain resources of your Google Cloud project.

We will need thoose permissions: “Dialogflow API Client” and “Dialogflow API Reader”

It’s important, that you set these permissions, otherwise your integration may not work as expected

![](https://cdn-images-1.medium.com/max/5200/0*ZtCBwK0pme7dkEUa)

Set the Roles and press on “Continue”

## Step 3: Get the keys of the Service Account

In the this step we will generate the keys of our Service Account, which we will later give Dialogflow Gateway access to.

Press on “Create Key Button” and in the “Create Key” window set the “Key type” to “JSON”. Then, press on “Create”.

The keys should been downloaded to your default Downloads folder. Make sure to check that as well.

![](https://cdn-images-1.medium.com/max/4860/0*Bw3c0_7Az-MWc4gD)

Now, when you have the keys with the correct permissions, you are ready to setup Dialogflow Gateway

## Final Step: Upload your keys to Dialogflow Gateway

Go to the [Dialogflow Gateway Console](https://dialogflow.cloud.ushakov.co/console)

Sign in with your Google Account

And you will see the console

![](https://cdn-images-1.medium.com/max/5200/0*3WG-EcS8ae8H6mYR)

In the “Agents” section press on the “Upload” button and select your keys

![](https://cdn-images-1.medium.com/max/5200/0*5QltXiQ6vbGzhFM7)

Your agent should now appear on the “Agents” list. To find your connection information and settings, press on the “Manage” button

![](https://cdn-images-1.medium.com/max/5200/0*XTPYBZcSPWM3uhfx)

Congratulations! You have successfully connected your Agent to the Gateway.

If you have any questions or need help to complete these steps, please contact [Google Cloud Support](https://cloud.google.com/support-hub/)

# Configuration

Before making any further steps, described below, please make sure, that you have successfully connected your Dialogflow Agent to the Gateway and it is displayed in the console, like in my case:

![](https://cdn-images-1.medium.com/max/6720/1*dNrnAw5_fujrbkv9z3PkDQ.png)

## Get into the configuration screen

Please click on the “Manage” button on the right side of the Agent that you want to configure:

![](https://cdn-images-1.medium.com/max/6720/1*6IQ7e85pPwzec-2xpeJ4pQ.png)

The “Manage” window should now pop up. Here, select the “Settings” tab to continue:

![](https://cdn-images-1.medium.com/max/6720/1*dR5bVY4fUuWwgxEdeeqm_A.png)

## Configuring Webhook

![](https://cdn-images-1.medium.com/max/6720/1*1mXV-mVBdlEfFbFgxftaNg.png)

The Webhook field allows you to paste a URL to a Webhook, which will receive a POST request from Dialogflow Gateway, when it’s triggered.

It’s great for analytics or building custom functionality on top of Dialogflow Gateway Platform. You can code the webhook in any programming language or framework, but in our example we will use Node and Express.

### Example Webhook

Here i have coded the Example Webhook using Webtask.io:

![](https://cdn-images-1.medium.com/max/6720/1*1P5DjCpGfLWgP6lQpoV7OA.png)

It doesn’t do much more, than logging the incoming request body. Notice, that this is not the request body which is received by the Gateway, but instead it’s a matched Dialogflow response. In this case you are ensured, that your webhook only receives legit requests and does not need to handle the errors and some edge-cases.

For security reasons, i also recommend authenticating your requests via a secret request parameter or using CORS.

On the Screenshoot, i have highlighted the URL, which points to the Webhook. Paste that URL in the Console and you will see magic happening:

![](https://cdn-images-1.medium.com/max/6720/1*Kpqq20MfRzHgPRkMbkmADg.png)

For your convenience, i have also uploaded the example Code as Gist on Github:

```js
let express = require('express')
let Webtask = require('webtask-tools')
let bodyParser = require('body-parser')
let app = express()

app.use(bodyParser.json())

app.post('/', (req, res) => {
  console.log(req.body)
  res.sendStatus(200)
})

module.exports = Webtask.fromExpress(app)
```

Another thing you need to know about the Webhook functionality, is that it isn’t aware of your Webhook’s health. That means, if your webhook is down, you are not going to be able to receive requests at the time. Dialogflow Gateway isn’t going to retry and has very strict timeouts, so please make sure your webhook responds fast and is durable.

## Connecting Virtual Agent Analytics

![](https://cdn-images-1.medium.com/max/6720/1*XZEmrPIvovzEsj3ENKx9Fg.png)

In this part of the Article, we are going to connect Google’s Area 120 [Chatbase](http://chatbase.com) Virtual Agent Analytics Service to our Dialogflow Gateway in order to get detailed Analytics of our Agent’s usage and reports from the Dialogflow Gateway Platform. Notice that, Dialogflow Gateway has no built-in service for such Analytics and does not store your message history.

![](https://cdn-images-1.medium.com/max/6720/1*NQdQk2PPVFOrxHa56Q_qqw.png)

![](https://cdn-images-1.medium.com/max/6720/1*62QR5SouuxZH_n9U9epMkg.png)

![](https://cdn-images-1.medium.com/max/6720/1*mPnKhb4JonFQprSGAMQEXA.png)

On top of that, Chatbase gives you even more features to play with:

* Metrics

* Session flow

* Retention

* Cohorts

* Rich Filtering

* Funnels

* Transcripts

* Sensitive Data Masking

* Grouping of not-handled messages

* Machine learning: Suggested intents for missed & misunderstood messages (Beta)

Excited? Let’s connect our Agent to get it!

1. Sign into [Chatbase](https://chatbase.com/overview) with your Google Account:

![](https://cdn-images-1.medium.com/max/6720/1*dIvJurZEmgizndXTGdlFBw.png)

2. Press on “Add a bot” button on the Dashboard and enter the requested info:

![](https://cdn-images-1.medium.com/max/6720/1*bitGmbe8nlnrr4IX0Ipcew.png)

3: Copy the API Key for your Bot:

![](https://cdn-images-1.medium.com/max/6720/1*XiohDWc7br_BH_nkjHsLVw.png)

4: Paste the API Key into your Dialogflow Gateway Integration Settings:

![](https://cdn-images-1.medium.com/max/6720/1*cP1g1f87V9x7ycNsTf5v1w.png)

5: View your Analytics Data

![](https://cdn-images-1.medium.com/max/6720/1*aKTMHA4wygSZb67T2G3S6w.png)

Notice: Chatbase it can take up to 24 hours to index your messages, so be patient. For your convenience Dialogflow Gateway messages are tagged as “Dialogflow gateway” Platform in Chatbase.

Success! Now we have connected our own Webhook and configured Virtual Agent Analytics Service 🤘

## Sources

![](https://cdn-images-1.medium.com/max/6720/1*mvXf5JQnhnEtcifBEnG3Qw.png)

It’s a little bit difficult to explain this feature for newbies, but no worry, i will do my best, so you can understand, what it does and how you can benefit from it as well!

To understand what problem it solves, have a look at this Example Intent:

![](https://cdn-images-1.medium.com/max/6720/1*gzRWa6Edve7C17UzmYQiSA.png)

As you see in the “Responses” field, there can be different Responses for different Platforms. In the Simulator window, you can manually select, which Platform you want to have previewed:

![](https://cdn-images-1.medium.com/max/6720/1*-AsFIfJ-1TVUZVcPmYrGQg.png)

Of course, in a Real-World application, there is no such selector, so there is also no way to know, which responses Dialogflow Gateway should respect. To solve this problem (and many others) the “Sources” feature was introduced, so you can manually select which Platforms you want to Support in your Dialogflow Gateway Integration.

## Actions

![](https://cdn-images-1.medium.com/max/6720/1*ZBL4EhjjtVRt__XUWG4kXw.png)

The last, but not least thing is the “Actions” section. You can press on the “Unlink” button, to unlink your Agent from Dialogflow Gateway.

Remember, to save your Configuration, after you make any changes

# Usage

In this guide we will code our own CLI-Integration for Dialogflow using [Dialogflow Gateway API](https://github.com/mishushakov/dialogflow-gateway-docs/blob/master/api.md) and [Dialogflow Gateway JS SDK](https://www.npmjs.com/package/dialogflow-gateway).

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

* Run npm init or yarn init

* Create new file, called index.js

* Open the file using your favourite code editor

* Install the node-fetch dependency

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
const endpoint = `https://${appid}.gateway.dialogflow.cloud.ushakov.co/${session}` // <- endpoint
```

### Step 3: Define the messages loop

```js
/* Define the loop */
let ask = () => {
    /* Ask for your message */
    readline.question('You: ', (message) => {
        /* Build a request */
        let request = {
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
const endpoint = `https://${appid}.gateway.dialogflow.cloud.ushakov.co/${session}` // <- endpoint

/* Define the loop */
let ask = () => {
    /* Ask for your message */
    readline.question('You: ', (message) => {
        /* Build a request */
        let request = {
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

![](https://cdn-images-1.medium.com/max/2000/1*g7d0ZMyE_ODPiwGqalT_wQ.png)

Type in your message and press Enter:

![](https://cdn-images-1.medium.com/max/2000/1*0zPrr0SOtY9ulOXmWHhXig.png)

Your Agent will reply with Dialogflow Response, Webhook or Actions on Google Simple Response! That works 🤘

## Coding the Integration (with JS SDK)

### Step 1: Uninstall node-fetch from previous step

* NPM: npm uninstall node-fetch

* Yarn: yarn remove node-fetch

### Step 2: Install dialogflow-gateway

* NPM: npm install dialogflow-gateway

* Yarn: yarn add dialogflow-gateway

### Step 3: Change the code

```js

const { Client } = require('dialogflow-gateway')
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
})

/* Define variables */
const appid = 'dialogflow-web-v2' // <- Google Cloud Project ID
const session = 'dialogflow-cli' // <- Session ID
const lang = 'en' // <- Language

let client // <- Define the client
new Client(`${appid}`).connect().then(conn => {
    client = conn // <- Set the client
    ask() // <- Start the messages loop
})

/* Define the loop */
let ask = () => {
    /* Ask for your message */
    readline.question('You: ', (message) => {
        /* Make a request */
        client.request({
            session: session,
            queryInput: {
                text: {
                    text: message,
                    languageCode: lang
                }
            }
        })
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

Should work as promised!

## Bonus: Let your Mac speak

macOS has a pre-installed speech-synthesis library and executable, called say , try typing it into the Terminal and enter some sentences. You can even change voices by passing the -v parameter to your command.

To integrate this awesome macOS feature, do these little steps:

### Step 1: Require child_process

```js
const { exec } = require('child_process')
```

### Step 2: Execute the “say” command

```js
exec('say hello world')
```

### Final Code:

```js

const { Client } = require('dialogflow-gateway')
const { exec } = require('child_process')
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
})

/* Define variables */
const appid = 'dialogflow-web-v2' // <- Google Cloud Project ID
const session = 'dialogflow-cli' // <- Session ID
const lang = 'en' // <- Language

let client // <- Define the client
new Client(`${appid}`).connect().then(conn => {
    client = conn // <- Set the client
    ask() // <- Start the messages loop
})

/* Define the loop */
let ask = () => {
    /* Ask for your message */
    readline.question('You: ', (message) => {
        /* Make a request */
        client.request({
            session: session,
            queryInput: {
                text: {
                    text: message,
                    languageCode: lang
                }
            }
        })
        .then(res => {
            let messages = res.queryResult.fulfillmentMessages
            for (let message in messages){
                /* Display Dialogflow/Webhook Messages */
                if (messages[message].text){
                    exec('say ' + messages[message].text.text[0])
                    console.log('Bot: ' + messages[message].text.text[0])
                }

                /* Display Actions on Google Simple Response */
                else if (messages[message].simpleResponses){
                    exec('say ' + messages[message].simpleResponses.simpleResponses[0].textToSpeech)
                    console.log('Bot: ' + messages[message].simpleResponses.simpleResponses[0].textToSpeech)
                }

                ask() // <- Restart the messages loop
            }
        })
    })
}
```

Our CLI Integration can now talk!

I hope you have learned some basics on how to easily create cool and stunning Dialogflow Integrations using Dialogflow Gateway APIs! Now is your turn to make some 😄