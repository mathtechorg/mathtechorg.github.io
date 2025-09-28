---
title: Using Doenet Interactives to Facilitate Inquiry
tags: PreTeXt Doenet Inquiry
author: Steven Clontz
---

*This post describes work of the author funded by National Science Foundation awards [#2011807](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2011807) and [#2449139](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2449139).*

---

An essential element of my [Team-Based Inquiry Learning (TBIL)](https://TBIL.org) classroom
is that, as much as possible, students whould be discovering knowledge for themselves,
rather than just being told.

However, I only have 150 minutes with my students each week! And while I'd love to
give them the expeirence of my own inquiry-based learning classrooms (a list of
problems to solve and theorems to prove, a pat on the head, and a "go get 'em, show
us in class next week what you were able to figure out"), that level of unscaffolded exploration
is frankly not appropriate for the majority of my students, who are mostly non-mathematics
STEM majors.

To this end, a TBIL classroom emphasizes the need for "Scaffolded Exploration", where students
are given enough tools, preparation, and context to authentically engage with an activity
using inquiry, without leaving them rudderless and unable to even begin to puzzle things out.

One mechanism to do this is to provide students interactive applets to help them explore
mathematical concepts. While many of us who went on to teach in the mathematical sciences
are quite comfortable with drawing several examples to explore a new concept, or even
visualizing things in our own heads, many students have not yet developed such skills, or
at least are not prepared or confident enough to run to a whiteboard or their paper to do
so in a small group of their peers.

I found this particularly tricky when I first introduced
[geometric properties of linear maps](https://library.tbil.org/2025/linear-algebra/GT.html)
in my sophomore (engineering-majority) linear algebra course. Asking students to consider
how changing the length of a parallelogram's base affects the change in its area isn't
a terribly deep question, but it's one that definitely benefits from visualization.

So in
[Activity 5.1.8 (2024 Early Edition)](https://teambasedinquirylearning.github.io/linear-algebra/2024e/GT1.html#activity-140)
we provided such a static image:

<img style="background-color:white; width:100%" alt="before/after of scaling a determinant" src="https://teambasedinquirylearning.github.io/linear-algebra/2024e/generated/latex-image/GT1-image-scaled-column-determinant.svg">

Well, it's certainly better than nothing. But for all the abstraction of multiplying the base by $c$,
that $c$ value sure looks fixed at around maybe $1.4$... Would it be nice if students could see the
effect for many different values of $c$, or even observe the effect for the $c$ values of their choosing?

Enter [Doenet](https://doenet.org/), a language and platform for authoring interactive mathematics visualizations.
There's much to be said about Doenet (and I'm sure to do so in my future posts!), but for today
I'm just going to focus on the end product (which, of course, is 100% open-source markup powered by
100% open-source software). Let's see how Doenet can helped us create an interactve figure to replace
the above static image in [Activity 5.1.10 (2025 Edition)](https://library.tbil.org/2025/linear-algebra/GT1.html#GT1-activity-scale-column):

<iframe src="https://library.tbil.org/2025/linear-algebra/GT1-interactive-scale-column-if.html" style="width: 100%; background-color: white; min-height: 600px;"></iframe>

This has worked so much better! I can send this link to my students' computers, and together they will
move the slider, observing the original area when $c=1$, the doubled area when $c=2$, and the halved area
when $c=0.5$! By removing the cognitive overload of visualizing various values of $c$ in their heads
or on paper, students can dive directly into the mathematics, particularly my true goal to make them
realize that $\operatorname{det}([c\vec{v}\hspace{0.5em} \vec{w}])=c\operatorname{det}([\vec{v}\hspace{0.5em} \vec{w}])$
without me telling them this myself!

To see how we explore the determinant's other geometrical properties using Doenet interactives, check out
[Section 5.1 of our 2025 Edition](https://library.tbil.org/2025/linear-algebra/GT1.html)! In a future post, I'll explore
how to create a great Doenet interactive for yourself at [Doenet.org](https://doenet.org/).
(Or if you're inpatient, you can of course see the Doenet source for this interactive yourself
[here on GitHub](https://github.com/TeamBasedInquiryLearning/library/blob/8988a246077a74888461faf57e8acc9292bee8a7/source/linear-algebra/source/05-GT/doenet/GT1-doenet-scale-column.xml).)

---

As you probably expect,
TBIL activities and interactives aren't trivial to always come up with alone, which is why I'm excited to work with a great
team of collaborators at [GitHub.com/TeamBasedInquiryLearning](https://github.com/TeamBasedInquiryLearning)
to create high-quality, free and open-source TBIL activity books for Precalculus, Calculus,
and Linear Algebra. If you want to get involved, ping me in the
[MathTech.org Discord](https://discord.gg/64tkJueD6G), or even better, join our
[TBIL Zulip chat](https://tbil.zulipchat.com/)!

