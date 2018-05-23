---
layout: post
title: Mastering Android Themes - Chapter 2
gh-repo: g24khanna/Dynamic-Theme-Example
gh-badge: [star, fork, follow]
categories: [ android, ui ]
tags: [android, adroid styling, app development]
share_request: true
subscribe_email: true
---

This is chapter 2 of our [Mastering Android themes series](/blogs/android/mastering-android-themes). If you have not read [Chapter 1](/blogs/android/ui/mastering-android-themes-chapter-1/), I recommend you to spend 5 minutes.

From chapter 1, we agreed on we will not remain novice and hardcode anything. We will not write semantic same names for the sake of no-hardcoding and we will use styles for reusability. Lets see how our expert developer improves the last approach from competent peers.

{: .box-note}
Note: This chapter focuses on theme basics along with design language. If you want to learn only about themes, feel free to jump to next chapter. However, I recommend to read it.

![](https://cdn-images-1.medium.com/max/1600/1*Qjptdilv42CsLCrnoO_YaA.gif)

### Developer level - Expert

Expert developers feel proud in writing code/xml in the best possible way as per their scope of requirement. When it comes to styling, they introduce the magic ingredient, design language.

#### Design Language

Remember our competent developer used styles either with hardcoded values or semantically same name values. 
Fast-Forward:
~~~
<style name="heading_text_style">
    <item name="android:padding">8dp</item>
</style>
OR
<style name="heading_text_style">
    <item name="android:padding">@dimen/padding_8</item>
</style>
~~~

Problem with former approach is there is no common ground between designers and developers. They do not understand each other contextually and developers tend to reuse same resource reference at multiple places which can lead to maintainence issue in future. For example, Changing padding in all your content cards from 8 to 16 is not very easy because you are referring to padding_8 which is being used at other places like window padding. Tooling might help little bit, nonetheless it is difficult to maintain and there is scope to improve readability.

Expert developer understand the key to a successful project is communication between teams. Putting other way, A project is bound to fail because of miscommunication or vague communication. Design and development team need to communicate every single day. Consider following 2 discussions.

>Discussion 1:
>Designer to developer: lets make the text in cards black, margin 16dp between items on home page because that is content and make padding 8dp on borders because we have standard 8dp spacing around window. 
>Competent++ examples on chapter 1 fall in this category.


>Discussion 2:
>Designer to developer: change the text to 'content_text_color', margin between items to 'content_margin'
>and padding on borders to 'window_padding'. All of these values are in our design document we shared.

``1 vs 2``
<br/>
Discussion 1 is an example of premature design and dev communication who interact with exact values and there is atmosphere of chaos every-time. Developers do not understand the rationale and it is error prone.
However, discussion 2 is mature and subtle. There is no confusion. There is a reference document and very less possibility of error. Developers can create their resource file from design language and they use same naming convention. Let me show you one example.

Usage in xml
```
<TextView
    android:id="@+id/heading"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"       
    android:textSize="@dimen/text_size_h1"
    android:padding="@dimen/text_content_padding"
    android:textColor="@color/content_text"
 />
 ```

Usage in styles

```
<style name="heading_text_style">
    <item name="android:textColor">@color/content_text</item>
    <item name="android:textSize">@dimen/text_size_h2</item>
    <item name="android:layout_margin">@dimen/text_content_margin</item>
    <item name="android:layout_width">match_parent</item>
    <item name="android:layout_height">wrap_content</item>
</style>

With

<TextView
    android:id="@+id/heading"
    android:style="@style/heading_text_style"
/>
```

Using above mentioned approach, it becomes breeze to communicate and implement design with every new feature. It will speed up your team 2x.

Simply put, Design language is how your design and development team communicates. Similar to software design patterns(Singleton, Factory etc) facilitate communication between developers, design language does between designers and developers.

{: .box-note}
Now, we agreed on design language is crucial aspect of a project. We will use it in every project. Promise.

Who will define design language? My recommendation is designers should come up with a design language as they have understanding of all the components used in a product. However, they should sit with developers to come up with a common terminology based on the platform they are working on. Don't boss around. No one likes it. Play a good teammate.

### Theme & Style

Styling your user interface means assigning color codes and dimensions to various properties. Like text color, size of icon, text size, background of screen etc etc.

Style is a reusable form of collection of resources. By the way, A theme is also a collection of resources which modifies look and feel of your product. The minor difference is you apply theme on a bigger level, say a screen(Activity) and you usually apply style on a granular level, say a view. The difference is so subtle that there is no theme resource in Android. There is only style resource. You apply theme with style tag.

```
android:theme="@style/ThemeGrey"
```

**Ladies and Gentlemen, you just created your very first theme in above examples.** Check next chapter for details.

#### Summary of chapter 2

Expert are experienced competent developers who understand the power of common design language which facilitates communication, readability and maintainability. They also create styles whenever needed and collaborate with designers for a design language. They define styles which act as theme with strong terminologies being used.

**[Click here to read Chapter 3.](/blogs/android/ui/mastering-android-themes-chapter-3/)**