---
layout: post
title: Mastering Android Themes - Chapter 1
categories: ['android','blog']
gh-repo: g24khanna/Dynamic-Theme-Example
gh-badge: [star, fork, follow]
categories: [android, ui]
tags: [android, adroid styling, app development]
---

Who does not want to style their Android app? Unfortunately there are very few resources available online to learn styling and theming. I thought to share something simple yet powerful.

{: .box-note}
For the sake of this series, lets divide developers in 4 categories. Novice, Competent, Expert and Master. In this series, we will explain the path an Android developer takes from novice to master and we will make you master today.

#### Index
* Chapter 1 (Novice and Competent): You are reading
* Chapter 2 [(Expert - Design Language)](/android/ui/mastering-android-themes-chapter-2/)
* Chapter 3 [(Themes)](/android/ui/Mastering-Android-Themes-Chapter3/)
* Chapter 4 [(Dynamic Themes and Conclusion)](/android/ui/Mastering-Android-Themes-Chapter4/)
* [Sample Github Repository]({{site.techgit}})

<br/>
#### Preface
Our motto from this series is to master Android themes. At the end of this series, you should be able to write an app with more than 1 theme and dynamic theme as well. Along the way you will learn various ways to style an Android app with reasoning of what is best. We have shared a sample git repo with example of dynamic themes. Recommend you to open after last chapter.

Make sure you read all chapters to connect the dots. We have put effort to keep it simple and fun. Grab a coffee, tea, wine, beer, kahwah and lets get started.

## Novice
These developers are new to Android ecosystem and they just want to get things done. When it comes to styling, these developers follow the very basic approach which is Hard coding (either in code or xml). Example:
~~~
<TextView
    android:id="@+id/heading"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:padding="10dp"
    android:textColor="#000022"
 />
 ~~~

{: .box-error}
Conclusion In three words, Never do it.

!IMHO, This is the worst form of coding practice. Biggest challenge with this approach is maintainability and readability of code/xml. If your designer asks to change text colour or padding at everyplace, you will have to replace in all files (Yes you can use tools for that but makeup for an ugly child does not make him pretty). Remember styling is more than just text colour or padding. Every time you hardcode, you reserve headache for future.

>**Lets agree on we will not hardcode anything and we reach the next level, Competent.**

## Competent
Every competent developer was novice once and he learnt the hard way. He knows hardcoding is a big no-no. So what does he do? Simple, he does not hardcode. He maintains separate files for resources: colors, dimens and so on. He refers them wherever he wants and maintenance becomes better. Lets see a small example.
color.xml
~~~
<resources>
<color name="primary">#222B2B</color>
<color name="primaryDark">#222224</color>
<color name="white">#FFFFFF</color>
<color name="black">#000000</color>
</resources>
~~~
dimen.xml
~~~
<resources>
    <dimen name="padding_16">16dp</dimen>
    <dimen name="padding_12">12dp</dimen>
</resources>
~~~
Usage in xml:
~~~
<TextView
    android:id="@+id/heading"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:padding="@dimen/padding_16"
    android:textColor="@color/black"
 />
 ~~~
It is not perfect solution but it works for most of use cases.

!IMHO, this is not good either. In-fact it is equivalent of hardcoding 16dp or #000000, except one benefit you get, single change reflects everywhere. Overall, I find it of no use. It is like going to a gym followed by drinking beer with burgers and eating cake in dessert. You think you did a good job, reality is at your belly. Do you see any problem in this approach?

What if your design changes and you need to make the standard padding 12dp instead of 16dp OR your content text font changes from black to dark gray. How will you handle this? If you goto each file and change padding_16 with padding_12, you are back to maintenance hell. If you use tooling and replace all padding_16 with padding_12, be ready for unintended changes and donald-trump face of your designer, believe me it is no good. Neither of them.

{: .box-error}
Conclusion In three words, Never do it.

This is surely better than hardcoding but it does not solve the purpose. Styling should be easy to read and easier to maintain.

>**Now, we agreed on we neither hardcode anything nor use semantic same names for the sake of no-hardcoding. We reach the next level, Competent++.**

## Competent++
These developers are already competent in their craft. They have experience.They know good practices and they believe in writing maintainable and readable code/xml. Likewise former peers, they do not hardcode anything. They believe in writing reusable styles and apply wherever needed. Consider following example:

Styles.xml
~~~
<style name="heading_text_style">
    <item name="android:textColor">@color/black</item>
    <item name="android:textSize">18dp</item>
    <item name="android:layout_marginLeft">8dp</item>
    <item name="android:layout_marginRight">8dp</item>
    <item name="android:layout_marginTop">4dp</item>
    <item name="android:layout_marginBottom">4dp</item>
    <item name="android:layout_width">match_parent</item>
    <item name="android:layout_height">wrap_content</item>
</style>
~~~
OR
~~~
<style name="heading_text_style">
    <item name="android:textColor">@color/black</item>
    <item name="android:textSize">@dimen/size_16</item>
    <item name="android:layout_marginLeft">@dimen/margin_8</item>
    <item name="android:layout_marginRight">@dimen/margin_8</item>
    <item name="android:layout_marginTop">@dimen/margin_8</item>
    <item name="android:layout_marginBottom">@dimen/margin_8</item>
    <item name="android:layout_width">match_parent</item>
    <item name="android:layout_height">wrap_content</item>
</style>
~~~
Usage of style:
~~~
<TextView
    android:id="@+id/heading"
    android:style="@style/heading_text_style"
/>
~~~

Now similar to heading_text_style, we can have many styles in our app and apply wherever needed. If your design demands change, you goto styles.xml and update the style. How does that sound? To me, for sure it sounds better. It is readable and it is maintainable(okay, to some extent). Do you see any problem in this approach? Think yourself and goto Chapter2 to cross verify.

{: .box-error}
Conclusion In three words, Never do it.

This approach is good but no where close to perfect. We will explain details in next chapter.

### Summary of Chapter 1
To grasp themes and the concept behind, we needed to understand the alternatives well. In this chapter, we saw various ways of styling your app and none of the above mentioned way is a good candidate for a quality app. They all do the task at hand, but masters always follow the best possible path. We already saw journey from Novice to competent to competent++.

* Novice hardcode and get things done. Never do it.
* Competent are against hardcoding, they maintain proper files and refer resources. Still, Never do it.
* Competent++ take a leap and maintain styles, they solve problem of readibility and maintainability to some extent. Most good developers fall under this umbrella. It has lot of scope for improvement.

### Conclusion
>Novice < Competent < Competent++ < Expert < Master; Lets see what expert and a master has to offer in next chapters.

{: .box-note}
Share the article if you like it and follow me for the updates.

[Click here to read Chapter 2.](/android/ui/Mastering-Android-Themes-Chapter-2/)