---
title: HTML vs PDF for screen readers
tags: PreTeXt, accessibility, course-materials, videos
author: Oscar Levin
---

After my [last post](https://mathtech.org/2025/04/10/accessibility.html) about writing accessible course materials, someone asked whether the math in the HTML produced by PreTeXT really is any better than what you would get for a PDF when read by a screen reader.  Is it worth the effort? 

<!--break-->

I think so!  To demonstrate the differences, I recorded a short video showing how the [NVDA](https://www.nvaccess.org/about-nvda/) screen reader performs reading an HTML document compared to a PDF document.

{% include video id="Cxeh5MLNjNU" provider="youtube" %}


NVDA (Non-Visual Desktop Access) is a free and open source screen reader available for Windows.  It takes some practice to get used to navigating, and even then, you might need to tweak settings to get things to work. For example, you might need to right-click on some math in the HTML to get to the MathJax menu and turn on screen reading.  Similarly, when opening a PDF with Adobe Acrobat, it might ask you to fix the document to be used with a screen reader.

There are other screen reader options available, incluing the comercial JAWS and built in screen readers for your operating system.  I haven't tested any of these, so if you have suggestions, please get in touch or comment below.

Oh, and one more thing.  In the video, Adobe Acrobat messed up the math expressions by combining them into one, even though they were in different parts of the document.  This doesn't always happen, so I'm not saying that it is impossible to fix up a PDF so that a screen reader works correctly.  However, it would definitely be an extra step and might require multiple tries to get right.  With PreTeXt, the HTML will *just work*.



