---
layout: post
title: Android UI/UX Tips & Tricks
subtitle: Master the Basics
categories: [android]
tags: [android, adroid internal, app development]
share_request: true
subscribe_email: true
---

Most of the times we developers do 99% of the Job and push it in the user's
hand. The last 1% is a small number but really important. That, my friend, is
UI/UX polishing and subtle feedback to users. Remember, the difference between a
winner and a loser is that extra 1% effort.

This article aims to share some tips and tricks so you can develop little better
UI/UX. Basic understanding of Android UI and XML is required. I hope you learn
at least 1 new thing from this article.

Let’s agree Android has improved a lot in past some years. There are many good
things (Yin & Yang) in Android and XML are one of them. Creating UI with XML and
having a real-time preview is really nice.

![](https://cdn-images-1.medium.com/max/1600/0*au4JkNU9SEvs2Jqg)
<span class="figcaption_hack">Image from Pexels</span>

#### One of the most important rules is UI feedback to the user.

Imagine you press the light switch in your home and nothing happens. What will
you think? You will imagine the power supply is gone OR it is a dead switch. At
the very least you will be confused. It is supposed to work. That’s how your
mind is trained.

Exactly the same happens when user taps on something in your app and nothing
happen. He is confused and does not like it when unusual behavior pops up. There
are plenty of ways Android offers you to give feedback to the user.

> XML in Android are really powerful and neat way to manage UI layer. We will
> discuss a few tips in this article.

### Delight == Good UI + Good UX

Not everything is your app is clickable/selectable/checkable/**actionable. **If
something is, it better behave that way. There is a notion of states in Android
Views. ‘State_Pressed’ ‘State_Checked’, ‘State_Selected’, ‘State_Enabled’ are
few examples.

#### Good UI

It is always a good practice to have reactive UI based on these states. It feels
more natural to user and absence of them is surely irritating at times. Let’s
see how can you alter state based UI with minimal efforts.

#### 1. Ripple Effect (State_Touched)

From Material Theme onwards, Ripple is the Android way of showing something is
actionable. When a user touches an actionable view, the view should show Ripple
effect. By default, almost all clickable views show this effect but sometimes
you need to customize UI and all of sudden, ripple is gone. Here is the magic
keyword to get it back.


    If your View does not have a background, you can use the background variant. Sometimes, you need a custom background and creating Ripple seems a heavy task. You can use the foreground variant.

#### 2. Themed View components

Many times you need a custom color in components like Button, RadioButton,
Checkbox etc. I have seen naive developers using drawables to show various
states (checked, clicked, selected …). Remember, try not to deviate from the
system as much as possible. Always leverage platform offerings. Here are some
quick keywords to show system components with custom theme colors with default
behavior.

    Button with custom colour
    -----
    Radio Button with custom colour
    -----
    Images & Icons
    -----
    ProgressBar with custom colour

The advantage of using these properties is you get your themed look and feel
with system behaviour and UI with states (Ripple, Shadow etc).

> Note: It is also possible to create those states with Custom selectors and your
> own drawables. My recommendation is to go this route if system does not fulfil
your requirements.

#### 3. Shadow below components

The magic keyword to show a subtle shadow below components like cardview is to
use elevation property.


> Tip: Sometimes elevation property has no effect when it is the last View in your
> hierarchy. You might need to add a dummy last view.

#### 4. merge tag and parent property

If you want more control over UI and manageable XML with time, start using merge
tag. Create separate XML for small UI components and include all those reusable
XML in your main UI. It increased readability and nothing else matters. At
compile time, tools can take care of merging and it has a negligible impact on
your app performance.

Here is one example, 3 XML are merged in one using include. Give it a shot, you
will like.

    <LinearLayout>

    <include layout="@layout/light_expanded_card" />

    <include layout="@layout/light_collapsed_card" />

    <include layout="@layout/color_indicator_strip" />
    </LinearLaout>

And yes, you can see the preview of your XML using parentTag property. Just
change it to any ViewGroup and you will see a different outcome in the preview.
It is plain good.

    <
     xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
        
    ="android.widget.LinearLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

> Question: Suppose you have to show same UI in two forms Card form and without
> card form, how will you create those 2XML without merge tag?

#### 5. ViewStub

If you don’t need it. Don’t inflate it. Though devices are powerful these days,
it does not make wastage any good.

Use ViewStub for conditional views which you need very rare. ViewStubs are not
inflated with your XML. You can use the power of XML and layouts with
optimization of Lazy inflation. [Read
here](https://developer.android.com/training/improving-layouts/loading-ondemand)
for more.

#### 6. Width/Height — Designer-Developer Dilemma

Remember two things. Always develop a flexible UI and never hardcode dimensions
unless it is the only way. Most of the times, your designer will ask you to make
your views fixed width-height to ensure it looks pixel perfect. It is not their
mistake, try to understand what they want and use your Android knowledge to
implement it in the right way.

One good example if min_width, min_height, max_width and max_height attributes
over width and height attributes. My experience says your designer mostly wants
min_width and min_height when they speak about width and height. With fixed
width OR height, you might show a good demo inside team but there are a lot of
chances it might break in the real world. With min-width and min-height
attributes, you ensure the UI components look good and cater mobile devices +
user preferences like Big Text.

#### 7. Text Size dp vs sp

If you use define text size in dp, you are not doing it right. If you define
text size in sp, you are not doing right either. Actually, there is no right OR
wrong answer here. I have interviewed a lot of candidates and trust me,
beginners always do this mistake. They use either dp OR sp in the whole app.

dp is ‘density independent pixels’<br> sp is ’scale independent pixels’

Explaining dp and sp is out of the scope of this article. sp is dp * scaling
factor. Android allows the user to set the text size as per her preference.

![](https://cdn-images-1.medium.com/max/1600/0*iUBmlXWiMaLdTWHu.png)

There is a scaling factor associated with each size. For the sake of example,
let’s say the medium is 1, large is 1.5, small is 0.8 and micro is 0.5

So 100 dp and 100 sp are same when the user has not changed this setting,
otherwise

sp = dp * scaling factor

**Coming back to question, which one to use when?**

Sit with your designer and ask him/her: Which text they do not wish to change
even when the user has set different scaling factor? Remember, the font is very
important and there are cases when some font size has to be exactly the same as
design demands otherwise it will f*** up whole UI. In those cases, use dp. Ask
your designer which text can flow the way the user wants and use sp there.

Most of the time, restricted texts like Single Line Heading, Error Banners, text
with Images and Icons are part of the design and they should use dp. Free flow
text like description, info text is not the part of the design and they must use
sp.

> **However, few people might debate to use sp with text always. I recommend
> discuss it with your designer. He/She knows better**

#### Good UX

In the last section, we saw state feedback is one of the ways to guide the user.
Another important feedback and delight mechanism is subtle animations. There are
many ways you can animate views in Android. But this article is about tips and
tricks. Following are the ways you can animate with very little code.

#### 1. Activity Transition

With one line you can facilitate Activity transition.

    overridePendingTransition(android.R.anim.
    , android.R.anim.
    );

#### 2. Animate Layout Changes

This is by far my favorite trick in Android. Almost all ViewGroup respect this
attribute. With this one line, you can improve your UX a lot. Here is the magic
line:


This is called LayoutTransition and whenever there is any change in the View
hierarchy, it will be animated. Common operations like Add View, Remove view are
handled with default animations.

Yes, there is Java equivalent as well.

    TransitionManager.
    (
    );

After this line, all changes made to children of this ViewGroup will be
animated. You can combine multiple changes to view hierarchy with this variant.
Of-course, you can create [custom
transition](https://developer.android.com/training/transitions/custom-transitions)
effects.

#### 3. State Change with animation

In the last section, we saw state changes and feedback to the user. What if you
want to animate with state change? Let’s say you want to increase the button
width and height by 20% whenever the user clicks. This can easily be done with
XML and no code is required.

You can use selectors. Create a file “animate” in the xml folder.

    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <!-- the pressed state; increase x and y size to 150% -->
        <item 
    >
     <objectAnimator android:propertyName="scaleX"
                    android:duration="@android:integer/config_shortAnimTime"
                    android:valueTo="1.5"
                    android:valueType="floatType"/>
    </item>
    ... Similarly for other states_
    </selector>

You can set this to any View in XML

    <Button 
    ="@xml/animate"
    ....

#### 4. KeyFrame and PropertyValueHolder for complex animations

Sometimes we do not have a simple animation like scale, rotate to perform.
Complex animations can easily be obtained with Keyframes.

Check this [flipbook animation](https://www.youtube.com/watch?v=SGw6VREYkLE). In
flipbook animation, each page has a drawing with some values tweaked so when it
is played fast, our eyes perceive it as animation. That’s how Walt Disney
created his magic.

In the animation world, a keyframe is a frame with some values tweaked. A
keyframe is one of the pages of that flipbook. Don’t worry we are not going to
animate cartoons (though we can).

What if you have to play a simple heartbeat animation? Think about flipbook.

If it has to be played in 1 second, you can imagine a timeline and heart effect.

Time 0, normal<br> Time 1/4, heart size decreased by some fraction<br> Time 3/4,
heart size increased by some fraction<br> Time 1, heart size is back to normal

Here are 4 keyframes expressing what we described.

    Keyframe k0 = Keyframe.
    (0f, 1f);
    Keyframe k1 = Keyframe.
    (0.275f, decreaseRatio);
    Keyframe k2 = Keyframe.
    (0.69f, increaseRatio);
    Keyframe k3 = Keyframe.
    (1f, 1f);

Now we have these keyframes, we have to apply them to View Property. We can use
PropertyValueHolder.

    PropertyValuesHolder scaleX = PropertyValuesHolder.
    (
    , k0, k1, k2, k3);

It says, to whichever view this is applied, change the x as per these keyframes.

    PropertyValuesHolder scaleY = PropertyValuesHolder.
    (
    , k0, k1, k2, k3);

Same for Y.

Now, We can create ObjectAnimator using value holder.

    ObjectAnimator beat = ObjectAnimator.
    (viewToAnimate, scaleX, scaleY);
    beat.start();

It will return an animator which will change the object (here view) properties
as per the key frames stored in these value holders.

### Summary

Feedback is very important. Either you are guiding the user by showing state
feedback OR you are trying to delight users with animations. With tricks, it
does not need that much effort to take your app from 99 to 100%.


