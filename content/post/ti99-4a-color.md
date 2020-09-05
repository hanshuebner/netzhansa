---
title: "Getting Color Output out of a TI-99/4A"
date: 2020-09-05T18:27:26+02:00
tags: ["hardware", "vintage-computing"]
draft: false
---

![TI-99/4A](https://upload.wikimedia.org/wikipedia/commons/1/1b/TI99-IMG_7143.jpg)

When I was around 13-14 years old, home computers were just appearing
on the market.  The first machines that I remember to have been sold
in the large department stores in Berlin were the [Atari 400 and
800](https://en.wikipedia.org/wiki/Atari_8-bit_family) as well as the
[TI-99/4A](https://en.wikipedia.org/wiki/Texas_Instruments_TI-99/4A).
At the time, these machines were incredibly expensive, but me and my
friends often hung out in the stores to play around with these
machines and dream of owning them.

I found the TI-99/4A to be very good looking with its late 1970ies US
design and its nice keyboard.  In magazines I read that the machine
was slow and crippled, though, and the british home computers were
much cheaper, so I never became friends with the TI back then.

Recently, I found a machine cheaply on sale on an auction site and
bought it, despite the fact that neither the power supply nor the TV
modulator was included with the system.  The power brick was easy to
buy separately and I thought that I would not want the TV modulator
anyway.  I figured that the (european) TI outputs a component (YPbPr)
video signal and decided to try some conversion circuits that I found
on the Internet until I got something that one of my monitors could
display.

The first two attempts that I made failed, though.  One of the
schematics I had was a generic YPbPr conversion circuit based on a
bunch of opamps.  It did not produce a picture at all, but not knowing
enough about video signal encoding I did not manage to debug the
problem.  The same happened with another schematic that supposedly was
specifically designed for the TI-99/4A, but did not work for me
either.  Finally, I run across a new adapter circuit called
[TMS-RGB](https://tms-rgb.com/) designed by [Nicholas
Piegdon](https://twitter.com/npiegdon) just this year. 

![TMS-RGB PCB](/images/tms-rgb-pcb.jpg)

The TMS-RGB is a generic RGB converter for systems using the
[TMS9929A](https://en.wikipedia.org/wiki/Texas_Instruments_TMS9918)
chip, i.e. the TI-99/4A, some MSX systems and the ColecoVision.  It is
a tightly packed PCB with surface mounted components meant to be
soldered onto the solder side of the PCB of the machine to be
converted.

![TMS-RGB mounted on TI-99/4A board](/images/tms-rgb-on-ti.jpg)

To me, getting the PCB fabricated in China, sourcing the components
and soldering them onto the board with a microscope was part of the
fun.  It was not all that easy, though, and the TMS-RGB website has
some shops listed that sell the board ready for installation.
Installing it in the machine is very easy thanks to the excellent
videos published on the site.

The TMS-RGB converts the YPrPb signal to RGB + Sync, but it does not
change the overall timing.  This means that one either needs a monitor
that can deal with TV format RGB signals (i.e. one that has a RGB
SCART Input or a CBM 1084), or an upscaler that converts the signal to
match the expectations of a more modern computer monitor.  I bought a
cheap GBS-8200 upscaler board which produces a VGA output and it
worked fine.

![TI-99/4A board and GNS-8200 upscaler](/images/ti-and-upscaler.jpg)

In fact, the picture quality that I get with this on my TFT monitor is
just awesome.  The pixels are crisp and the is no blur, jail bars or
ghosting.  I can recommend the TMS-RGB board to anyone who wants to
equip their TMS9929A based hardware with a better video output.  The
overall cost for the PCB's, components and the upscaler board was
around 70 Euros.

![TI-99/4A Screenshot](/images/ti-crisp-image.jpg)
