---
title: "What's the most productive tool?"
date: 2020-06-10T07:23:18+02:00
tags: ["xslt", "code", "lisp"]
---

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">XSLT, quite clearly. Followed by Common Lisp and Clojure</p>&mdash; Hans Hübner (@HansHuebner) <a href="https://twitter.com/HansHuebner/status/1270406784050757632?ref_src=twsrc%5Etfw">June 9, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

My reply deserves some elaboration, so here it goes:

# What is XSLT?

XSLT is a document transformation language.  It was conceived as part
of the [Extensible Stylesheet Language
Family](https://www.w3.org/Style/XSL/) and was originally intended to
prepare XML documents for printing.  While the document formatting
aspect of the family has not seen much development during the last
decade, XSLT and its sibling XPath were continuously improved.

XSLT, in its latest version 3.0 from summer 2017, is now a
general-purpose document transformation language.  While it is still
largely compatible with version 1.0, most of the things that made XSLT
1.0 rather hard to use in many aspects are fixed.  Sure, XSLT still
uses XML syntax and some people might find that alone to be a no-go.
As a Lisp programmer, though, I find this argument as superficial as
the never-ending objections raised about Lisp's ubiquitous use of
[parentheses](https://xkcd.com/297/).

# Choosing the right tool for the job

The original tweet that I replied to asked for the "software or tool or
language or framework" that I felt being most productive in.  XSLT
fits this space in the realm of programming languages, but I did not
mean to say that XSLT is the right language for every job.  Its
strength is transforming documents, so whenever I need to convert a
file from one format to another, I consider using XSLT.

Now, "format conversion" is a wide field and it can mean several
things: It can mean simply turning an XML or JSON file into a CSV or
Excel file.  But in the process, data can be added, removed,
reordered, and this is where the power of XSLT comes into play.

Whenever I need to convert some data, I consider using XSLT for the
task and if I come to the conclusion that it would be the right tool,
I also have an idea how to solve it in XSLT.  Now here is what makes
it special for me:  Once I have devised a solution in XSLT, I usually
write the transformation and am done.  Done in the sense that the
solution works as I had imagined it and I can move on to something
else.  Done also in the sense that there is nothing I need to refactor
because the process of building the solution required me to try out
different approaches, leaving things back untidy.

Now you could argue that this is because I written too much XSLT and
that the same thing could be said about any language by someone who is
an expert.  For one, though, I have written more code in other
languages.  I am or have been an expert programmer in Clojure, Common
Lisp, Perl, C++ and C, and I would still say that XSLT is most
productive.  For another, there are some specific properties in XSLT
that make it as productive as it is.

# Homoiconicity

Lisp programmers love their parentheses, and it is for a very good
reason.  They are the fundamental syntactic abstraction that is used
both for representing code and data in text form.  Because both code
and data are denoted in their text form as lists, enclosed in
parentheses, Lisp programs can manipulate themselves, and thus is said
to be [Homoiconic](https://en.wikipedia.org/wiki/Homoiconicity).  Now,
here's the catch:  Lisp code and data is only homoiconic after it has
been converted from its text form into the internal representation.
Lisp source text, on the other hand, is just text and Lisp cannot
handle text more easily than other languages.

XSLT, on the other hand, is a document transformation language, and
XSLT source files are just XML documents.  Thus, XSLT can easily be
used transform XSLT source files into something else, or generate
XSLT.  XSLT thus is homoiconic in a very practical sense, and this is
because it uses XML as its syntactic base.

I have used XSLT often to create file converters: From a table
describing the fields of an input file, an XSLT transformation
generated another XSLT transformation that would turn an input file
conforming to the specification in the table to some other format.
This is so straightforward and requires so little boilerplate code
that I never felt the need to write a library or framework to do it.
If I need to convert a file and have a table describing the input
data, I write an XSLT that converts the table to XSLT which I then use
to convert the actual files.

Likewise, I have used XSLT to refactor hand-written XSLT
transformations.  As it can read and write XML files preserving all
white spaces and comments, this was really just a matter of thinking
about a solution and writing it down.

# No I/O

In its basic operation mode, an XSLT transformation processes one
input file into one output file.  This is implicit and the program
does not need to specify where the data comes from or where it goes
to.  It is possible to read or write multiple documents in one
transformation, but in many applications this is not needed.  This
makes XSLT devoid of boilerplate for the most common use case, thus
keeping programs compact.

Furthermore, as transformation of one document to another is the basic
operation mode of XSLT, the program can implicitly assume the
mechanics of file handling to be present.  I have had several
applications where I needed to convert a specific element down in
input tree which consisted of a single template.

In this regard, XSLT is similar to [perl](https://www.perl.org/) and
[sed](https://en.wikipedia.org/wiki/Sed), which can operate on streams
easily.  Those tools normally operate on lines, though, where XSLT
operates on tree structured documents.

# Do you need to learn XSLT?

I can't answer this for you, but if you're converting documents from
one form to another, then I would recommend having a look at it.  I'd
certainly recommend it over using the excellent
[jq](https://stedolan.github.io/jq/) for tasks that are not one-liners
in shell scripts.  Ihe Onwuka wrote a [blog
post](https://www.linkedin.com/pulse/why-good-software-engineer-should-love-xslt-ihe-onwuka/)
that might convince you.
