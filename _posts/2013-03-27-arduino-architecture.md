---
layout: post
comments: true
title: Arduino Architecture
type: post
date: 2013-03-27 9:17:25
---

##Arduino Architecture

> For robots of any size, the choice of brain can be of critical importance. While we plan to offload much of the computation to remote computer, there is still a fair amount of computation that needs to be done onboard each one of our SwarmBots.
> Since our class started out working with Arduino Unos, all of our team members have a familiarity with the Arduino platform, including writing simple programs. Unfortunately, it is not practical to have a large, $20 development board on each one of our reasonably small robots. For this reason we decided to use standalone ATMega328P microcontroller chips. These are the same ones found in the Arduino Uno, but with the development board.
> Normally, working with microcontrollers without a development board can be relatively tricky. To program them, you still need another Arduino or programmer capable of ISP programming. In order to simplify that step, we will be using chips with a specific [bootloader][] that allows us to program them directly via USB from any computer.
<div class="center container"><img class="postImage" src="/img/homemadearduino.jpg" alt="Home-made Arduino"/></div>

{% include disqus.html %}

[bootloader]: https://github.com/KevinMehall/FiveDollarArduino "Five Dollar Arduino"
