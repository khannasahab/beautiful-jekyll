---
layout: post
title: A beginner guide to Android Watch App (Wear 2.0)
bigimg: /img/android-wear/cover.webp
categories: [android]
tags: [android, app development, android-wear]
share_request: true
subscribe_email: true
---

Have you ever struggled with how to develop an Android wear app OR you want to learn basics of Android watch app? In either case, You are at right place.

As usual, whenever Android does something new, it does not do justice to
documentation. Recently we released an Android watch companion app for one of our appa and it was not a smooth experience. This article aims to cover the basics you need to know to develop a watch app. At the end of this article, you should be able to write a companion watch app. If you have experience in watch app, feel free to correct mistakes. I would appreciate notes.

Note: Android phone app development knowledge is required for this article.

#### Companion

I used companion word twice already and this is the third time. What is it? You can divide the Android watch app in 2 categories. 

Standalone and Companion app. Former is when your app is a full-fledged app on watch and does not need a supportive app on Phone, an example can be weather app. 

Latter is when you need your other app to run on the phone, an example can be smart lights app where all heavy lifting is done at phone and watch app is just to send command. Below is a screenshot for companion watch app for smart lights.

![](https://cdn-images-1.medium.com/max/1600/1*Ltj7227V3YVsWgVTTEMxSw.png)

> Android wear 2.0 is almost identical to you in terms of development with some
> restrictions of API’s not available and form factor of watch screen size.

In this article, we will discuss and learn about companion app for your existing Android app. The main app is running on phone and watch app is an add-on. We will discuss A-Z steps from development to publishing.

#### Step 1: Add wear Module

In Android studio, add a new module. Select the Android wear module.

#### Learning 1: Give same package name as your Phone module. 

This sounds silly but we wasted a good amount of time because of this. Android studio by default neither offers this nor suggests this. For standalone watch apps, it does not matter as they do not depend on phone counterpart. But for a companion app, because it needs to interact with phone module and vice-versa, the identity has to be same on phone and watch. That’s how system offers security for apps to communicate.

#### Learning 1.2: Sign with the same certificate. The Identity of apps is same when they have identical package name and same signing certificate.

If you have signingConfig blocks in main app build file, you should use same certificates for watch module. I would recommend having identical signing block.

Add minimum target to 25 as Android wear 2.0 is available to watches with
minimum Android 7

~~~
    minSdkVersion 25
~~~

#### Step 2: Add a launcher Activity

You can run your watch module (check build file, it has configuration apply **plugin**: **‘com.android.application’),** Android studio will allow you to run and it is exactly same as running an app on phone. 

Tip: You can extend `WearableActivity` which offers utility functions ambient mode entered and exited. Consider ambient mode as a configuration change you handle in phone app (keyboard, orientation). In the watch, ambient mode triggers quite frequently and you should give it a thought whether it makes sense for you to do anything or not.

Add Launcher entry in Manifest and rest everything is same as Phone. 

#### Step3 : Finding Your main app

Now your launcher activity is running, it is time to interact with your main app. I think Android has done a good job in that. As connected devices are increasing in number and form-factor, Android wear considers itself a mesh network of nodes where one Node is your phone. Your phone is no different for wear API. 

Instead of finding the connected phone, you should find a node which has the capability of fulfilling your request. It may sound little confusing now but don’t worry, You will find it very easy once you understand.

Assume a user has 2 watches (one sport and one normal), one connected ring, one newly launched watch connected to two phones at a time and some new hardware in Android wear eco-system. Now your main app is running on one of the phone and user installed your wear app on one watch.

Android calls all these devices **nodes.**

Now Android came up with a decent solution. Instead of you querying all
nodes(devices) and finding which has your app installed. Wear API offers a
utility to do so.

~~~
Wearable.getCapabilityClient(context)
        .getCapability("Capability String You define", CapabilityClient.FILTER_REACHABLE);
~~~

It means you can expose a string literal which acts as a unique identifier when wear API will find your app in nodes mesh. Simplifying more, your app module on different devices can expose some offerings (capability in Android wear term) for e.g Your phone app module can say I am “watch-server” and watch module can say I am “watch-client”. You can use the same API on both devices, watch and phone. All you need to do is use different capability.

#### In Watch

~~~
Wearable.getCapabilityClient(context)
        .getCapability(Constants.CAPABILITY_PHONE_APP, CapabilityClient.FILTER_REACHABLE);
capabilityInfoTask.addOnCompleteListener(...
~~~

**In Phone**

~~~
Task<CapabilityInfo> capabilityInfoTask = Wearable.getCapabilityClient(context)
        .getCapability(CAPABILITY_WATCH_APP, CapabilityClient.FILTER_REACHABLE);
capabilityInfoTask.addOnCompleteListener(...
~~~

Note that: These **CAPABILITY_PHONE_APP and CAPABILITY_WATCH_APP are string literal defined by you.**

Now question is how your module registers these offerings(capabilities) with system. IMHO they should have done with some Manifest tag pointing to some xml resource. However they chose to do it with only xml resource.You need to add one xml file wear.xml in your values folder. Example:

#### In Phone

~~~
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="android_wear_capabilities" translatable="false">
        <item>watch_server</item>
    </string-array>
</resources>
~~~

**In Watch**

~~~
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="android_wear_capabilities" translatable="false">
        <item>watch_client</item>
    </string-array>
</resources>
~~~

You can choose any literal name which makes sense in your context. Once you have added wear.xml file, used same package name and signed with same certificate. You should be able to find the Node which offers desired capability.

Note that this will work if the  user has an old version of your app on any node. Your search will not find the node with the desired capability and you can show an error message to the user to update the app.

Complete search Node code:

~~~
Task<CapabilityInfo> capabilityInfoTask = Wearable.getCapabilityClient(context)
        .getCapability(CAPABILITY_WATCH_APP, CapabilityClient.FILTER_REACHABLE);

capabilityInfoTask.addOnCompleteListener(new OnCompleteListener<CapabilityInfo>() {
    @Override
    public void onComplete(Task<CapabilityInfo> task) {

        if (task.isSuccessful()) {
            CapabilityInfo capabilityInfo = task.getResult();
            nodes = capabilityInfo.getNodes();
        } else {
            Log.d(TAG, "Capability request failed to return any results.");
        }

    }
});
~~~

Same code in the watch with different literal *CAPABILITY_PHONE_APP *

#### Step 4 : Communication

Now you have found nodes, all you need is some sort of communication mechanism. Message Client is perfect API for you. It is very simple:

~~~
Wearable.getMessageClient(context).sendMessage(
        nodeId, uniqueIdentifierOfMessage, dataBytes);
~~~

With this simple API, you can send any message to the node you just discovered in step 3. **NodeId** you will get from the Node. **uniqueIdentifierOfMessage **is your defined some string literal you will use to identify what message means. Example:

/getImage

/sendPushNotification

…

Data is the raw data you can send both ways. It is in the form of bytes, so you need to convert a string to byte and vice-versa.

We just sent a message, where was it received? By default, It is lost unless you add a listener. So in both apps, you should have the **addListener** line before anything.

~~~
Wearable.getMessageClient(context).addListener(messageEvent ->
        handleMessage(messageEvent));
~~~

This is where you will get the message you sent in the messageEvent form. 

~~~
switch (messageEvent.getPath()) {
case "/syncData":
performSync();
break;
}
~~~

Message event also contains node of the sender so you can respond

~~~
messageEvent.getSourceNodeId()
~~~

Overall it feels like client-server model where both parties act as client and server time to time. I hope it is clear to you how to develop a watch app.

#### Step 5 : Publishing

This section is for people who have a play-store developer account. Others can
skip.

I personally find publishing watch app little confusing. For those who have play store developer account, under release. They have an option of add from the library which did not work for me. I ended up using multi-apk approach. I had to generate watch apk with increased version code over my market app and publish normally.

It seems because of the manifest, Android play store is intelligent to serve APK to watch devices only.

Do not forget to add this in your manifest:

~~~
    <uses-feature android:name="android.hardware.type.watch" />
~~~

#### Summary

We saw Android is improving day by day and their decision of not hardcoding watch and considering everything Node is confusing at first but makes a lot of sense once you grasp the concept. There can be many devices/nodes connected at a time, few of them will have your app, few will not. Few will have an old version and few will have version mismatch and so on. To mitigate all this, we used capability client which can be used to search for Nodes who are offering those capabilities. For any node, all they need to add one wear.xml file with string array of `android_wear_capabilities` and system will take care of rest.

Once your node is found, you can interact with nodes using Message client which is very simple API to use. It is similar to a  client-server model where each message has path and data associated with it. 

