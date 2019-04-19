# Dialogflow Gateway Guide

Follow this guide, if you want to setup your Dialogflow Gateway. You can find more guides and tutorials on [our Medium Blog](https://medium.com/@ushakovhq). Starting April 2019, you will need to configure and upload your service account keys manually, because of Google's limitation of 100 insecure sign-ups and permissive Google Cloud scopes, that our client used before

## Step 1: Getting your Agent's Service Account

First of all we need to know, whats the service account, that our Agent uses. To find that out, go to the [Dialogflow Console](console.dialogflow.com), select your Agent and press on cogs icon (Settings):

![](https://i.imgur.com/G49wHXs.png)

Now, if you are on the Settings Page, you need to scroll down to the "Google Project" section and **remember your Service Account**

![](https://i.imgur.com/52RFVAP.png)

## Step 2: Setting permissions of Agent's Service Account

Visit [Google Cloud IAM Dashboard](https://console.cloud.google.com/iam-admin). Make sure, you have selected the Google Cloud Project, that is associated with your Dialogflow Agent

![](https://i.imgur.com/mNjcVFQ.png)

Find the service account of your Agent in the list and press on the pen icon

![](https://i.imgur.com/vuVOGKt.png)

Now in the "Edit Permissions" window, make sure you have selected the right Service Account and the right Project. When you have checked that, add the `Dialogflow API Client` and `Dialogflow API Reader` Roles to your Service Account and press on "Save"


## Step 3: Getting the keys

Go to the Service Accounts Page, find your Service Account in the list and press on 3 dots. In the menu, select "Create key"

![](https://i.imgur.com/MmJUT2A.png)

You are going to see the "Create private key" window. Set the "Key type" to "JSON" and press on "Create"

![](https://i.imgur.com/lFzG6QY.png)

Now, the keys should be downloaded to your default Downloads folder. Make sure to check that as well

![](https://i.imgur.com/eDkBkGS.png)

Now, when you have the keys for your agent, with the correct permissions you are ready to setup Dialogflow Gateway

## Step 4: Uploading your keys to Dialogflow Gateway

Go to the [Dialogflow Gateway Console](https://dialogflow.cloud.ushakov.co/console)

Sign in with your Google Account

![](https://i.imgur.com/Ei0ibBj.png)

And you will see the console

![](https://i.imgur.com/JXCS5qk.png)

On the "Agents" section press on the "Upload" button and select your keys

![](https://i.imgur.com/bxQsK3h.jpg)

Your agent should now appear on the "Agents" list. To find your connection information and settings, press on the "Manage" button

![](https://i.imgur.com/eWvsRty.png)

# Have fun!

If you have any questions, [contact me](https://i.ushakov.co/#/contact)