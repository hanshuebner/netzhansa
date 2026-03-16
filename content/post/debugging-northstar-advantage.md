---
title: "Resurrecting a NorthStar Advantage computer"
date: 2026-03-16T09:17:34+01:00
draft: false
---

During a recent visit at the
[Oldenburger Computermuseum (OCM)](https://computermuseum-oldenburg.de/), I spotted two NorthStar Advantage computers in their reserve collection.  I'd used a friend's NorthStar Advantage to lay out our school magazine using WordStar back in 1983, so I was immediately intrigued.

We pulled them into the workshop and gave them a quick test.  One asked for a boot medium, while the other displayed garbage.  I offered to take them home for maintenance and repair, as I'm always happy to work on machines that I can return once I'm done.

The NorthStar Advantage is an all-in-one system that integrates the computer, monitor, keyboard and disk drives into one unit.  It comes with a Z80 CPU at about 4 MHz, 64 kB RAM, two 5.25-inch diskette drives (or, optionally, one diskette drive and a 5 MB hard disk).  Its standout feature was a 640x240 monochrome framebuffer backed by 32 kB of dedicated display RAM - unusual for the era, which mostly relied on character-based display controllers.

Despite its capabilities, the machine was not a huge success.  The base unit was expensive, and it used hard-sectored 5.25-inch diskettes instead of the common soft-sectored variety - a format that was harder to find and more expensive.

# Fixing the faulty machine

Back home, I started to diagnose the faulty machine:  When switched on, it displayed garbage on the screen.  Sometimes, the machine beeped and "LOAD SYSTEM" message that the ROM displays to prompt the user to insert a bootable floppy was partially visible.  Sometimes, it would just beep after a while and sometimes, it would just do nothing.

To me, this sounded as if there was a problem with one or more of the 4116 dynamic RAM chips.  The machine has 52 of these chips, and they're known to go bad over time.  Luckily, they were all socketed, so it was a matter of pulling them out and testing them with Tube Time's excellent [Pico DRAM Tester](https://github.com/schlae/pico-dram-tester).  The tester found 8 chips to be faulty, but after I replaced them with working chips, the system's behavior was still the same.

## Using Claude Code as Hardware Debugging Assistant

I described the fault to Claude Code and also gave it access to the manual, schematics and boot ROM disassembly, planning to use it to assist me during the debugging process.  It pointed out that the boot ROM uses the display memory as stack after the machine was reset.  With this information and the observation that the garbage displayed was not always the same after I'd switch the machine on, I decided that I wanted to know whether the CPU would be able to execute code reliably when avoiding the display RAM.

First, I needed a way to run custom code in the machine.  For that, I used a memSIM2 EPROM emulator from [MOMIK electronics](http://www.momik.pl/) with an adapter to make it work in the 24 pin socket for the 2716 boot EPROM.  As I work on this project with Linux, I installed the [memSIM2 tool by Nils Eilers](https://github.com/nils-eilers/memSIM2) so that I could upload test programs to the emulator through USB.

For starters, I had Claude Code write me a simple test that performs the necessary machine initializations and then create a beep.  That program would not require any RAM at all and if it ran, it would tell me that the machine basically works - and it did.  Next, I had it extend the program so that it would test the display and main RAM and provide me feedback with beep signals.  This showed that the main RAM worked, but the display RAM did not.

What Claude could not really tell me is the why.  Trying to do so, it sent me down a rabbit hole of measuring ripple and noise on the power supply rails and trying to fix that, but I eventually compared the broken to the working machine to learn that the noise was normal.  While probing around, however, I noticed a thing that I observed earlier, but did not really give enough attention:  The garbage that appeared on the screen faded away over time, and within a couple minutes, the screen became completely black.  That, for sure, would be a thermal issue and confirming that just required cooling down the board with some cold spray.  With that, I could identify one SN74LS164N chip (a 6 bit register) that would would react on being cooled down with a visible response on the screen.

As the chip was not in a socket, I needed to be quite sure that the chip was somewhere in the path between the display RAM, the CPU and the video logic so that i cause the visual effect that I was seeing when failing.  Again, Claude Code was helpful, even though its understanding of schematics is somewhat llimited.  Eventually, I swapped the chip and that completely removed the failure and brought the machine to work.

I created a couple more in-depth RAM tests and gained confidence that everything was in order from that perspective.  I found Claude Code to be quite reliably create assembler programs that would do what I needed, and it was instrumental to being able to diagnose and fix this issue.

## Why stop here?

While the machine was no longer completely broken, it still only asked for a boot floppy which I did not have.  While disk images for the system software are available on line and I could also borrow a very small number of empty diskettes from a friend, I thought it would be nice if I could run some more interesting software by using a custom boot ROM that'd accept binary code on the serial port.  Thus, I prompted Claude Code to create a boot ROM that reads [Intel HEX files](https://de.wikipedia.org/wiki/Intel_HEX) from the serial port.  That way, I could load bigger programs into the machine and not be limited to the 2 kB of the boot ROM.

While interactively getting the serial port communication to work with Claude Code, I also prompted it in another window to create a high performance graphics library with support for lines, circles, rectangles and sprites as well as a demo program.  To do so, it consulted a lot of web sites with Z80 coding techniques and iterated a couple of times over what it had created before it had something that I could upload using the hex file boot prom.  And again, it worked well enough to be promising for further refinement.

Three days later, I had a sprites demo, a Pac Man game (fully playable with a reasonable player AI for demo mode), a side scrolling demo and a simple vertical shooter.  The performance of all of these programs is very reasonable given the hardware constraints, and I did not touch any of the code that Claude Code generated.

## Conclusion

- to be written -
