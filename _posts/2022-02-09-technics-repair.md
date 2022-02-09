---
layout: single
title:  "Repairing a 1970s Analog Amp"
date:   2022-02-09 13:14:15 -0700
categories: tips
read_time: true
---

When he was in his 20's, my father invested in what was at that time a very high-end sound system: a "hi-fi" manufactured by [Technics](https://www.technics.com/us/), then the premium line of Panasonic.  We have dated the hardware to about 1978; for over forty years since then, it provided great sound to his -- and now my -- home.  It may only have two channels, but something about the analog filters gives a richness to the audio that modern digital processing seems to lack.

Until one morning when it wouldn't turn on.  In those days, equipment was made to be repaired: so I decided to see if I could figure out what was wrong.  I opened up the box, and immediately I could see that the fuses into the power supply were blown, so something was shorting to ground.  Since they're discrete, I could tell that the power electronics rectify the analog input down to 20V and 5V DC rails.  From there, my multimeter made clear that it was the 20V circuit shorting out.  But where?

Discrete components are magical: they can be tested individually!  I ruled out all the caps, and then began testing the diode drops and reverse biases on the transistors, which all checked out.  That left only the integrated power amplifier as a potential suspect.  Amazingly, the data sheet for the Sanyo STK-043 was available online, and I was able to see that there are four power pins: a + and - for each of the right and left channels.  Sure enough, there was no resistance between those two pins on the right channel!

<figure>
  <img src="{{ '/assets/images/posts/amp-repair/p0.jpg' | relative_url }}" alt="Sanyo" class="full">
  <figcaption>The Sanyo STK-043 integrated power amplifier.  Inside the package are two Darlington pairs, one for each channel.</figcaption>
</figure>

Perhaps even more amazingly, that integrated amp is *still available.*  For about $20 on EBay, I was able to buy a brand new unit.  This was also a good opportunity to buy a solder-sucker, so that I could (as cleanly as possible) remove the old solder along with the old amp.  I don't have the steadiest hands, but I was able to do a reasonably good job of installing the new one onto the (single layer, hand-traced!) PCB:

<figure>
  <img src="{{ '/assets/images/posts/amp-repair/p1.jpg' | relative_url }}" alt="solder" class="full">
  <figcaption>Slight scorching on the PCB, but clean joints! The black fins on the lower of the image are the Sanyo's heat-sink.</figcaption>
</figure>

Holding my breath, I pushed the power button -- and the fuses held!  Connecting the amp to a source and cranking it up, the analog meters show about a watt of power headed out to each speaker.  Hopefully I'm able to get another 40 years out of this unit ... but just in case, I ordered two STK-043's.

<figure>
  <img src="{{ '/assets/images/posts/amp-repair/p2.jpg' | relative_url }}" alt="amp" class="full">
  <figcaption>Repaired amp driving 1W on each channel!</figcaption>
</figure>
