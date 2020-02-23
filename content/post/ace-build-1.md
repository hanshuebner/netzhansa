---
title: "Building a Jupiter Ace - Part 1"
date: 2020-02-23T08:19:20+01:00
draft: true
---

I have been tinkering with electronics since I was a kid, and since my
high school years I dreamt of building a computer from scratch.  Back
then, my dream CPU was the venerable Motorola MC6809 and I spent my
lunch break drawing schematics and pondering what chips I would want
to use.  That design work stayed theoretical, but recently I came
close to this dream when I started to build a Jupiter Ace home
computer from parts.

The [Jupiter Ace](https://en.wikipedia.org/wiki/Jupiter_Ace) was a
home computer similar to the Sinclair ZX series, and in fact it was
designed by the some engineers that previously worked for Sinclair
research.  It thus does not come as a surprise that the Jupiter Ace
was similar to the Sinclair systems in several respects: It used a
Zilog Z80 CPU and it had a rubber keyboard not unlike the ZX Spectrum.
The most important difference, though, was that the Jupiter Ace had
FORTH as its programming language.  It was quite a bold decision to
not go with BASIC like virtually all other home computers of the time
did, and it is probably fair to say that FORTH was one of the reasons
why the Jupiter Ace flopped commercially.

Another difference between the Jupiter Ace and most other home
computers of the time was that it was built entirely out of standard
chips.  Because no special parts are required, several people have
built clones of the Jupiter Ace over time.  I had pondered doing that
myself for quite a while, and when I found that the original manual
for the machine had been
[republished](https://www.amazon.co.uk/Jupiter-ACE-Manual-Anniversary-Programming/dp/1785387294/ref=sr_1_1),
I went ahead and sourced the components.

# Build plan

Even though the Jupiter Ace is not a very complicated machine, it
still consists of 25 Chips.  I decided that rather than designing a
PCB and having that manufactured in China, I would use Veroboard and a
[wiring pencil](https://en.wikipedia.org/wiki/Wiring_pencil).  I used
that technique for many years, but this was by far the largest project
I ever undertook.
