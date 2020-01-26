---
title: "PiDAB Progress Report 1"
date: 2020-01-26T06:41:18+01:00
tags: ["C++", "code", "Raspberry Pi", "PiDAB"]
---

I'm working on building a DAB+ radio to replace the one that I have in
our bathroom.  The one we have is not terrible, but there are some
things that I don't like.  Also, it does not have the few features
that I really want, and I am not aware of any commercial solutions for
them:

 * Fast startup:  I want the radio to turn on when I press the "on"
   button and not need seconds to start playing.
 * Play last episode: I want easy access to the last episode of
   periodic programmes that I like.

My radio will be based on a Raspberry Pi and a [FM/DAB tuner
module](https://www.monkeyboard.org/shop/index.php?main_page=product_info&products_id=272)
manufactured by Keystone.  This module implements a headless radio
with a serial interface for control.

To get started quickly, I got myself the [MonkeyBoard development
board](https://www.cartft.com/catalog/il/1618).  It contains a serial
to USB converter based on a PIC microprocessor as well as some
additional circuitry to control the headphone output.  No drivers were
needed for macOS, and the radio just shows up as a standard serial
port on the USB host.  I had to inquire with MonkeyBoard to get the
documentation for the module and the serial board, and they quickly
sent over schematics, source code for their sample application and
libraries as well as the description of the serial protocol.

The libraries provided by MonkeyBoard did not compile on macOS right
away.  Even though porting would not have been too difficult, I
decided that I rather want to implement the serial protocol.  This
gives me another opportunity to learn Modern C++ and discover how the
language has evolved over the 15 years that I have not been using it.

One Modern C++ library that comes in particularly handy when dealing
with the serial protocol of the radio is [Magic Enum
C++](https://github.com/Neargye/magic_enum).  It provides compile-time
reflection on enums which allows me to print the symbolic names of
command codes exchanged with the radio module in debug output.

I'm publishing the code to this project on
[GitHub](https://github.com/hanshuebner/pidab).  Follow the repository
or watch this space for updates.
