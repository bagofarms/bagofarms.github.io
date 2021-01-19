---
layout: post
title:  "Water-damaged Game Boy Color: Fixing the Speaker Output"
date:   2021-01-18 12:30:00 -0500
tags: consoles gameboy repair
---

In my [previous post]({% post_url 2021-01-15-water-damaged-gbc %}), I cleaned up the button contacts on a seemingly water-damaged Game Boy Color.  Everything seemed to be working except for the fact that there was no audio from the built-in speaker.  Well, today I figured that out.

## Does the audio work at all?

I have had other Game Boys where the onboard speaker does not output sound, but headphones work, so that's what I tried first.  I plugged some speakers into the headphone port on the bottom of the GBC, and the sound worked!  That was a huge relief.  I knew that the headphone port has a switch inside it that is normally `on`.  When headphones are plugged in, it changes the switch to the `off` position.  So there was most likely something wrong with that switch.

## Cleaning the Switch

I wasn't sure where exactly the switch was located inside the headphone port, so I did some digging and found [this excellent post on Sam's Asylum](http://samzasylum.blogspot.com/2015/05/gameboy-color-speaker-repair.html) that showed exactly where to clean.  Luckily, the designers of this headphone jack left a little window in the case to allow me to clean the switch without desoldering it from the board.

![corroded headphone port](/assets/img/water-damaged-gbc-headphone-battery-before.jpg)

I inserted a headphone cable to open the switch and make the contacts accessible.  Using a set of precision tweezers, I gently scraped off the oxidation from both sides of the switch.  When I was done, I used some canned air to blow out any oxidation dust and put the GBC back together enough to test it.  Aaaaand the speaker still didn't output any sound.

## Ok, how does this circuit even work?

At this point, I knew I had to start poking around with my multimeter.  I found this extremely detailed [schematic for the GBC](https://console5.com/techwiki/images/e/e6/Nintendo_GBC_Schematic.png) from Console5 and dove in.

![Game Boy Color Headphone Port Schematic](/assets/img/gbc-headphone-port-schematic-marked.jpg)

I've cropped the schematic to focus on the headphone jack, which is marked as `P5` on the right side of the image above.  The red arrow points to pin 5, which is is the side of the switch that connects to pin 6 (`SW`) of the sound amplifier `AMP-MGB` (`U3`) on the left side of the image.  Pin 4 of `P5` is connected to ground.  When there are no headphones connected, the switch is closed, connecting pins 4 and 5 of `P5` together, which connects the `SW` pin of `U3` to ground.

You may have noticed that I'm ignoring `EM4` (part number [BLM11B102S](https://docs.rs-online.com/ae7f/0900766b80021680.pdf)), which is in-between pin 5 of `P5` and the `SW` pin of `U3`.  This is just a ferrite bead, and is most likely there to filter out noise.  It's good to have, but not absolutely necessary.

## Probing

I set my multimeter into continuity mode and found the `SW` test point on the Game Boy's PCB.  I touched one probe of the multimeter to that, and the other probe to pin 5 of the headphone jack (the red arrow in the image below).  Unfortunately, I did not get a tone, so there was a break somewhere in the trace (the yellow line in the image below).

![The GBC headphone jack and EM4](/assets/img/water-damaged-gbc-em4-marked.jpg)

Fortunately, I quickly found the break.  I probed both sides of EM4, which showed continuity, so I tested both sides of the circuit from EM4.  However, there wasn't continuity on either side!  I wondered if the oxidation got under the pads on the PCB and lifted them up, so I scraped off some of the coating to reveal the traces on either side of EM4.  I tested the continuity from one side to `SW`, and there was continuity.  Likewise with the other side and pin 5 of the headphone jack.  I knew my suspicions were correct.

## Fixing the Problem

I tried to reflow the solder of `EM4`, but the component was so light that it lifted off the board as soon as I touched it with the iron.  I tried to bridge the trace and the pad with solder, but I wasn't able to do it.  In the end, I decided to bypass `EM4` entirely with a piece of very thin (bodge) wire connected between the `SW` test point and pin 5 of the headphone jack.  I was careful to route it around the button pads, cartridge connector, and screw holes.

![bodge wire between sw and pin 5 of headphone jack](/assets/img/water-damaged-gbc-speaker-fix.jpg)

And... it worked!  The audio comes out of the onboard speaker with no issues, and switches off when headphones are plugged in.  I normally don't like to bypass filters like that, but I'll address that if the audio quality is a problem in the future.  In the meantime, my wife's coworker can show this off to her daughter!

<video controls>
    <source src="/assets/video/water-damaged-gbc-repaired.mp4">
    Your browser does not support the video tag.
</video>