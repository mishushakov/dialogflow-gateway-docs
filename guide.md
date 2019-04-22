# Dialogflow Gateway Guide

Follow this guide, if you want to setup your Dialogflow Gateway. You can find more guides and tutorials on [our Medium Blog](https://medium.com/@ushakovhq).

If you have any questions or need help to complete these steps, please contact [Google Cloud Support](https://cloud.google.com/support-hub/)

## Step 1: Getting Service Account

Service Accounts represent identities of a Service (much like user account represents identity of a user). Dialogflow Gateway requires a Service Account, to authenticate the requests, as they were triggered from your service. In this step, we will generate such Service Account for your Google Cloud project, that is connected to your Agent.

Visit [Google Cloud IAM](https://console.cloud.google.com/iam-admin/serviceaccounts)

Make sure you are on the "Service Accounts" Page and have selected the Project, which your Dialogflow Agent is associated with

![](https://i.imgur.com/GCwRrvB.png)

Then, press on the "Create Service Account" button

Enter the name of your Service Account and press on "Create"

![](https://i.imgur.com/xa6wYs7.png)

## Step 2: Granting permissions to the Service Account

You have now successfully generated a service account! You just need to give it some permissions, so it can access certain resources of your Google Cloud project.

We will need thoose permissions: **"Dialogflow API Client"** and **"Dialogflow API Reader"**

It's important, that you set these permissions, otherwise your integration may not work as expected

![](https://i.imgur.com/zgAdbLO.png)

Set the Roles and press on "Continue"

## Step 3: Getting the keys of the Service Account

In the this step we will generate the keys of our Service Account, which we will later give Dialogflow Gateway access to.

Press on "Create Key Button" and in the "Create Key" window set the "Key type" to "JSON". Then, press on "Create".

![](https://i.imgur.com/0fcsaWy.png)

The keys should been downloaded to your default Downloads folder. Make sure to check that as well.

![](https://i.imgur.com/eDkBkGS.png)

Now, when you have the keys with the correct permissions, you are ready to setup Dialogflow Gateway

## Final Step: Uploading your keys to Dialogflow Gateway

Go to the [Dialogflow Gateway Console](https://dialogflow.cloud.ushakov.co/console)

Sign in with your Google Account

![](https://i.imgur.com/Ei0ibBj.png)

And you will see the console

![](https://i.imgur.com/JXCS5qk.png)

In the "Agents" section press on the "Upload" button and select your keys

![](https://i.imgur.com/bxQsK3h.jpg)

Your agent should now appear on the "Agents" list. To find your connection information and settings, press on the "Manage" button

![](https://i.imgur.com/eWvsRty.png)

## Done!