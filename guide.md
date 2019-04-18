# Setting Up Dialogflow Gateway

Go to the [Dialogflow Gateway Console](https://dialogflow.cloud.ushakov.co/console)

And you will see the following Sign In screen:

![](https://i.imgur.com/bse2Akc.png)

**Note**: some adblocks hide the Google Sign-In button. Please disable your adblock, if you experience the issue

Press on the "Sign in with Google" button and you will see similar account picker:

![](https://i.imgur.com/6kjJPPR.png)

Choose the account, you used to create your Dialogflow Agent with

**Note**: some browsers (like Safari in my case) block popups. Please allow popups on the website and log in and out again in order to proceed

And you will then see this page:

![](https://i.imgur.com/KkfA3bn.png)

This may happen, because Google needs to verify my app (if it hasn't done it yet). You can still continue by pressing on "Advanced" and "Go to Dialogflow Gateway (unsafe)"

Then you will see this:

![](https://i.imgur.com/MvUNN7u.png)

Press on "Allow" in order to allow the application to access your Google Cloud. Google Cloud is used for following reasons:

- Listing your Google Cloud Projects
- Listing IAM Policies for projects
- Updating IAM Policies for projects
- Creating Service Account Keys
- (Overall) Connecting your Google Cloud Project to Dialogflow Gateway

You then may see an additional confirmation, like this:

![](https://i.imgur.com/6ZED5Ur.png)

Make sure, to press "Allow" here as well

When you are ready with all of that, you will see the console:

**Note**: it may take a second to load your Google Cloud Projects (or two)

**Note**: non-Dialogflow-V2 Projects will not link, no matter how hard you try

**Note**: you may not see your projects, if you have not allowed popups and finished the previous step

![](https://i.imgur.com/AEFdvaH.png)

This list shows your Google Cloud Projects.

In order to link your Dialogflow Agent to Dialogflow Gateway, press on "Link" button, on the associated Google Cloud project.

The process may take a couple seconds to finish, so be patient. When its finished, the link icon will turn green and you will see the "Manage" button.

Then, press on the "Manage" Button and you will see this view:

![](https://i.imgur.com/G8fHU4l.png)

Save the Gateway URL, because we will need it for later

The UI URL is for the managed version of Dialogflow for Web v2, for people, who just want it to work (for example in iframe) without additional steps, described below

Optionally, you can also change your Gateway Settings:

![](https://i.imgur.com/Wp37Ycm.png)

When you specify the Webhook URL, the Dialogflow Gateway will send a POST request to it, when it is being triggered. Please note: it doesn't have anything to do with the Dialogflow Webhooks.

You can also specify the sources, which the formatting option will respect. The formatting option will then only return the components and messages for the specified Platforms (for example if you select Facebook, it will only return Facebook components for your Intents)

You can also unlink the project from Dialogflow Gateway service

**Note**: Unlinking your project, does not remove the service account keys and reset IAM Policies, you have to do it yourself (if you want to). Also note, that unlinking your project does not reset the quota or unblock the project
