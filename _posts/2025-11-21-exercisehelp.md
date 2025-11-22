---
title: So, all your exercises are written in Word...
tags: Word, pretext, exercises, scripts
author: James Vranish
comments: true
---
Well, thanks to Dr. Christina Safranski (Chrissy), Pretext is spreading beyond the realm of mathematics.  I'm a biochemistry instructor at Franciscan University of Steubenville, and I've been working on writing an introductory chemistry textbook.  Now, as a math user rather than a mathematician, my initial approach was to write my textbook in Word and later worry about getting it formatted for HTML and print forms.  Chrissy had mentioned Pretext to me on several occasions, but with the stress of writing/revising/etc. her words basically went in one ear and right out the other.  That is...until it came time to try and move towards publication.  After a few minutes of sitting down with her and *actually* paying attention (sorry Chrissy), I quickly realized how amazingly powerful Pretext was for what I was trying to do.

A few months later (mostly trying to find time and trying to learn my way around Git (again with Chrissy's fabulous help)) and I've got most of my text formatted correctly.  Now for the fun of adding in the images, the figures, and the EXERCISES.  Oh dear the exercises.  All of my exercises were written in Word and they often employ the equation editor (go ahead and let your heads spin around mathematicians...I'll wait...).  As a chemist, I know about LaTeX, but honestly I didn't think I'd use equations enough to bother writing entire documents using it.  To make matters worse, each section of my textbook has dozens of problems, the answers are at the end of each section, rather than right next to the question, and yeah...I quickly realized what a pain this was going to be to get every exercise formatted correctly for Pretext.

So, maybe I should write a script to do it for me.  I'd like to say that my programming ability is up to the task (it isn't...it REALLY, REALLY, isn't...).  Plus I have NO idea how to parse a docx file.  So, what's chemist to do...?  Well, I'd heard that ChatGPT was pretty good at writing programs.  What the heck, let's give it a shot.  I figured I could copy my exercises and answers into a single Word file in a reasonable format that could (hopefully) be easily parsed. I told ChatGPT about my file format and my goals and amazingly, it worked (after a little hiccups that mainly involved me trying to figure out what language would be best/easiest for me to use...ie. wouldn't require me to download and install extra packages).  ChatGPT, I discovered, is great at allowing your to incrementally add functionality to a program.  My first version could only process text questions and answers.  Future versions started being able to read the output of the Microsoft Word Equation Editor and format the output into LaTeX.  Then I had it add additional functionality to recognize even more types of functions in my Word files.  I started even reaching beyond what's in my text, challenging ChatGPT to be able to convert integrals and limits (this isn't working fully yet, but if you wanto to play around with the program below (or let ChatGPT do it for you, have at it!).

Long story short (several hours worth of back and forth with ChatGPT...occasionally chewing out the brainless wonder kid for messing up the code while fixing other things and whatnot...), I've got a functional program that reads Word files that are formatted thusly:

**Input.docx**

1. Question
   a. Answer
     i. Hint
       1. Solution
2. Question 2
   a. Answer
     i. Hint
       1. Solution  ...

The Equation Editor to LaTeX functionality still needs some work (limits and integrals don't work well at the moment...), but it does what I need it to at the moment!  Perhaps my story, my approach, and/or this code can help you with similar problems you might be facing.

**Command line to run the program:**  powershell.exe -ExecutionPolicy Bypass -File .\ConversionProgram.ps1 -InputDocx .\input.docx -OutputFile .\output.ptx

**ConversionProgam.ps1**
param(
    [Parameter(Mandatory=$true)]
    [string]$InputDocx,

    [Parameter(Mandatory=$true)]
    [string]$OutputFile
)

Add-Type -AssemblyName System.IO.Compression.FileSystem

# Extract document.xml from DOCX
function Get-DocxXml {
    param($path)
    $tempDir = Join-Path $env:TEMP ([System.IO.Path]::GetRandomFileName())
    [System.IO.Compression.ZipFile]::ExtractToDirectory($path, $tempDir)
    $xmlPath = Join-Path $tempDir "word\document.xml"
    return [xml](Get-Content $xmlPath -Raw)
}

# Recursive OMath -> LaTeX converter
function Convert-OMathNode-To-LaTeX {
    param($node, $nsMgr)
    if (-not $node) { return "" }

    $latex = ""

    foreach ($child in $node.ChildNodes) {
        switch ($child.LocalName) {
            "f" {
                $num = Convert-OMathNode-To-LaTeX $child.num $nsMgr
                $den = Convert-OMathNode-To-LaTeX $child.den $nsMgr
                $latex += "\frac{$num}{$den}"
            }
            "sSub" {
                $base = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                $sub = Convert-OMathNode-To-LaTeX $child.sub $nsMgr
                $latex += "${base}_{$sub}"
            }
            "sSup" {
                $base = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                $sup = Convert-OMathNode-To-LaTeX $child.sup $nsMgr
                $latex += "${base}^{$sup}"
            }
            "sSubSup" {
                $base = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                $sub = Convert-OMathNode-To-LaTeX $child.sub $nsMgr
                $sup = Convert-OMathNode-To-LaTeX $child.sup $nsMgr
                $latex += "${base}_{$sub}^{$sup}"
            }
            "sPre" {
                $sub = Convert-OMathNode-To-LaTeX $child.sub $nsMgr
                $sup = Convert-OMathNode-To-LaTeX $child.sup $nsMgr
                $base = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                $latex += "_{$sub}^{$sup}$base"
            }
            "rad" {
                $radicand = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                $latex += "\sqrt{$radicand}"
            }
            "accent" {
                $base = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                $chrNode = $child.SelectSingleNode(".//m:chr", $nsMgr)
                if ($chrNode) {
                    $val = $chrNode.GetAttribute("val")
                    switch ($val) {
                        "→" { $latex += "\vec{$base}" }
                        "⟶" { $latex += "\overrightarrow{$base}" }
                        default { $latex += $base }
                    }
                } else { $latex += $base }
            }
            "nary" {
                $chrNode = $child.SelectSingleNode(".//m:chr", $nsMgr)
                $symbol = ""
                if ($chrNode) { $symbol = $chrNode.GetAttribute("val") }

                switch ($symbol) {
                    "∫" {
                        $lower = Convert-OMathNode-To-LaTeX $child.sub $nsMgr
                        $upper = Convert-OMathNode-To-LaTeX $child.sup $nsMgr
                        $body = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                        $diff = ""
                        if ($body -match "(d[a-z]+)$") {
                            $diff = $matches[1]
                            $body = $body -replace "(d[a-z]+)$",""
                        }
                        $latex += "\int"
                        if ($lower -ne "") { $latex += "_{$lower}" }
                        if ($upper -ne "") { $latex += "^{$upper}" }
                        $latex += " $body$diff"
                    }
                    "lim" {
                        $sub = Convert-OMathNode-To-LaTeX $child.sub $nsMgr
                        $base = Convert-OMathNode-To-LaTeX $child.e $nsMgr
                        if ($sub -ne "") { $sub = $sub -replace "→","\\rightarrow"; $latex += "\lim_{$sub} $base" }
                        else { $latex += "\lim $base" }
                    }
                    default {
                        foreach ($c in $child.ChildNodes) { $latex += Convert-OMathNode-To-LaTeX $c $nsMgr }
                    }
                }
            }
            "r" {
                $tNode = $child.SelectSingleNode("m:t", $nsMgr)
                if ($tNode) {
                    $text = $tNode.InnerText
                    $text = $text -replace "×","\times"
                    $text = $text -replace "÷","\div"
                    $text = $text -replace "→","\rightarrow"
                    $text = $text -replace "…","..."
                    $latex += $text
                }
            }
            default {
                $latex += Convert-OMathNode-To-LaTeX $child $nsMgr
            }
        }
    }

    return $latex
}

# Load XML
$xml = Get-DocxXml $InputDocx

# Namespace manager
$nsMgr = New-Object System.Xml.XmlNamespaceManager($xml.NameTable)
$nsMgr.AddNamespace("w",  "http://schemas.openxmlformats.org/wordprocessingml/2006/main")
$nsMgr.AddNamespace("m",  "http://schemas.openxmlformats.org/officeDocument/2006/math")

# Get all paragraphs
$paragraphs = $xml.SelectNodes("//w:p", $nsMgr)

$outputExercises = @()
$currentQuestion = ""
$currentAnswer = ""
$currentHint = ""
$currentSolution = ""

foreach ($p in $paragraphs) {
    $numNode = $p.SelectSingleNode(".//w:numPr/w:ilvl", $nsMgr)
    if (-not $numNode) { continue }
    $level = [int]$numNode.val

    # Build text content with math
    $text = ""
    foreach ($child in $p.ChildNodes) {
        if ($child.LocalName -eq "oMath" -or $child.LocalName -eq "oMathPara") {
            $latex = Convert-OMathNode-To-LaTeX $child $nsMgr
            $text += "<m>$latex</m>"
        }
        elseif ($child.LocalName -eq "r") {
            $t = $child.SelectSingleNode("./w:t", $nsMgr)
            if ($t) { 
                $txt = $t.InnerText -replace "…","..."
                $text += $txt 
            }
        }
    }

    $text = $text.Trim()

    switch ($level) {
        0 { # Question
            if ($currentQuestion -ne "") {
                $outputExercises += "<exercise>"
                $outputExercises += "  <statement>"
                $outputExercises += "    <p>$currentQuestion</p>"
                $outputExercises += "  </statement>"

                if ($currentHint -ne "") {
                    $outputExercises += "  <hint>"
                    $outputExercises += "    <p>$currentHint</p>"
                    $outputExercises += "  </hint>"
                } else {
                    $outputExercises += "  <!--<hint>--> "
                    $outputExercises += "  <!--</hint>--> "
                }

                $outputExercises += "  <answer>"
                $outputExercises += "    <p>$currentAnswer</p>"
                $outputExercises += "  </answer>"

                if ($currentSolution -ne "") {
                    $outputExercises += "  <solution>"
                    $outputExercises += "    <p>$currentSolution</p>"
                    $outputExercises += "  </solution>"
                } else {
                    $outputExercises += "  <!-- <solution>--> "
                    $outputExercises += "  <!-- </solution>--> "
                }

                $outputExercises += "</exercise>"
                $outputExercises += ""
            }

            $currentQuestion = $text
            $currentAnswer = ""
            $currentHint = ""
            $currentSolution = ""
        }

        1 { $currentAnswer = $text }
        2 { $currentHint = $text }
        3 { $currentSolution = $text }
    }
}

# Last exercise
if ($currentQuestion -ne "") {
    $outputExercises += "<exercise>"
    $outputExercises += "  <statement>"
    $outputExercises += "    <p>$currentQuestion</p>"
    $outputExercises += "  </statement>"

    if ($currentHint -ne "") {
        $outputExercises += "  <hint>"
        $outputExercises += "    <p>$currentHint</p>"
        $outputExercises += "  </hint>"
    } else {
        $outputExercises += "  <!--<hint>--> "
        $outputExercises += "  <!--</hint>--> "
    }

    $outputExercises += "  <answer>"
    $outputExercises += "    <p>$currentAnswer</p>"
    $outputExercises += "  </answer>"

    if ($currentSolution -ne "") {
        $outputExercises += "  <solution>"
        $outputExercises += "    <p>$currentSolution</p>"
        $outputExercises += "  </solution>"
    } else {
        $outputExercises += "  <!-- <solution>--> "
        $outputExercises += "  <!-- </solution>--> "
    }

    $outputExercises += "</exercise>"
}

# Save output
$outputExercises -join "`r`n" | Out-File $OutputFile -Encoding UTF8

Write-Host "Conversion complete. Output saved to $OutputFile"


Well, this has been fun invading the mathematical space.  **Chemist out!**
