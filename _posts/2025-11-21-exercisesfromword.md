---
title: So, all your exercises are written in Word...
tags: Word PreTeXt exercises scripts
author: James Vranish
comments: true
---
I'm a biochemistry instructor who has been working on writing an introductory chemistry textbook.  Now, as a user of math rather than a mathematician, my initial approach was to write my textbook in Word and later worry about getting it formatted for HTML and print forms.  My colleague, who is also one of mathtech.org's editors, had mentioned PreTeXt to me on several occasions, but with the stress of writing/revising/etc. her words basically went in one ear and right out the other.  That is...until it came time to try and move towards publication.  After a few minutes of sitting down with her and *actually* paying attention (sorry Chrissy), I quickly realized how amazingly powerful PreTeXt was for what I was trying to do.

After a couple of months, most of which was spent trying to find the time to work on it and trying to learn my way around Github, I've got most of the text formatted and building in PreTeXt.  Now for the fun of adding in the images, the figures, and the EXERCISES.  Oh dear the exercises.  All of my exercises were written in Word and they often employ the Microsoft Equation Editor (go ahead and let your heads spin around mathematicians...I'll wait...).  As a chemist, I know about LaTeX, but honestly I didn't think I'd use equations enough to bother writing entire documents using it.  To make matters worse, each section of my textbook has dozens of problems, the answers are at the end of each section, rather than right next to the question, and yeah...I quickly realized what a pain this was going to be to get every exercise formatted correctly for PreTeXt.

So, I thought that maybe I could write a script to do it for me.  I'd like to say that my programming ability is up to the task (it isn't...it REALLY, REALLY, isn't...).  Plus I have NO idea how to parse a docx file.  So, what's chemist to do...?  Well, I'd heard that ChatGPT was pretty good at writing programs.  What the heck, let's give it a shot.  I figured I could copy my exercises and answers into a single Word file in a reasonable format that could (hopefully) be easily parsed. I told ChatGPT about my file format and my goals and amazingly, it worked (after a few hiccups that mainly involved me trying to figure out what language would be best/easiest for me to use...i.e. wouldn't require me to download and install extra packages).  ChatGPT, I discovered, is great at allowing you to incrementally add functionality to a program.  My first attempt could only process text questions and answers.  Later attempts started being able to read the output of the Microsoft Word Equation Editor and format that output into LaTeX.  Then I had ChatGPT add additional functionality to recognize even more types of functions in my Word files.  I started even reaching beyond what's in my text, challenging ChatGPT to be able to convert integrals and limits (this isn't working fully yet, but if you want to to play around with what I've started, have at it!).

After several hours worth of back and forth with ChatGPT...occasionally chewing out the brainless wonder kid for messing up the code that had been working while attempting to fix other things and whatnot..., I've got a functional program that reads Word files that are formatted in a file called **Input.docx** as a nested list of exercises, answers, hints, and solutions, for example:  

1. Question 1

    a. Answer

        i. Hint
            1. Solution
2. Question 2

    a. Answer
    
        i. Hint
            1. Solution

and it outputs valid PreTeXt exercises with statements, answers, hints and solutions.

|Before|After|
|:-:|:-:|
|![Some sample exercises written in Word](/assets/images/20251121/exercisesinword.png)|![after conversion, the same sample exercises formatted in PreTeXt](/assets/images/20251121/exercisesinpretext.png) |


The Equation Editor to LaTeX functionality still needs some work (limits and integrals don't work well at the moment...), but it does what I need it to for my chemistry text.  Perhaps my story, my approach, and/or the code below can help you with similar problems you might be facing.

Well, this has been fun invading the mathematical space.  **Chemist out!**

To try it yourself, this what I type on the command line: 
```
powershell.exe -ExecutionPolicy Bypass -File .\ConversionProgram.ps1 -InputDocx .\input.docx -OutputFile .\output.ptx
```

And this is the program: [ConversionProgam.txt](/assets/files/20251121/ConversionProgram.txt) (Rename the file to ConversionProgram.ps1).  
