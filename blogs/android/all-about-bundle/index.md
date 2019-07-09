---
layout: post
title: Everything about Android Bundle
subtitle:  Build to Dynamic Features
use-site-title: true
bigimg: /img/all-about-bundle.jpeg
tags: [android, android-build, dynamic-feature]
share_request: true
subscribe_email: true
---

If you are in the Android world, you must have heard about Android Bundle format
lately. There was a lot of buzz around this last year and if you have not moved
yet, your play store developer page reminds you of bundle benefits every-time
you release.

This article is to simplify bundle so you can decide whether it is a good fit or
not for your organization.

> Bundle format is a good example of engineering but not well documented in my
> eyes.

### Bundle — The Birth Story

In one line, it is a format of delivery which is user-centric.

Sure the line above is complex to digest. Let me simplify it.

Every product has a final binary format. It is apk in the Android context. Apk
is the format which Android OS understands how to execute. A user can download
an apk from Google Play store, third-party stores OR even from any website.

An apk consists of the code developers wrote along with resources needed to run
the app. Resources are images (drawable), strings, assets(music file) etc.

The interesting part is mobile devices and apps are used in every part of the
world. The same app can be used in the United States OR Germany. The same app
can run on a small phone, a big phone, a tablet OR even a TV. The same app runs
on different CPU architecture. There is so much fragmentation in Android from
devices to OS version.

To cater to all this, the same app needs to have localized resources (USA,
Germany, etc). The same app needs to have resources for different screen sizes.
The same app needs to have different binaries for different CPU architectures.

APK used to consist of all of the localized resources, all screen size
resources, all CPU architecture binaries, etc. It is very simple to create an
APK. One click and you are done.

The solution is simple but like other simple solutions in the Software world, it
comes with its drawbacks. The drawback is all the resources are packed in each
APK and shipped to users. German users are getting ENGLISH resources and vice
versa, Small phones are getting Big phones resources and vice versa. We are
directly wasting not only so much bandwidth but memory on users phones as well.
If we have something in abundance, it does not mean we should waste it. It means
we should divide it among others who lack. Next time when you pass that poor
lady, give her food if you can.

**Solution 1**

