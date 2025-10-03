---
title: True Randomization in an Introductory Statistics course
tags: PreTeXt R Inquiry
author: Tien Chih
<script src="https://sagecell.sagemath.org/static/embedded_sagecell.js"></script>
<script>sagecell.makeSagecell({"inputLocation": ".sage"});</script>

---

For about a decade now, I've been teaching on and off some form of introductory statistics for a wide variety of students across different institutions.  This is an immensely popular course at the undergraduate level which should come as no surprise.  The ability to analyze data and draw some basic inferences and conclusions is incredibly important across numerous disciplines.  Students have a wide variety of interests, goals and prior experience, and not everyone comes to this course with a love or experience with technical mathematics.


Those who have taught some form of introductory statistics will recognize that the mathematical aspects of this course pose one of the greatest barriers to effective instruction.  It's worth it to take a moment and acknowledge that conceptually, the ideas in intro stats are hard!  The central idea that distinguishes statistics from other branches of mathematics is the notion of randomness and uncertainty, and this can be a difficult concept to illustrate.  In other math courses like Calculus, one may present or have students work through an example to elucidate some concept.  However when dealing with uncertainty and probabilities, any single example or instance of a random variable would be insufficient to highlight the essential behavior.  A lot of the relevant behavior only manifests after dozens, or hundreds, or thousands of occurences.  More to the point, any specific example is, by it's very nature *not* random, and in many ways defeating the whole point!  (This is a pet peeve I have with *every* intro stats textbook I've ever read, including very good ones!)

Luckily, with the use of technology, we can use randomization and simulation via languages like [R](https://cran.rstudio.com/index.html).  As mentioned in my [previous post](https://mathtech.org/2025/03/05/activelearning.html), this is also an oppourtunity to introduce active or inquiry elements into the course!

For example in [this activity](Exploration 3.5.1: Flip Many Coins 
https://tienchih.github.io/tbil-stats/Sec_NormalApprox.html#exploration-flipmanycoins) from my IBL based statistcs course, we are easing ourselves into discussing the Central Limit Theorem.  The CLT is one of those results that only makes sense at scale.  Even restricting to the binomial case, typically \(n=30+\) trials for the binomial variable would have to be observed in order for the CLT to reasonably apply.  But to see that the distributions of this variable is normal, one needs many many more instances of these \(30+\) trials, far too many coins than can be flipped in class!  However, with [R](https://cran.rstudio.com/index.html), this is no issue.



<iframe
  width="100%"
  height="500px"
  src="https://sagecell.sagemath.org/?z=eJxFjsEKAiEYhO-C7_Aff0mW38BL4LEXiHoA11xWclV0O0XvnlFbtxlm-GbSfZm9vTYwUH1BkqCISHDmckhTDKUZRZxxNuWKAUICdfhF4sEZQPoizCZ2zS4lenSoJJD4IOWbH63zfel8uhwllJrHbhzSoHtv0EIAZ0_O5tBW3GASxurtrZm97q9KDWnF_4EXVTI5NQ==&lang=r&interacts=eJyLjgUAARUAuQ==">
</iframe>

In the above [R](https://cran.rstudio.com/index.html) cell, we flip <c>coinflips = 10</c> coins, record the number of heads, and repeat this \(1000\) times, and plot the distribution of the outcomes in a histogram.  The students are then prompted to repeat this for increasing values of <c>coinflips</c>,  \(20, 30, 50, 100, 200\) etc..  One can observe then that the histogram conforms to a normal curve-like shape.  By editing the <c>prob = c(0.5, 0.5)</c> vector, one can adjust this experiment to any binomial variable, and regardless the normal curve manifests itself.  We can observe all this without stating the CLT, and students may even conjecture as to the general principle at play here.  We may then further illustrate this idea by superimposing the curve on the histogram:

<iframe
  width="100%"
  height="500px"
  src="https://sagecell.sagemath.org/?z=eJxNT0tOwzAQ3VvyHUZZjYtBTqVukGbJBRAcwLFd1ap_2AkUIe6Oo9Cqu_m8b1riyWnbgKC6gkrCqJQSnJns0zH40qgfOCukng6ccRbp9tkVztpsqX3UGe-uOxwfixAr-pgrevAJxucbQPxwBpD-jek6PDQdS3BocJSgxBak5-mxgjauB3x7fX-RUGqe-mKwdMjqA5z9cnbybcarloSpOn1utD9sBFq5a6ulfjq0KdeIFwnR6URRQuslZts9TQ40WF3PU1jcICF8WdpL0NbS5v6tLzMNaehapfp011v8AXQPZR8=&lang=r&interacts=eJyLjgUAARUAuQ==">
</iframe>

The students can confirm their conjecture, and by adjusting <c>coinflips</c> and <c>p</c>, they can verify the result for other binomial variables.  A proof of the CLT is way way beyond the scope of an intro stats class, but using these simulations, students can still engage in the act of inquiry, conjecture, and at least experiential verification.

Another concept which may be difficult to describe without demonstration is the *confidence interval*.  Given some sample proportion, one generates an interval which has the probability (also a proportion) of containing yet another (the population) proportion.  This is an intricate definition, and students can find the task of unpacking what a confidence interval actually *is* to be daunting.  However in [this activity](https://tienchih.github.io/tbil-stats/Sec_ConfidenceInterval.html#activity-Steak), the students assuming  that 23% of Americans like their steaks medium rare, and then simulate what random samples of the population may produce as samples and corresponding confidence intervals.  Simulating 100 potential confidence intervals at random, they can see how much variation there may be in samples of size <c>n=50</c> and the corresponding confidence intervals may vary as well.  They can also see that about 95% of the confidence intervals will contain the actual true proportion of 0.23:


<iframe
  width="100%"
  height="500px"
  src="https://sagecell.sagemath.org/?z=eJxtkk1PwzAMhu-V-h-sIqEEMrYOgQRSkDiMIwc-TtM0ZW2mBaVJSLPBQPx3nK7bOtilSmy_j187NcuqFpXj-WCQJoZf4dfxwcXwMk3SJGambiECcPDSkcd7BmYjoG32eXQ0V1ht_XQlC_4_lyZz64kCZSC_3Ya_0wQgniJupoytSM4McxROwM6CwGLR5LUEO4dafUkwAHNvKwgLCc66pRZBWbMFNcbHaoLAeO1jNSDsQZlyo_DWWR8VgH4wpOq2wY7wPGr17z6QLvOM5L3unfYNPYDXQZhS-BKk99anyU-aaPshm5UE7Mb3FqEH-cXNNZxte6bJ0rnjpef_SptlAnk7ss3dG4zfJjzzssxiVM2Ju-NdM5iGU-j2xMgdR8ohYaaXMvtpZnHaBlKQShnSJVFWiU_SJVHKoCADBjl637pjENZO4mCZyWg7wJ_fAbQysiaoPXCqJuzQKK6eFUQxUEjVHyVChyxOzvfeo0iUJX95eh1hOzGLbLLCUtfR6LCOB_oL3D_7-w==&lang=r&interacts=eJyLjgUAARUAuQ==">
</iframe>

These are just a couple of the statistical concepts which lend themselves well to discovery and inquiry via simulation and experimentation.  Having these cells loaded and ready in a [PreTeXt workbook](https://tienchih.github.io/tbil-stats/book-1.html) cuts down on the barrier to entry for the students to benefit from these sorts of activities. The structure of the activities are highly informed by [Team Based Inquiry Learning](GitHub.com/TeamBasedInquiryLearning) pedagogy, the content follows from the excellent [OpenIntro](https://www.openintro.org/) OER.  To talk more about tech in math education, you can find me and many others on the [MathTech.org Discord](https://discord.gg/64tkJueD6G)!