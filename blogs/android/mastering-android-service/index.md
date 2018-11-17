---
layout: post
title: Mastering Android Service in 2018
subtitle: WTS - What and Why of Service
categories: [android]
tags: [android, adroid internal, app development]
share_request: true
subscribe_email: true
---

If you are an Android developer and got a chance to work with Services (which I
presume everyone should have at least once), you are at the right place. If you
have not touched services till date, trust me you should skip the coffee gossip
today with colleagues and read+implement services. If you are an expert in
Service, foreground service and bonded service; you can host the Gossip today.

> I know **Service** is an old concept. Let me assure you we will not discuss the
> basics and we will learn the recent changes made to the service layer in Android
8.0+, we will solve the mystery of famous **IllegalStateException** and
**RemoteServiceException**

This article will help you understand ‘Why’ aspect of service. You should be
able to use services confidently and debugging should be slightly easier than
before. I believe you will learn at least one new thing. If you do, promise me
you will **upvote**, follow me (if you want) and feel free to offer a coffee. If
you do not learn anything at all, I failed at one job I was supposed to do. Do
let me know, I will improve.

<br> 

![](https://cdn-images-1.medium.com/max/1600/1*yCods9P1WXv4WvA6O9be7g.png)
<span class="figcaption_hack">The image is for good preview when you share the article, I care for you</span>

#### What is Service

Unconventional definition: Service is a component which we can think of like a
flag. It says to the system, User is not interacting with the app but the app is
doing some important work. Please mark this app process important over other
background apps and put it as last as possible in the killing list. **Service is
not related to execution and threading.** There are other means to do background
operations. Usually, we combine service with those other means. Service helps in
two folds: keeps the process afloat till the system can And helps in gaining
thread priority.

> If you do not understand the paragraph before, I advise you too to read further
> only when we are on the same page.

We will discuss vanilla flavors of service in this article. Background service,
Foreground service, and bound Service.

From the definition, we know a service is just like a flag which keeps the
process alive as much as it can.

#### Serious Problem

You must be asking, **what if all apps use service** and they never get killed?
OR they get more priority. You are right.

If all apps use service and the system is in need of resources. It has to lean
towards other methods, may be FIFO or LRU or Lottery OR whatever. It is none of
our concern. There are gazillion of other things (poverty, Gender inequality …)
to worry about.

Let’s go over a possible scenario. User boots the device. All apps listen to
BootReceiver and start a service immediately.<br> Letting the apps start and
then killing them one by one is fine by the processor but someone pays a hefty
price. Who pays the hefty price? You can guess. Yes, you can.

You are right, **Battery and User**

If so many apps are running all the time, the system is busy all the time doing
one thing or another. The battery dies frequently and the user suffers.
Secondly, if many apps are running with services the system mostly feels slow to
the user. The system has to kill apps and perform garbage collection frequently.
Overall, a bad performing phone. Now you understand why people install those
apps-killers and battery optimizers.

If you are wondering why would developers do this? Ask yourself: we, in most of
the world, are civilized and democratic. Yet our prisons are full! Why? People
do such stuff. Devs are no different.

#### The Solution

#### Part 1 — Foreground service

A service which says to the system “Do not kill me, please” and notifies the
user with a notification that some background important work is happening. The
implementation is very simple.

Start the service and call startForeground method passing a notification.

The benefit of foreground service is, it has more priority over normal service.
It means it will not be killed before normal service. The Android system tried
to lure developers with this contract. “**More priority**” “**More Respect**”
“**More Battery**”

**ForegroundApp > ForegroundService > Service**

Unfortunately, the bad guys did not want more respect, priority, battery, and
popularity. They just wanted more money. They kept using normal services.

#### Part 2 — Forced Foreground service

Before I explain anything, let me share a crash. Many of you must have seen this
crash when Android 8 rolled out.

    Caused by: java.lang.IllegalStateException: Not allowed to start service Intent { xyz....../abc ..... }: app is in background

    Background start not allowed

OR this

    android.app.RemoteServiceException: Context.startForegroundService() did not then call Service.startForeground()
     at androActivityThread$H.handleMessage(ActivityThread.java:1768)

Exception 1 is straightforward. It is the solution to the problem we just
discussed in part 1.

> Because developers were not being Social, Android decided to penalize. I know in
> some countries, they penalize women for being social. Crazy world we live in.
Anyways, let’s move on. That’s what we do.

It simply says if your app is in the background you are not allowed to use
**startService** method when you want to start a service. You must use the other
variant **startForegroundService **followed by startForeground() to show a
Notification to the user. That way, the user knows who is draining the battery.

Believe me, It’s not that big of a change. If you are a legitimate app, you do
not want to hide anything from users and showing a Notification should not
bother you at all. It surely can be a problem for those data stealing, super
good looking yet shady apps. Sure there are other alternatives like JobScheduler
but we are here to talk about Services.

With this solution, many apps stopped bombarding the system with foreground
service because they knew they were not a good candidate. Battery improved, the
user was happier and developers rushed, more coffee was sold.

Before we discuss crash 2, let’s dig more into **Forced Foreground Service.
**There are certain scenarios we must consider. I will keep it quick and short.

#### 1. What happens when you call stopService and you never had called startService?

Answer: No Problem, Android does not mind

#### 2. What happens to services you start (using startService and not
startForegroundService)when you were in the foreground but went background
later?

