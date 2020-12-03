---
layout: post
title: How did I manage 20+ micro apps in one app with scalable architecture and A/B testing
subtitle:  Thanks to Kotlin & Architecture components
use-site-title: true
tags: [android, android-architecture, software]
share_request: false
subscribe_email: false
notschedule: true
playurl: https://goodapp.in?referrer=gaurav-khanna.in
---

## Lately I was developing an app, [GoodApp]({{ page.playurl }}). Yes, that is the name of the app. The problem statement was, it contains more than 20 micro apps and I am supposed to architect in a scalable way.

{: .text-center}
![GoodApp Logo](/img/goodapp/goodapp_all_cards.png)

Though there is a lot of action happening all around the app. Few things helped me to scale quickly.

- Kotlin Coroutines
- Architecture components
- UI components
- Custom ViewController architecture

The first and the second part is out of scope of this article. If you do not know any of this, I recommend you to read [here](https://kotlinlang.org/docs/reference/coroutines-overview.html) and [here](https://developer.android.com/topic/libraries/architecture).

Every app has some architecture defined according to the requirments and for me, it was about speed and **A/B** testing of UI. Being in a startup, it was clear we need to revamp UI a couple of times until we reach wide adoption and hence some architecture was needed to support the same.

We came up with 2 ideas:

1. Creating reusable UI components
2. Making view smart-dumb and replaceable on the fly

### Creating reusable UI components