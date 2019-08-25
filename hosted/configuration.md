# Dialogflow Gateway by Ushakov (Hosted) Configuration

Before making any further steps, described below, please make sure, that you have successfully connected your Agent to the Gateway and it is displayed in the console, like in my case:

![Dialogflow Gateway Console](images/1*dNrnAw5_fujrbkv9z3PkDQ.png)

## Get into the configuration screen

Please click on the “Manage” button on the right side of the Agent that you want to configure:

![Dialogflow Gateway Console "Manage"](images/1*6IQ7e85pPwzec-2xpeJ4pQ.png)

The “Manage” window should now pop up. Here, select the “Settings” tab to continue:

![Dialogflow Gateway Settings](images/1*dR5bVY4fUuWwgxEdeeqm_A.png)

## Configuring Webhook

![Dialogflow Gateway Settings (Webhook)](images/1*1mXV-mVBdlEfFbFgxftaNg.png)

The Webhook field allows you to paste a URL to a Webhook, which will receive a POST request from Dialogflow Gateway, when it’s triggered.

It’s great for analytics or building custom functionality on top of Dialogflow Gateway Platform. You can code the webhook in any programming language or framework, but in our example we will use Node and Express.

### Example Webhook

Here i have coded the Example Webhook using Webtask.io:

![Dialogflow Gateway example webhook](images/1*1P5DjCpGfLWgP6lQpoV7OA.png)

It doesn’t do much more, than logging the incoming request body. Notice, that this is not the request body which is received by the Gateway, but instead it’s a matched Dialogflow response. In this case you are ensured, that your webhook only receives legit requests and does not need to handle the errors and some edge-cases.

For security reasons, i also recommend authenticating your requests via a secret request parameter or using CORS.

On the Screenshoot, i have highlighted the URL, which points to the Webhook. Paste that URL in the Console and you will see magic happening:

![Dialogflow Gateway example webhook url](images/1*Kpqq20MfRzHgPRkMbkmADg.png)

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

![Dialogflow Gateway Settings (Virtual Agent Analytics)](images/1*XZEmrPIvovzEsj3ENKx9Fg.png)

In this part of the Article, we are going to connect Google’s Area 120 [Chatbase](http://chatbase.com) Virtual Agent Analytics Service to our Dialogflow Gateway in order to get detailed Analytics of our Agent’s usage and reports from the Dialogflow Gateway Platform. Notice that, Dialogflow Gateway has no built-in service for such Analytics and does not store your message history.

![Chatbase Dashboard](images/1*NQdQk2PPVFOrxHa56Q_qqw.png)

![Chatbase Dashboard messages](images/1*62QR5SouuxZH_n9U9epMkg.png)

![Chatbase Dashboard transcripts](images/1*mPnKhb4JonFQprSGAMQEXA.png)

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

![Chatbase Bot List](images/1*dIvJurZEmgizndXTGdlFBw.png)

2. Press on “Add a bot” button on the Dashboard and enter the requested info:

![Chatbase Add bot](images/1*bitGmbe8nlnrr4IX0Ipcew.png)

3. Copy the API Key for your Bot:

![Chatbase bot API key](images/1*XiohDWc7br_BH_nkjHsLVw.png)

4. Paste the API Key into your Dialogflow Gateway Integration Settings:

![Dialogflow Gateway Settings (Chatbase API Key)](images/1*cP1g1f87V9x7ycNsTf5v1w.png)

5. View your Analytics Data

![Chatbase Dialogflow Gateway Analytics](images/1*aKTMHA4wygSZb67T2G3S6w.png)

Notice: Chatbase it can take up to 24 hours to index your messages, so be patient. For your convenience Dialogflow Gateway messages are tagged as “Dialogflow gateway” Platform in Chatbase.

Success! Now we have connected our own Webhook and configured Virtual Agent Analytics Service 🤘

## Sources

![Dialogflow Gateway Settings (Sources)](images/1*mvXf5JQnhnEtcifBEnG3Qw.png)

It’s a little bit difficult to explain this feature for newbies, but no worry, i will do my best, so you can understand, what it does and how you can benefit from it as well!

To understand what problem it solves, have a look at this Example Intent:

![Dialogflow Intent Responses Example](images/1*gzRWa6Edve7C17UzmYQiSA.png)

As you see in the “Responses” field, there can be different Responses for different Platforms. In the Simulator window, you can manually select, which Platform you want to have previewed:

![Dialogflow Simulator Responses](images/1*-AsFIfJ-1TVUZVcPmYrGQg.png)

Of course, in a Real-World application, there is no such selector, so there is also no way to know, which responses Dialogflow Gateway should respect. To solve this problem (and many others) the “Sources” feature was introduced, so you can manually select which Platforms you want to Support in your Dialogflow Gateway Integration.

## Actions

![Dialogflow Gateway Settings (Actions)](images/1*ZBL4EhjjtVRt__XUWG4kXw.png)

The last, but not least thing is the “Actions” section. You can press on the “Unlink” button, to unlink your Agent from Dialogflow Gateway.

Remember, to save your Configuration, after you make any changes