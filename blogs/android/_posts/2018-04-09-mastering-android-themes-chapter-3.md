---
layout: post
title: Mastering Android Themes - Chapter 3
gh-repo: g24khanna/Dynamic-Theme-Example
gh-badge: [star, fork, follow]
categories: [android, ui]
tags: [android, adroid styling, app development]
share_request: true
subscribe_email: true
---


This is chapter 3 of our [Mastering Android themes series](/blogs/android/mastering-android-themes). If you have not read
[Chapter 1](/blogs/android/ui/mastering-android-themes-chapter-1)
OR [Chapter 2](/blogs/android/ui/mastering-android-themes-chapter-2),
I recommend you to spend 10 minutes.

> In chapter 2, we agreed on strong need of a good design languge.Our expert
> developers make sure to define a good terminology with designers for ease of
communication and implementation. Lets master themes.

![](https://cdn-images-1.medium.com/max/1600/1*Qjptdilv42CsLCrnoO_YaA.gif)

#### One example of resource defined with design language from last chapter:

Color.xml

    <resources>
    <color name="
    ">#d2d2d2</color>
    <color name="
    ">#FFFFFF</color>
    <color name="
    ">#000000</color>
    </resources>

Dimen.xml

    <resources>
    <dimen name="
    ">22dp</dimen>
    <dimen name="text_size_h2">20dp</dimen>
    <dimen name="text_size_h3">18dp</dimen>
        <dimen name="
    ">8dp</dimen>
        <dimen name="text_content_margin">16dp</dimen>
    </resources>

Usage in TextView

    <TextView
        android:id="@+id/heading"
        android:style="
    />

If you see in above resources, colors and dimensions are given name as per
context and theme used by your designer(White and Grey with black text in this
case). If you have hardcode values or use semantic names in your project, like
padding_16, please read chapter 1 and chapter 2.

#### Out of the box Theme in Android

I assume you know about material design and three colors we assign in every app,
**primary, parimaryDark, accent.** What are these? Short answer: Theme
Attributes.

These are colors used by system to paint standard components.Example, toolbar
will get **primary** color, system bar will get **primaryDark** color and views
which offer feedback may or may not use **accent.** Whether to respect these
colors or not depends on view implementation. All system components respect
material theme attributes. You can write your own component, eg custom toolbar
which does not respect these colors.

> In simple words these are **attributes** of a theme. Attributes are properties
> given name and values. You can also write your own attributes as well.

From programming world, you can relate attributes to **constants**. Let me show
you how.

```
<item name="colorPrimary">#303F9F</item>
```

Here, colorPrimary is a member variable of the applied theme whose value you
assigned 303F9F. Think theme as immutable object whose properties you can not
modify at runtime. So colorPrimary is fixed to 303F9F.

Similar to these three colors, there are many more properties which are part of
a theme like **windowBackground**, **windowActionBarOverlay****,
****textColorPrimary****, ****textColorSecondary**** and **so on**. **Remember
these attributes are defined by Android system and the native components like
TextView read values from these properties so you get the color of your choice.
Every view you use in your layout is interested in some of these attributes. For
extensive list ofall possible attributes, read [themes.xml
here](https://chromium.googlesource.com/android_tools/+/25d57ead05d3dfef26e9c19b13ed10b0a69829cf/sdk/platforms/android-23/data/res/values/themes.xml).

Now we know custom theme is equivalent to assign some values to attributes which
views will use at runtime to draw themselves. Lets see example of one custom
theme.

```
<style name="IndigoTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">@color/indigo</item>
    <item name="colorPrimaryDark">@color/indigoDark</item>
    <item name="colorAccent">@color/indigoLight</item>
    <item name="textColorPrimary">@color/black</item>
    <item name="textColorSecondary">@color/gray</item>
</style>
```

IndigoTheme is extending DarkActionBar theme from system and overriding few
attributes, rest will be given default values which you can check inside
DarkActionBar theme. We can apply this theme to application or an activity.

#### Extending theme capability — custom properties

Sometimes, we want to define some properties correspond to domain problem we are
solving. For example, your app needs to show connectivity state to user all the
time and you want to define 2 colors for connected and not connected. Our novice
or competent developers will use selectors with hardcode values or semantic name
values. But we are expert now and we will be master soon. We will use **custom
attributes**. Example:

attr.xml

```
<attr name="textColorConnected" format="reference"/>
<attr name="textColorDisConnected" format="reference"/>
```

And we will define these values in our theme

```
<style name="IndigoTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">@color/indigo</item>
    <item name="colorPrimaryDark">@color/indigoDark</item>
    <item name="colorAccent">@color/indigoLight</item>
    <item name="textColorPrimary">@color/black</item>
    <item name="textColorConnected">@color/green</item>
    <item name="textColorDisConnected">@color/light_gray</item>
</style>
```

Now, in our views or selectors, we can use custom attributes. It feels close to
domain problem and design language plays its role. Example of usage:
```
<TextView
    android:id="@+id/heading"
    android:style="@style/heading_text_style
android:textColor="?
/>
```

Notice we refer a custom attribute with **?customName OR ?attr/customName
**syntax**. **Similarly if you want to refer some attribute defined by android,
you can use **?android:attr/name_of_attr** and style your custom components with
theme attributes. Example:
```
<SomeCustomView
 app:custom_attr=”?android:attr/activatedBackgroundIndicator”
/>
```

Here we used our custom attr name but we ensured value comes from system defined
attribute which developer can customise while extending theme.

### And you are theme expert from now onwards.

> Summary of chapter 3

You understood theme in very simple form, a model object which carries
properties used by views at runtime. You saw power of custom attributes aligned
to design language and how can we refer system attributes for our custom
components.

The only part left is how to change themes dynamically. How can I offer 5 themes
in app and let user choose which color he wants. We will explain rest in last
chapter. Are you ready to become master of Android themes?


#### [Click here to read last Chapter](/blogs/android/ui/mastering-android-themes-chapter-4).
