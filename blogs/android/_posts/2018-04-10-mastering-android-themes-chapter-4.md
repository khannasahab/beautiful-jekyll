---
layout: post
title: Mastering Android Themes - Chapter 4
gh-repo: g24khanna/Dynamic-Theme-Example
gh-badge: [star, fork, follow]
categories: [android, ui]
tags: [android, adroid styling, app development]
share_request: true
---

This is chapter 4 of our Mastering Android themes series. If you have not read
[Chapter 1](/blogs/android/ui/mastering-android-themes-chapter-1),
[Chapter 2](/blogs/android/ui/mastering-android-themes-chapter-2)
or [Chapter 3](/blogs/android/ui/mastering-android-themes-chapter-3).
I recommend you to spend 10–15 minutes.

> In chapter 3, we agreed on using design langauge and creating custom attributes
> aligned to design language. We understood theme is equivalent to an object
filled with values used by view components at runtime. It is time to master
themes.

![](https://cdn-images-1.medium.com/max/1600/1*Qjptdilv42CsLCrnoO_YaA.gif)

By now, you must be able to create a good material theme and your designers must
be proud of your implementation details. How about jumping to dynamic theme?

#### Dynamic Theme

Let me ask you a question, try to write the answer and match later. Suppose you
are creating a compound viewgroup which plays role of an utility in your project
and you have a title bar filled with primaryColor of your theme. How will you do
it?

Think. Write and then proceed.

Many developers will write this:

```
<FrameLayout 
    width, height ... other properties
    android:background="@color/colorPrimary">
</FrameLayout>
```

If you hardcoded color code, start from chapter 1 again. If you created custom
style and applied titleBarStyle with min height and other common properties,
bravo!

But we had learnt this topic in last chapter. Aren’t we here to learn dynamic
themes? Yes we are! The above solution is correct when you have only one theme,
it wont work in multi theme scenario. What could be solution? You can guess,
**custom attribute**.

attr.xml
```
<attr name="themeColorPrimary" format="reference"/>

Usage

<FrameLayout
    width, height ... other properties
    android:background="?themeColorPrimary">
</FrameLayout>
```

We defined an attribute **themeColorPrimary** and we are using it. The attr tag
states this attribute is a reference whose value is defined somewhere else. If
you use this **attr** without defining its value, your app will crash. So where
do we define it? I guess theme is a good option.

How about our custom theme.

Theme 1 : Indigo
```
<style name="IndigoTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">@color/indigo</item>
    <item name="colorPrimaryDark">@color/indigoDark</item>
    <item name="colorAccent">@color/indigoLight</item>
    <item name="textColorPrimary">@color/black</item>
    <item name="textColorConnected">@color/green</item>
    <item name="textColorDisConnected">@color/light_gray</item>    
    <item name="themeColorPrimary">@color/indigo</item>
</style>
```

Theme 2: Pink
```
<style name="PinkTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">@color/pink</item>
    <item name="colorPrimaryDark">@color/pinkDark</item>
    <item name="colorAccent">@color/pinkLight</item>
    <item name="textColorPrimary">@color/black</item>
    <item name="textColorConnected">@color/green</item>
    <item name="textColorDisConnected">@color/light_gray</item>    
    <item name="themeColorPrimary">@color/pink</item>
</style>
```
Now you can simply apply these themes dynamically:

```
setTheme(R.style.PinkTheme);

OR

setTheme(IndigoTheme);
```
<br/>

> Tip: You need to call **recreate()** method in activity to ensure activity
> recreates itself and lifecycle is called again, so you can inflate your xml with
new theme in **onCreate()** method.

Wasn’t that simple? However I see a small problem there. Rule number 1 of
programming, duplicacy is bad! It leads to future maintenance issues and we just
left duplicacy in above themes. **colorPrimary** and **themeColorPrimary **both
are assigned same value. Being master, we can not tolerate such flaws. How about
this?

Theme 1 : Indigo
```
<style name="IndigoTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">?themeColorPrimary</item>
    ...
     ....
    <item name="themeColorPrimary">@color/indigo</item>
</style>
```

Theme 2: Pink
```
<style name="PinkTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">?themeColorPrimary</item>
    ...
     ....
    <item name="themeColorPrimary">@color/pink</item>
</style>
```
Woo! Now colorPrimary is referring to our custom atribute. Thats awesome!

Do you still see scope of improvement here? Write down your answer before
reading further.

And the answer is

*****

Yes, I do. We just cleaned our xml little bit and removed duplicacy but I still
smell duplicacy which can be avoided easily. I see

```
<item name="colorPrimary">?themeColorPrimary</item>
```

at two places(in both themes). Why not make it even better!

How about this?
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">?themeColorPrimary</item>
</style>
```

Theme 1 : Indigo
```
<style name="ThemeIndigo" parent="AppTheme>
    <item name="themeColorPrimary">@color/indigo</item>
</style>
```

Theme 2 : Pink
```
<style name="ThemePink" parent="AppTheme>
    <item name="themeColorPrimary">@color/pink</item>
</style>
```

We just created a parent themes and extended two themes from it. We understand
real user of Inhertitance. We truly are a master.

Note: You could also extend style with dot operator. Example:
```
<style name="AppTheme.ThemeIndigo">
    <item name="themeColorPrimary">@color/indigo</item>
</style>
```

It is same as mentioning parent explicitly.

Needless to say, you should be able to create awesome themes by now. Both static
and dynamic. Cheers!

### We have come a long way. I bow to you, Master.

> Summary of all chapters

We started our journey with hardcoded values and then we moved to semantic
same-name values like padding_16. We saw lot of scope of improvement in novice
style, so we invited competent developers who suggested us to maintain styles
and sleep well.

We were sipping coffee with satisfaction and by then, expert developer coached
us to design language. He explained use of design terminology with designers and
how to ensure a proper communication takes place with in teams. He is a real
team player and collaborator. He introduced custom attribues and themes to us
and we learnt a theme is just an object filled with values so views can use it
to draw at runtime. We learnt how to create a theme by extending Material themes
offered by system.

But we are hungry developers, we wanted to become master. We saw how to use
custom attributes in custom components and how to create themes hierarchy. We
created our custom themes with common parent and used custom attributes to fill
values. Finally we felt proud writing awesome code.

Remember, a master not only writes good code; he spreads the knowledge also.

### [Git Repo for Dynamic Theme Sample](https://github.com/g24khanna/Dynamic-Theme-Example.git)