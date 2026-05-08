XTMax-only fork of [Ted Fried's MicroCore Labs Projects repo](https://github.com/MicroCoreLabs/Projects)

The original repo is huge, with many unrelated projects. Since I wanted to just
work on XTMax portion and not have to worry about whatever made the repo so
huge, I made this.

Description of the XTMax
========================
See [Ted's original 2024 blog post here](https://microcorelabs.com/2024/11/17/xtmax-8-bit-software-defined-isa-card-using-teensy-4-1/)

As the project exists now, it consists of a simple carrier card that plugs into
the 8bit XT bus of a PC compatible computer. A PJRC Teensy 4.1 is coupled to
the bus through some bus converter chips. The Teensy 4.1 has a MicroSD card
socket, an optional header for a ethernet jack, and an optional 16MB of
serially-connected RAM (so-called pseudo static RAM, PSRAM).

Additionally there is some sort of header for USB host and maybe other things,
but I haven't found the complete documentation yet. Additional info is welcome!
Simply cut a PR to this repo.

Plans
=====
Beyond the original advertised capabilities of the device, I hope to add the
ability to monitor the ISA bus to aid debugging drivers and other hardware.

I am also interested in emulating a simple video card whose contents can be 
accessed over the network.

Structure
=========

* `PCB/` contains a KiCad project and related files for the XTMax PCB. You will need one of these
* `Drivers/` contains the BIOS Extension ROM and MS-DOS drivers to use various features like bootable HDD emulation, EMS memory and UMB memory. The `README.md` in that directory is very thorough.
* `Code/XTMax/` contains the Arduino firmware for the Teensy

How I Made This Repo
====================
1. I forked the original repo on Github
2. I cloned it with filters when cloning to my laptop
   `git clone --filter=blob:none --sparse <this repo>
3. Change directory to the new clone.
   `cd XTMax`
4. Materialize only the files I wanted (the files in the `XTMax` directory).
   `git sparse-checkout set XTMax`
5. I made a new directory with a new bare repo
   `cd ../`
   `mkdir XTMax-split`
   `cd XTMax-split`
   `git init --bare`
6. split the subtree out back in the original
   `cd ../XTMax-sparse-checkout`
   `git subtree split --prefix=XTMax --annotate="(split)" -b split`
7. push it to the new repo
   `git push ../XTMax-split split:main`
8. make the bare repo not bare by changing `core.bare=true` to `false`
   `cd ../XTMax-split`
   `git config --file .git/config core.bare false`
9. Push to github
   `git remote add origin <another repo>
   `git push origin main`

Or at least, it was _something_ like that. I have to admit it was wierd git
trickery and I didn't actually achieve what I wanted:
1. original history preserved (YES)
2. no giant repo of random files (YES)
3. the ability to push changes back to MicroCoreLabs' repo (NO)

In the process, all the historical commits were evidently "replayed" and thus
you will see that the git commit hashes now differ between
1. here: [`(split)Add support for DOS Upper Memory Blocks (UMBs). (#38)`](https://github.com/eparadis/XTMax-split/commit/976976535f47e6fa837075ff0fd88a80a8bb1fbf)
2. there: [`Add support for DOS Upper Memory Blocks (UMBs). (#38)`](https://github.com/MicroCoreLabs/Projects/commit/0fbbc0070cc88edd90cb238c11304deac72d3a03)

_Oh well, I tried._