One day Google realized something needs to be done. It prepared [Google Play
store to accept multi
apks](https://developer.android.com/google/play/publishing/multiple-apks) and
allowed developers to generate multi apks using Gradle. You could with build
file, generate n apks instead of 1 (n >1). The only catch Google Play did not
support the same version number and you needed to write some script to generate
apks with different version number and then upload those apks to play store.

This solution was working fine until one day, an engineer said. **DRY**. DO NOT
REPEAT YOURSELF. It means do not have the exact same codebase at multiple
places. In this context, it was having the same build methods in different
companies.

Secondly, generating and maintaining multi apk was surely harder than 1 apk.
Every app company had to come up with their own way of organizing this which
simply was not helping their business directly. They could use the same
man/woman power somewhere else.

**Solution 2**

Google said enough is enough. I made this mess. Let me correct it. Instead of
you all generating and maintaining the multi-apks, I will do that for you.
That’s how Bundle was born.

![](https://cdn-images-1.medium.com/max/1600/0*ijYzxFF-5l_kHTfJ.png)

### Little more history

As we know Android OS understands APK format. Before Android 5 (21), one app
could have one APK. In Android 5 (21), Google developed a mechanism to support
multiple apk for a single app. It is like one apk contains English resources,
one apk contains German, one apk contains large display resources, one apk
contains medium display resources etc.

The OS was ready to support multiple apks for a single app, it is only the
creation of multi apk was painful. Something was needed, Google named it Bundle.

### [Bundle](https://developer.android.com/guide/app-bundle)

As you read before the Android platform supports one app having multiple apks.
It could be API level, screen size, CPU architecture, etc. Now is the time to
make it better. All you need to do is generate the new binary format (Bundle)
and the tool will take care of generating the required APKs. You do not need to
mention factors (API, screen, CPU etc). Not only this, tools can take it to the
next level and truly generate a lot of apks from one bundle ( by adding more
factors like language ). For example, my app bundle has following apks inside.

[You can check with bundletool](https://developer.android.com/studio/command-line/bundletool)

![](https://cdn-images-1.medium.com/max/1600/0*YBsg3KkisjkSQXmr.png)

It has 25 apks. Split intelligently using the languages, CPU architecture,
screen size etc. All with zero code. Now if a user downloads this app from play
store, Google play will send not one but few apks based on the device
requirements. It might send it

### base-x86_64.apk + base-xxhdpi.apk + base-fr.apk

![](https://cdn-images-1.medium.com/max/1600/0*xEvBk6o-xsdTuAjq.gif)

The benefit is crystal clear, instead of downloading everything an app has. It
downloads the required files, thus saves bandwidth, memory on device, app
footprint etc.

### Possible Issues

1.  **Missing Resources with Bundle**

As anything else in life and software, nothing comes without a tradeoff. This
amazing delivery mechanism has its own shortcomings. One straightforward is “Not
everything is downloaded” So “Not everything should be accessed freely”.

Before Bundle, when your apk is installed, the device has every resource. You
can use any locale resource using
[Context](https://gaurav-khanna.in/blogs/android/mastering-android-context/) OR
by other means like reflection. But now, because it is not downloaded, it won't
be found. You need to stop accessing via reflection OR you need to have
defensive checks.

One example of reflection
```
   int instructionsResId =getResources().getIdentifier("mygame_instructions" , "raw", getPackageName());
```

Though instructionsResId can be 0 without Bundle as well, with Bundle there are
higher chances.

**Rule of Thumb: Stop Reflection**

**2. Language Changed but showing English**

It is not very common for a user to change the locale of the mobile phone. As
you know, initially the system downloads only the needed apks. It means English
users will not get German resources and strings. If a user changes the locale
from English to German, Play store will batch and download the required apks in
the background without you needing to do anything. That is dope! The only flaw
is sometimes, the user might see the default version (raw, values of your app
without identifier) for some time.

**3. Support for In-App Language change option**

Again, it is not very common to have this feature inside apps but many apps
allow to do so. Read about
[Context](https://gaurav-khanna.in/blogs/android/mastering-android-context/) if
you want to learn more. Context is the soul of your app in the Android
framework.

The summary is, you can attach a base context with the new configuration you
want, and boom! You expect your app to pick resources from the new configured
context. The only catch now is even though, your new context says I want German
resources while you are in an English speaking country. Android will try to find
the resources and this time, it will simply fail to find and fallback on the
default version. The reason is german localized apk was never downloaded and
play store has zero knowledge about your app changed the context. The solution
is simple yet complicated.

You need to use [PlayCore
Library](https://developer.android.com/guide/app-bundle/playcore) and flag to
the system whenever you want to download the localized apk. I am not going in
details here but in a nutshell, you can tell the system to download using
SplitInstallManager and pass the language identifier. Below is the code if you
have locale and you want to download the localized apk (language resources and
strings in this case).

```
splitInstallManager = SplitInstallManagerFactory.create(getActivity());
    SplitInstallRequest 
request = SplitInstallRequest.newBuilder().addLanguage(Locale.forLanguageTag(locale.getLanguage())).build();
splitInstallManager.startInstall(request);
```

NOTE: You can call this any number of times you want and the system will
download only if the apk does not exist. If it exists, you will get callback
immediately.

It is equivalent of the user changing the locale in the phone setting and google
play store getting informed. The only catch is user is inside the app and Play
store does not want to restart the app by itself. After successful installation
of the apk, either you restart the app programmatically so the system can pick
the new resources OR you tell the system to do for you.

**Informing system to install the latest installed apk so you can use**

You can do this by adding two lines in your code

1.  In Activity

```
override fun attachBaseContext(newBase: Context) { super.attachBaseContext(context)
    SplitCompat.install(
this)
}
```

1.  In Application Class

```
@Override
protected void attachBaseContext(Context base) {
    
super.attachBaseContext(base);
    SplitCompat.install(
this);
}
```

If you miss one of these, your latest dynamically installed apk resources won't
be available. Do not waste time on this. Netflix has a lot of content and Tinder
is full of profiles.

### Modules

![](https://cdn-images-1.medium.com/max/1600/0*zmujT62FmaJJo4wD.png)

An Android app is made of modules where some modules are written by the app
developer and some might be libraries written by other developers. For example,
if you have an image editing app, you can structure your app in the following
way:

![](https://cdn-images-1.medium.com/max/1600/1*B72BUsAXhLqPtm6M9SXjPQ.png)

It is just an example, you might create totally different modules depending on
your app and your experience. For the sake of example, let's say we have these 7
modules in our app.

In Android app structure, the default name of the base-module is “**app**”

![](https://cdn-images-1.medium.com/max/1600/0*erZy0U8Ki1MK8ffe.png)

Though it is not mandatory and you can name it whatever you want (All you need
to do is use **apply plugin: ‘com.android.application’ in **build file of the
base-module). [You can learn more about build files here](https://medium.com/mindorks/introduction-to-android-build-system-for-beginners-cfafa1ab4104).

As we use Gradle in Android, we use “implementation” in build file to express
the dependency on any module. For example, your app module build file might look
like this:

![](https://cdn-images-1.medium.com/max/1600/0*5sPxigMiHk8UX1pw.png)

Here app is explicitly saying, I need these dependencies to be shipped along
with me to function properly. In a nutshell, a module is a reusable library and
in most cases, the base-module depends on it. However, any module can also have
app as a dependency but it is generally considered a bad practice to have
circular dependancy. A → B → A. [You can learn more about Android build files
here.](https://gaurav-khanna.in/blogs/android/build-system/)

**Multi Apk + Modules + Bundle Format**

From the first section, we learned we already have multi-apk support, bundle
format in place and we can download an apk at runtime. Some genius proposed at
Google, how about using the same underlying mechanism and allow developers to
download a module at runtime. But why his manager asked?

**His arguments were:**

1.  Users can download only features they need with initial apk from play store
1.  Will save bandwidth
1.  Users prefer to try small size apps
1.  It will enforce developers to modularise the apps
1.  Better mental model
1.  Parallel development possible
1.  Structured Code
1.  It is an amazing gimmick in the tech world and ios do not have it (Sold!)

Why not! Everyone responded and that’s how the dynamic feature was born. (It is
a fictional story)

### [Dynamic Delivery of Features](https://developer.android.com/studio/projects/dynamic-delivery)

Now we know dynamic feature module is a simple Android module in your app which
is marked to download at runtime, thus not available to your app initially.

It is very simple to make a module dynamic, all you need to do is add a line in
the build file of the module and a line in the build file of the app.

### Old way to make a module

```
apply plugin: 'com.android.library' // in build file of module
and
implementation project(
':moduleName') // in the base module (app)
```

### The new way to make a dynamic module

```
apply plugin: 'com.android.dynamic-feature' // in build file of module
and
dynamicFeatures = [":module_name"] // in build file of base module (app)
```

There are two subtle differences. You need to use the dynamic-feature plugin in
the module build file and use dynamicFeatures instead of implementation in the
app module.

The module is dynamic now, you need to give more information to Android which is
distribution related. Add a dist tag in the manifest of your module. This tag
tells the build system and Play store information about your module.

```
<dist:module
    
dist:instant="false"
    dist
:onDemand="false"
    dist
:title="@string/title_dynamic_feature_at_install">
<dist:fusing dist:include=
"true"/>
</
dist:module>
```

**dist:instant **tells** **if the dynamic module is an [instant module](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)(Out
of scope of this article).

**dist:onDemand** means even though it is a dynamic module but we can ship it
with the initial apk itself but marking false. If onDemand is true, then the
dynamic module is not available at install time and you have to request
download.

If it puzzles you, these are the reasons I could think of why to mark it false:

1.  You are preparing the app for dynamic features and do not want to release
dynamic modules right now
1.  This feature is the first feature user needs in your app and making it dynamic
delivery does more harm than good BUT because it is dynamic, you can uninstall
it once it is used (e.g signup flow) and release memory from phone

**dist:title** is the title is used by the system to show a dialog to user when
system thinks it needs permission from user. Though Android says it will ask
user permission if the module you are about to download is more than 10mb but I
find the behavior inconsistent. So, I would suggest being prepared and handle
all the cases.

```
<dist:fusing dist:include="true"/> is used by Android to deliver the apk before Android 21 // out of scope
```

Let’s assume we marked the module onDemand=true, we can check if the module is
downloaded by one simple line

```
val installedModules: Set<String> = splitInstallManager.installedModules
```

And you can initiate the download with this simple API.

```
request = SplitInstallRequest.newBuilder()
            .addModule(identifier.toLowerCase())
            .build()
splitInstallManager.startInstall(request);
```

Note that downloading a module is the same as downloading the language resources
dynamically. Android considers both dynamic modules.

### [I recommend reading this official documentation](https://developer.android.com/guide/app-bundle/playcore)

### [and check this official sample](https://github.com/googlesamples/android-dynamic-features)

> I wish I could say, that’s it. You have the dynamic feature module ready to use
> but that is not the case. Android is not that simple.

### Issues And Complexities with dynamic feature modules

This section is for Advanced use-cases and not every developer might
need/understand

### 1. Resource merging and name conflict

In the pre dynamic world, where we have app module and library modules. We use
the **implementation **in the build file to express our dependencies. After
that, we are free to use modules resources and classes. If your module has a
drawable ic_notification, you can use R.drawable.ic_notification in the app even
though ic_notification is not in the app and R is the generated file of the app.

It is possible that library module and base module have the same name of
resources (Not suggested by Android but it is possible). In such cases, while
building the final binary Android system uses main-app resources. [You can read
about build process and aapt here](https://developer.android.com/studio/build).
Same is applicable to other resources like layout, string etc.

**But in Dynamic Feature World, **it is not even possible to have the same
resource name. You can not have the same file name in layout/drawable inside a
dynamic module and your app module. The reason is dynamic module is not merged
into app because they are available at runtime. But the interesting part is, you
can have the same name string resource in dynamic module and app module. I don’t
know why!

One more important distinction is, R file of modules and the app is merged in
pre-dynamic world but it is not merged in the dynamic delivery case. So there is
a separate R file generated and kept separate from the main R for each dynamic
module. You can access the resources of the module using the R file.

### 2. Access to resources and classes

In pre-dynamic world, if you are using a module as a dependency, you can access
to its resources and classes. In our case, the app module can access module
resource and classes. But it dynamic-feature world, app module is not using
dynamic-feature as implementation dependency SO it can not access dynamic
feature resources and classes. What’s the use you might ask! Don’t worry, I will
explain how to access.

But the interesting part is a dynamic feature has the implementation dependency
for app module and it can access resources and classes of the app module. I
believe Google designed in such a way so your base-module is simply utilities
and common file/resources for the dynamic feature module.

### 3. Dynamic feature module can not have dependency on other dynamic feature
module

### 4. How to Access dynamic feature

As we know app does not a direct implementation dependency on the dynamic
feature. It can not see the classes and resources included in the dynamic
feature. Once dynamic feature us installed (check split manager), the resources
and classes are there. You need to use reflection to access.

**a. Start an activity inside the Dynamic feature module**

The simplest use case of having a dynamic feature is to start some Activity. You
can do this by using the **fully-qualified class name of the Activity class.**

e.g

    Intent().setClassName(BuildConfig.APPLICATION_ID, className)

Here class name is the fully qualified name of the class present in your dynamic
feature.

**b. Access a resource of dynamic feature in the app**

I have seen many times developers asking “I am facing resource not found in
dynamic feature module”

It is a rare use case but if you need to access the resource present in dynamic
feature, you have to reflection again. As you know you do not have access to the
resource identifier in your app R file. You need to use this special syntax:

```
resources.getIdentifier("res_name", "res-type", packageName)
```

[Check getIdentifier method here for more details](https://developer.android.com/reference/android/content/res/Resources)

Let’s say we are looking for a string named “module_description”, we can use

```
resources.getIdentifier("module_description", "string", packageName)

```

The tricky part is the packageName. This is not documented well. The package
name you to use is:

Your base package name + dynamic-feature-identifier-you-used-in-the-build-file

So if your dynamic feature module name is “image_decoder” and your base package
name is “com.gaurav.app”, you have to use

```
resources.getIdentifier("module_description", "string", "com.gaurav.app.image_decoder")
```

if you do not use the correct package name, you will get 0 identifier. This
package name might not match the package name you mentioned in the manifest file
of your dynamic-feature but that’s how you can access it.

### 5. Same string identifier

If you have the same string identifier in app and dynamic-feature and you want
to access the resource in a class in dynamic feature. The value of the
R.string.key depends on the R file you use.

Suppose your dynamic feature package name is

```
dynamic feature package="com.gaurav.features.dynamic_feature_1"
and main package = "com.gaurav.app"
```

then you can access the string as either

```
com.gaurav.features.dynamic_feature_1.R.string.key
        OR
com.gaurav.app.R.key
```

### Summary

Dynamic feature and App bundle is an important concept in the Android world and
both users and developers gain from it. Bundle format is the binary format which
consists of many apks inside. The apks are dependent on the localization, screen
density, CPU architecture, etc. These apks are downloaded in users phone
intelligently by play store. You can use play core library to initiate download
programmatically. You can add more apks by marking your modules dynamic, Android
calls it dynamic features. The app can not access directly dynamic feature
classes and resources but the dynamic feature can access the app resources
directly. The app needs reflection to do so. You can use SplitInstallManager to
install, uninstall and manage a dynamic feature. You need to be sure dynamic
feature is installed before you use it otherwise you will get ClassNotFound OR
ResourceNotFound Exception.

Hope you learned something new.

