---
layout: post
title: Emergency Communication in Your Mobile App
subtitle: Mayday, Mayday, Mayday!
categories: [mobile-apps]
bigimg: /img/emergency-mayday.jpeg
tags: [android, ios, mobile-apps]
share_request: true
subscribe_email: true
---


{: .box-bordered}
**Preface**<br/> Lately I was consulting an app holding company (they buy apps). They wanted to close the old account and re-release the entire portfolio of their apps with a new acccount. This post is the outcome of brainstorm we did for future.

Program for the good and prepare for the worst, is a sign of mature engineering. If you are a mobile app developer OR project owner, you know the pain points of app world. Once the app is gone, you have very little control. That's why we tend to use feature switches, A/B tests, gradual rollouts and needless to say, *We spend a lot of time in testing*. Irrespective of everything, you should be prepared for the worst. Here are a few communication features you should consider:


## 1. Notification with a weblink

{: .box-bordered}
This is probably the simplest and most used feature. You should always have a way to send notification to your users which opens a web page dynamically. This can be used for situations where you must reach your users and can not wait for them to come back to you. This can be used in conjuction with the other features below.

## 2. Update Dialog

{: .box-bordered}
We know the pain of users not updating the app to latest version. You should be prepared for this scenario. One solution is to implement update dialog. I divide it in 3 parts. Soft update, Medium update and Hard update. Soft update is you should show a dialog only once to user with a reasont to update and CTA. Medium update is little more annpying. You should show the dialog everytime user opens the app but allow user to cancel. Hard update is the show stopper, you should show the dialog and the user can not use the current version of the app unless she updates.

## 3. Announcement

{: .box-bordered}
This is another variant of the update dialog scneario. Here you do not show the annoying dialog to user but you show some sort of notification/message attentive icon inside your app, on click of which should either open a pre-defined place in your app OR open a weblink. It can also be divided into soft and hard category. Soft announcement is you show the notification icon once and if user checks it out, it goes away. Hard announcement is the icon remains sticky unless user takes the action. The latter form requires communication between your analytics engine and the announcement.

## 4. Kill Switch

{: .box-bordered}
Whatever be the reason if you do not want users to continue using the app, use this variant. This type of dialog is used with some weblink which can explain users whats going on. Google Trips is one recent example using this. Yes, you can update the app with only this dialog but there are chances old users do not update and you do not want that.

{: .box-plain}
Hope you learn something. If you happen to like the article, share among your circle. if you use some other technique, comment below. Let's learn together.