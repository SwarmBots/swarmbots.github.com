---
layout: post
comments: true
title: Electrical Pivots
type: post
date: 2013-05-07 10:45
---

##Electrical Pivots

>Over the course of our project we came across many issues that we hadn't planned for and had to pivot our design. In this post we hope to detail them so that future teams that might run into these same pitfalls know what worked for us and what to avoid.

>Our first major electrical pivot was on the subject of sensors and microcontrollers. We realized that not only did we not have room in our budget for either SHARP sensors or motor encoders, but we also unfortunately ordered the wrong chips (Important: The ATMEGA328-PU and the ATMEGA328P-PU are different chips. The second one is supported by Arduino.)

>We came across a wonderful hackaday in which someone was able to use an old PS2 mouse to work as a location tracking system and decided to use that to sense position. We also decided to move to a combination of an LED and a phototransistor for obstacle detection with the idea being that when the swarm bot got close to an obstacle it would see enough of its own light reflected that the phototransistor would trip.

>In hindsight we should have just re-ordered chips as soon as we realized that we had gotten the wrong ones, but instead we sunk a lot of time into getting the chips to work and actually succeeded. We used the Optiloader library and manually changed the address of the ATMEGA328P in our avrdude.conf file to the address of the ATMEGA328.

>We originally had also planned on powering everything through a number of combined button cells, though once we realized that they internal resistance was on the order of kiloOhms we realized this was no longer possible. We tried to make out own battery pack out of AAA Ikea batteries, but eventually just decided to use 9V.

>Once we changed our power source we also had to add a number of voltage regulators both to 5 and 3.3V at 250 mA to power which took up the small amount of space we had left on our boards. We also didn't realize that our chips required external crystal oscilators to function, which took up still more space.

>Eventually we scrapped the whole idea for obstacle detection and decided to rely more heavily on the mouse encoder's sensing abilities due to a combination of a lack of pins left on the arduino chip and a lack of space on our circuit to correctly step up the output of the phototransistor to match the high/low values on an ATMEGA.

>Our final pivot was more drastic and less pleasant than the rest of these. 2-3 days before the project was due we came to the realization that in fact all of our ATMEGA chips save one were broken. Important note: don't solder near the chips. Don't have them near hot things and make sure that they are well protected. Also only prototype with one chip if you think you're going to use them all so that if one breaks, only one breaks.

>We went out to youdoit and bought five new ATMEGA328P-PU's with the assumption that they would just work since they were the correct chips, but our setup for the previous chips turned out to not be compatible. In hindsight, trying to change chips at the last minute wasn't a fantastic idea, but at that point we had very little choice. We switched to using full arduinos as the size wasn't that much larger than the board we had made and we were using every output on the board.

>Finally, some of our best take aways on the electrical system were to heat shrink every joint that you can get to, always use solid core at the end of your stranded core wires, use legitimate(barrel) ports instead of wires for your batteries so they can be easily removed, and never ever use dip sockets with components that you're going to be moving around. Spring for the female header pins, even if the ece stockroom doesn't have them grab some from youdoit and snap them with pliers or clippers to fit your needs. You will be so glad you did. 

{% include disqus.html %}