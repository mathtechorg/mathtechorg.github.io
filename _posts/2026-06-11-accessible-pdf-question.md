---
title: Accessibile PDFs?
tags: accessibility pdf html
authors: 
- clontz
comments: true
---

A frequent question our community is asked: "How do I make an accessible PDF?"

And, even setting aside the large commercial incentives for certain businesses to make accessible PDFs viable,
there is a huge community working towards this goal! I have so much respect for professionals
like [Deyan Ginev](https://kwarc.info/people/dginev/), codeveloper of [LaTeXML](https://github.com/brucemiller/LaTeXML/)
and instrumental in the [ar5iv project](https://ar5iv.labs.arxiv.org/) converting arXiv preprints
to accessibile HTML.

But that's just it -- why are the most significant projects aiming to make accessible scholarly STEM documents
aiming not for an accessible PDF, but instead aiming to help authors create accessible HTML?

I believe it's because it's **just not possible to create accessible PDFs**.

Accessibility is not a trivial concern. But like many in our community, I do my best to help professionals
in the mathematical sciences create accessible documents, particularly those affected by
[DoJ's April 2027 deadline for public institutions](https://www.ada.gov/resources/2024-03-08-web-rule/)
to ensure all web content meets meet WCAG 2.1, Level AA requirements.

For working professionals in the mathematical sciences, it's not trivial or reasonable to become
an expert in accessibility. So how can we ensure we're doing not just meeting our legal obligations, but also
ensuring we are serving all of our readers as best we can?

My advice is to connect with communities and technologies that help you author and disseminate
semantic and accessible HTML-format mathematical documents. As a co-creator of [PreTeXt.Plus](https://pretext.plus)
I obviously would suggest trying us out, but more crucially I would emphasize the importance of
choosing any solution that targets *accessible HTML*, and not "accessible PDF".

While initiatives like the [LaTeX Tagged PDF Project](https://latex3.github.io/tagging-project/) attempt to
help authors create mathematical PDFs that are as accessible as possible, I'm very concerned that this
effort is a non-starter. In addition to the
[numerous examples of failed screenreader parsing of PDFs with math equations](https://www.youtube.com/watch?v=Cxeh5MLNjNU),
we must consider the accessibility requirement that
[content reflows to fit the device viewport](https://www.w3.org/WAI/WCAG21/Understanding/reflow.html),
even if the user zooms in to read text at the size they can visually perceive.

How do PDFs meet this standard? Well, in practice, they simply don't! Below I'll illustrate a PDF recently published
in the open access [Bulletin of the AMS](https://pubs.ams.org/journals/bull),
and a 300% zoomed-in (via the most popular web browser Chrome) PDF that fails to meet this standard.

![PDF screenshot with all text in the viewport](/assets/images/20260611/image1.png)

![further zoomed PDF of the same content that does not reflow content to fit the viewport](/assets/images/20260611/image2.png)

Now, it's technically possible for readers to spend signficant money on proprietary Adobe software that reflows PDF output
(well, to [mixed success](https://mn.gov/mnit/media/blog/?id=38-584554)).
But meanwhile, any HTML file will immediately and trivially meet this simple standard.
To illustrate this point, let's compare the exact same content published as HTML instead.

![standard zoom setting for article](/assets/images/20260611/image3.png)

![significantly zoomed in view of article; all content reflows to fit the device viewport](/assets/images/20260611/image4.png)

In a nutshell, this is the issue: PDFs
are not written to be accessible content for all audiences.
Instead, they are designed for creating print-ready documents
for fully sighted readers.

So what can we do to help our readers? The first and easiest
step is to assume that all PDF output is inaccessible, and
then to look toward solutions that publish HTML instead.
While writing HTML is not sufficient to guarantee
accessibility (nothing, not even AI, can replace the human
responsibility to author useful alt text for graphical
information), choosing semantic formats such as HTML over
visual-first outputs like PDF is an important first
step to serving all readers through universal design.
