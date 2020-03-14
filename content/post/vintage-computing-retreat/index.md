---
title: "Vintage Computing Retreat"
date: 2020-03-14T04:50:29-04:00
draft: true
tags: ["vintage-computing"]
---

{{< img "museum-front.jpg" "Store front of the Large Scale Systems Museum" "740" >}}

As luck had it, I was able to return to the [Large Scale Systems
Museum](http://www.mact.io/) and this time, I could spend four days
for an extended vintage computing retreat.  Dave McGuire, the curator
and director of the museum, was kind enough to host me and let me
spend my nights in the museum, while he was asleep.

On my last visit, I was mostly struck with awe by the massive
collection that Dave has accumulated over the years.  The showroom
itself is impressive, with dozens of mainframes, minis and peripherals
on display.  While there are so many things to drool about, the
[PLATO](https://en.wikipedia.org/wiki/PLATO_(computer_system))
terminal is a gem that I had never expected to see in my life:

{{< img "plato-terminal.jpg" "PLATO Terminal" "600" "" >}}

I had a knack for computer reel tapes since I first saw them as a kid,
and that interest seems to not fade away.  Whenever I go to a computer
museum, I hope to see one of those drives spinning.  Maybe it is
because there is so little movement in a data center that keeps me
attracted.

Tape drives are also featured rather nicely in [one of my favorite
movies](https://en.wikipedia.org/wiki/Dr._Strangelove):

{{< youtube "98NaJ8ss4sY?t=90" >}}

So keep me busy and entertained for the week, my plan was to get some
of the many tape drives that are in the museum to actually work:

{{< thumbnail "hp7978.jpg" >}}
{{< thumbnail "ibm2440.jpg" >}}
{{< thumbnail "ta78.jpg" >}}
{{< thumbnail "tape1.jpg" >}}
{{< thumbnail "te16.jpg" >}}
{{< thumbnail "tu81plus.jpg" >}}

# IBM 2440

There is a rack with two IBM 2440 drives in the museum.  These are
massive, great-looking machines with heavy metal doors, huge hinges
and spared-no-expense build quality.  They are connected to an IBM
mainframe using the [Bus and Tag
Interface](https://en.wikipedia.org/wiki/Bus_and_Tag).

Both drives had not been in working order, so I set about trying to
find out what their problems are and whether I could fix them.  The
upper of the two drives powered up fine, but it had trouble loading
the tape.  It got stuck in the load process at step L20 without timing
out or doing anything else.  The service manual provided little help,
indicating that the problem should be located in the upper vacuum
column and one should check for clogged sensor nozzles.

The lower of the drives did not power up at all, but I found a blown
fuse behind the drive door and replaced that.  After some head
cleaning, this drive passed its self tests without errors.  It is
unfortunate that it is the lower drive, which is hard to access or
show off.

The 2440 drive is self-threading, so the operator only needs to place
the reel on the file wheel, close the door and press the "Load"
button.  The rest is done by the machine automatically:

{{< youtube "3sRND6VNlDU" >}}

During the load process, a series of incrementing codes is shown in
the LED display on the top.  They show the progress that the tape
makes through its path inside the drive, pulled by the vacuum pump and
close coordination of wheel movements.

With the drive passing self tests, Dave set about to get it hooked up
to the mainframe so that we could actually try it out.  This required
him to go through the lengthy boot process of the System 390.  Also,
he had to reconfigure the hardware so that it would be able to use the
drive as configured, as its physical address was in use by a terminal
concentrator.  Luckily, there are several levels of addressing
indirection between the hardware and the operating system on a modern
IBM mainframe, so in the end it was a matter of changing some text
files before booting the machine.  I was rather impressed, both by
this flexibilty and by Dave actually putting it to good use within a
few minutes.

He was then able to read and write tapes from the mainframe, but
apparently there are some issues limiting the data rate between the
system and the drive, preventing it from streaming.  Reading and
writing small blocks from and to the drive causes it to make loud
noises as it steps the tape forward in tiny little increments.

# TU81

# Thanks!

Thanks to Autumn and Dave for having me this week!  It was great fun
working on these machines.  It is clear that 4 days are not enough to
become a good service engineer for any of the machines that I worked
on, but next time I'll have the opportunity, I will know some more
about them.  I can highly recommend doing a Vintage Computing Retreat
to anyone who has experience or is daring enough to get some more.
There are so many things in the Large Scale Systems Museum which one
can take care of, preserving these amazing machines for future
visitors and generations.
