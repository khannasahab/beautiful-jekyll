---
layout: page
title: A complete guide on Java-to-Kotlin
subtitle: Learning Simplified
use-site-title: true
part-1: /blogs/programming/java-to-kotlin/part-1/
part-2: /blogs/programming/java-to-kotlin/part-2/
part-3: /blogs/programming/java-to-kotlin/part-3/
part-4: /blogs/programming/java-to-kotlin/part-4/
part-5: /blogs/programming/java-to-kotlin/part-5/
part-6: /blogs/programming/java-to-kotlin/part-6/
part-7: /blogs/programming/java-to-kotlin/part-7/
part-8: /blogs/programming/java-to-kotlin/part-8/
part-9: /blogs/programming/java-to-kotlin/part-9/
part-10: /blogs/programming/java-to-kotlin/part-10/
---

### Preface 

{: .box-bordered}
*I asked a Java developer why he has not tried Kotlin, there are not good resources for Java-to-Kotlin, `he answered and this series was born`*

{: .box-plain}
Our motto from this series is to reduce the friction for java developers to adapt Kotlin. You will not become an expert in Kotlin but definitely you will start appreciating it. At the very least, you will become familiar with the concept and make an informed decision whether to use it ot not. **This series is strictly for Java Developers. All the examples and explanation assumes you know Java OR OOPS.** 

*`Disclaimer : Wherever possible, We will have Java code and similar Kotlin code. I care for you`*

This series is divided into few parts. The logic behind the sequence chosen: Earlier parts show the minimal difference between Kotlin and Java so you can grasp the concept easily and feel confident. The parts at the end show more capabilities and offerings of Kotlin which are either not present in Java OR they are slighty different as a concept.

>Some points to note: <br/>
<br/> 👉 Whatever you do in Kotlin can also be done in Java
<br/> 👉 From language basics perspective, Kotlin is syntatical sugar over Java. I say Java is compiled, Kotlin is smartly compiled.
<br/> 👉 There are some cases where Kotlin is more efficient (the new offerings from Kotlin Platform Classes, we will see later)
<br/> 👉 All the parts of this series are connected, it is better to read in the sequence. Bookmark it now.

*It is going to be a long series, grab your favourite hot drink and be ready to learn something new. If you happen to like the series, promise me you will share among your circle*


## Index

{% include post_card.html 	
title="Part 1" 
description="What and Why Kotlin? Are not you happy with Java? Java is a mature language then why do we even ..."
url=page.part-1
%}

{% include post_card.html 	
title="Part 2" 
description="All about Basics. From Semicolon to Inheritence, From Class to Variables. From Visiblity modifiers to method parameters. From constructor to Constant ..."
url=page.part-2
%}

{: .box-plain}
Topics in Part 2: [Semicolon]({{ page.part-2 }}#semicolon), [File & Package]({{ page.part-2 }}#file-package), [Access Modifier]({{ page.part-2 }}#access-modifier), [Equals ==]({{ page.part-2 }}#equals), [String Concatination]({{ page.part-2 }}#string-concat), [Variable]({{ page.part-2 }}#variable), [New Instance]({{ page.part-2 }}#new-instance), [Class Definition]({{ page.part-2 }}#class-definition), [Inheritance]({{ page.part-2 }}#open-keyword)

{% include post_card.html 	
title="Part 3" 
description="Smart Kotlin shows its magic and help you type less and focus more on the problem statement you are working on ..."
url=page.part-3
%}

{: .box-plain}
Topics in Part 3: [Explicit Cast]({{ page.part-3 }}#explicit-cast) , [Instance Of]({{ page.part-3}}#instanceof), [Smart Cast]({{ page.part-3 }}#smart-cast), [Ternary]({{ page.part-3 }}#ternary), [Functions]({{ page.part-3 }}#function), [Top Level members]({{ page.part-3 }}#top-level), [Multi Line String]({{ page.part-3 }}#multiline-string)

{% include post_card.html 	
title="Part 4" 
description="By this time you should be convinced from the power of Kotlin, This part is dedicated to constructors and inheritance with ..."
url=page.part-4
%}

{: .box-plain}
Topics in Part 4: [Constructors]({{ page.part-4 }}#constructor), [Inheritance & constructor]({{ page.part-4 }}#inheritance)

{% include post_card.html 	
title="Part 5" 
description="It gets interesting with every part. This part focuses on a very important feature of Kotlin: Null Reference and ..."
url=page.part-5
%}

{: .box-plain}
Topics in Part 5: [Null Reference]({{ page.part-5 }}#null-reference), [Elvis ?:]({{ page.part-5 }}#alvis-operator), [Let]({{ page.part-5 }}#let), [Safe Cast]({{ page.part-5 }}#safe-cast)

{% include post_card.html 	
title="Part 6" 
description="Consider this part a break in this series. After 2-3 difficult parts, I thought to take a step back and explain the small but powerful and easy to understand ..."
url=page.part-6
%}

{: .box-plain}
Topics in Part 6: [If Expression]({{ page.part-6 }}#if-expression), [When Expression]({{ page.part-6 }}#when), [Range]({{ page.part-6 }}#range), [Range with Step]({{ page.part-6 }}#range_step), [For In Loop]({{ page.part-6 }}#for_loop), [Labelled Block]({{ page.part-6 }}#labelled-block)

{% include post_card.html 	
title="Part 7" 
description="This part is all about Kotlin new offerings and how powerfull small things can be. It will amaze you and confuse you ..."
url=page.part-7
%}

{: .box-plain}
Topics in Part 7:  [Extension Function]({{ page.part-7 }}#extension), [Data Class]({{ page.part-7 }}#data-class), [Constant]({{ page.part-7 }}#constant), [Singleton]({{ page.part-7 }}#singleton), [Anonymous]({{ page.part-7 }}#anonymous), [Companion]({{ page.part-7 }}#companion)

{% include post_card.html 	
title="Part 8" 
description="Let's deep dive into true kotlin offerings in the area of Lambda and the scoped functions. You will see these everywhere you ..."
url=page.part-8
%}

{: .box-plain}
Topics in Part 8:  [Lambda Expression]({{ page.part-8 }}#lambda) & [Scoped Functions]({{ page.part-8 }}#scoped)

{% include post_card.html 	
title="Part 9" 
description="More utilities and better syntax from Kotlin will impress you and help you to focus more on ..."
url=page.part-9
%}

{: .box-plain}
Topics in Part 9:  [Collection Utilities]({{page.part-9}}#collection_utilities), [Generics]({{page.part-9}}#generics), [Annotations]({{page.part-9}}#annotations)

{% include post_card.html 	
title="Part 10" 
description="Some amazing and mind boggling offerings from Kotlin, it will taks sometime to sink in ..."
url=page.part-10
%}

{: .box-plain}
Topics in Part 10:  [Late Init]({{page.part-10}}#lateinit), [Delegator]({{page.part-10}}#delegator), [Lazy]({{page.part-10}}#lazy), [Coroutine]({{page.part-10}}#coroutine)

{% include post_card.html 	
title="Bonus - Mastering Kotlin Scoped and Higher-Order Functions" 
description=" Learn the fundamentals, master the concept..."
url="https://blog.kotlin-academy.com/mastering-kotlin-scoped-and-higher-order-functions-23e2dd34d660"
%}


{% include buy_me_coffee.html %}