---
layout: post
date: 2016-01-27 05:49:57 GMT
tags:
- coding
- heroku
- deployment
- push notifications
- tricks
title: "How To: Get a Push Notification When Your Heroku App is Deployed"
---
If you’re like me, you probably have at least a few apps deployed on [Heroku](https://www.heroku.com). And if you are taking advantage of their [GitHub integration](https://devcenter.heroku.com/articles/github-integration), there's a good chance you have at least one app that [deploys automatically](https://devcenter.heroku.com/articles/github-integration#automatic-deploys) whenever you push to a certain branch. And if that's the case, you may have realized that there's a small, but important problem with that: when you push to GitHub, you no longer know when the app has finished deploying, because the whole post-deploy process now happens in the background, rather than in your terminal.

Sure, you can always log into your [Heroku Dashboard](https://dashboard.heroku.com/apps), open the app, navigate to the "Activity" tab, and watch the progress from there, but who has time for that?

![](images/posts/b2ed39a2df3b426be221f4a1d34a8c9e919c81a72fd3eaadde522c22e6c827df.gif)

That’s right: nobody. So, here's the patented, "lazy programmer" solution: you set up a [deploy hook](https://devcenter.heroku.com/articles/deploy-hooks). Heroku already supports several channels out of the box, such as [Email, IRC, Basecamp, Campfire, and HipChat](https://elements.heroku.com/addons/deployhooks), but what if you use [Slack](https://slack.com/), or you prefer a push notification instead? Well, then this guide is for you.

Without further ado, let's get started!

First, you're going to sign up for a free account at [Zapier](http://zapier.com). This is a service very similar to [IFTTT](https://ifttt.com), but while the latter seems to be more prolific in their marketing, Zapier’s engineers have been busy creating more API integrations. Unfortunately, neither of the two list Heroku as supported service. But fear not, we can still make this work, because Heroku was nice enough to also provide a HTTP hook.

Second, you'll need some way to receive notifications. In this guide, I'll be using [Pushover](https://pushover.net) for that purpose, but you can also use [InstaPush](https://instapush.im), Basecamp, or Slack for this purpose. Or, if you’re old school, you can even have an SMS send to your phone (come on now, it’s 2016, time to ditch that feature phone and get with it, grandpa).

Once you've created your account on Zapier, you'll see the dashboard, with a big orange "Make a New Zap" button. Click it. You should see the following screen:

![](images/posts/2a891709c64133c0e0f6e06b2a67de58fde44f235ee554a5e55d7e3a35eafa57.png)

Type “webhook” into the search box, and select “Webhooks by Zapier” from the autocomplete dropdown. You should see the following screen:

![](images/posts/a699036f206b652beee5383dcd12063e874983250fb682902b86d685fda91249.png)

Select “Catch Hook” and click the “Save + Continue” button. You’ll see this screen next:

![](images/posts/654bae66a27d1d0efc345259da53a9f6484718044d2d64341d382852ac9671af.png)

We don’t need this part, so just click the “Continue” button. Now you will see this:

![](images/posts/bfa77846a80869dd237b8bbdb30588c653def0e2e57db1a328fb3f3623b08e84.png)

The textbox contains a randomly generated URL, which is where Zapier will receive notifications from Heroku. Copy this URL to your clipboard and click “Ok, I did this”. Zapier is now waiting for Heroku to send it some sample data to verify that the hook is working.

Next, we'll have to do some setup in Heroku. Log into your Heroku dashboard, open your app, and under "Resources", type "Deploy Hooks" into the search box and add the add-on from there. Paste the URL you copied into the input box and click the "Save and Test" button. Now, go back to Zapier. It should now tell you that the test was successful. On to the next step!

![](images/posts/a8ecae70e7d1ea95241251b05c6c4e0912c91dd013dc63bbbca02ef09cd430dc.png)

Click "Continue", select “Push Notification” on the next screen, and then select the Pushover account to use (or set up a new one). 

![](images/posts/ef929a4e6ca1554579afc2a4593da353190a3e19eeda1aceacdee9e2ef223dae.png)

Finally, here comes the fun part: this is where you set up the push notification. Pushover supports a lot of options. You can play around with these or just use the settings from my screenshot:

![](images/posts/5acdc134939d73d4184fa1483be26a694746e0da1da244072a53bcf2ff1e2a92.png)

Note that if you set a URL, clicking on a push notification will take you to that URL right away. In other words, if you put the placeholder for your app’s URL in that field, tapping on the push notification will take you straight to your app. Neat!

Lastly, Zapier will ask you to test this step by sending a push notification to your account:

![](images/posts/199758b9e8d1e8c3629757b3cb311319cea03634a901162383ccbb79d2105d37.png)

All you need to do now is set a name for this trigger and save it by clicking “Finish”. Voila, Heroku will now send you a push notification anytime your app is deployed.  

**UPDATE:** As luck would have it, Zapier deployed a completely new UI for creating and editing Zaps just two days after publishing this article. I will update it soon with new screenshots.

**UPDATE 2:** Just revised the article and updated it with current screenshots. Enjoy!