Answer: I could not find any issue with this. Maybe Android consider such apps
“not shady” because the user has interacted with them. So they are okay for now.
It is a genuine use case also, say you are fetching remote config from a server.

But sometimes we see this crash:

    Caused by: java.lang.IllegalStateException: Not allowed to start service Intent { xyz....../abc ..... }: app is in background

    Background start not allowed:

**3. What does foreground/background mean here?**Usually, app will be in one of
these states:<br> a) The user is interacting with your app (You can use
startService and startForegroundService both. No problemo!)<br> b) The user
pressed the home button (Your app is in the background but there is one process
alive) If you guess you have to call startForegroundService, unfortunately, that
is not always true. I tried many a times and system allows to use startService
and startForegroundService both. <br> c) This is a tricky scenario, when your
app process never came to the foreground but it was spawned by system maybe
because of BroadcastReceiver, AlarmManager, JobScheduler etc. Here you **can
not** use both startService and startForegroundService.

#### Conclusion

The background does not mean fake background. It means true background. But why
to bother this much, if you are in OreoORHigher aim for startForegroundService.
If you are sure your app is in the foreground, you are free to use startService
also but I will advocate against it. Why to increase complexity?

#### 4. What happens when you call startForegroundService multiple times? Do you need
to call startForeground every time?

Answer: **No**, if service is already in foreground. System does not require you
to call startForeground every time. So you can avoid keeping the startForeground
call in onCreate and onStartCommand both. You can only keep it in onCreate

#### 5. What happens to bound service?

Bound services are not considered as pure background services and that’s why
they are exempted from this. You can bind to a service even when you are in the
background. I find it a flaw in the system. Isn’t that whole of Android?

#### 6. Services is bounded but later started from the background.

By far this is one of the weirdest (if it is a word, it justifies itself)
scenario I faced. If the service was bounded earlier but later started using
startForegroundService()

     in com.yourprocess
     PID: your_pid
     Reason: Context.startForegroundService() did not then call Service.startForeground()

    Bringing down service while still waiting for start foreground:

Yes ANR. I have no clue why!

#### Part 3 — Forced Foreground service with a twist

Is it too much already? Hang tight. Grab next coffee and let’s move to exception
2.

    android.app.RemoteServiceException: Context.startForegroundService() did not then call Service.startForeground()
     at androActivityThread$H.handleMessage(ActivityThread.java:1768)

Sometimes when you follow all driving rules but cross a red light where no one
is nearby, you still get the ticket. This exception is quite similar.

This happens when you call startForegroundService after Android 8.0 BUT you do
not call startForeground within 5 seconds (aprx). This is agnostic to app state,
foreground OR background. But why would not you?

There are a few possible explanations for this to happen.

Assume you call startForegroundService

1.  Android did not call your service on create and crashed your app [ Possibility
of this happening is almost 0, I know Samsung you are laughing]
1.  You had some conditional check and did not call startForeground method because
of that
1.  You call startForegroundService and then stopService immediately before it even
started

In the first case, your onCreate/onStartCommand will not be called [ I have not
seen this ]

The second case is totally yours to handle. You can fix it.

The third case is very tricky, sometimes we do not understand how is it
happening, yet it happens. For example, you start a service when you are
connected to the server and you stop the service when you disconnect from the
server. What if you get connect call followed by disconnect call?

Putting Simply, if you call

    startForegroundService

followed by

    stopService

Before you call

    startForeground(notifId, notification);

You meet this:
```
    android.app.RemoteServiceException

    More info in logs "ActivityManager: Bringing down service while still waiting for start foreground"
```

How to reproduce:
```
    startForegroundService(intent);
    followed by
    stopService(
    Intent(context, YourService.
    ));
```

From the system perspective, before service could start its onCreate method, the
stop was demanded and thus the crash. Maybe Android could have been lenient
here. But it is what it is. That’s life.

    Avoid calling stop immediately after start. You can have your own flag which becomes true after you call startForeground OR you can ask system if the service is running before you call stop.

    However, one caveat is sometimes you will end up in scenarios where you will not stopService. I will leave upto your business logic. Maybe you can check the condition after onStartCommand. You know better.

### <br> 

#### Short Summary

### 1

```
    if (
    ()) {
        context.startForegroundService(intent);
    } else {
        context.startService(intent);
    }
```

### 2

```
    startForegroundService(intent);
    followed by
    stopService(new Intent(context, YourService.class));

    before startForeground(notification) was called. Foreground/background does not matter.
```

### Long Summary

Please read the article again.

