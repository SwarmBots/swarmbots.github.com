---
layout: post
comments: true
title: Electrical Update
type: post
date: 2013-04-18 1:41:29
---

##Electrical Update

>We managed to put together our first nearly full prototype last night and ran into some issues. Got our nice protoboards from youdoit and put our arduino, h-bridge and programmer down onto the board, but we ran into some issues. 
>
>Firstly the documentation for building the programmer we were looking at was inconsistent and relied on parts we didn't have, which meant that when we actually put it together, it worked, but then the atmega chip got extremely hot and the motors stopped running. We are moving to an ftdi chip to program the chip, which should get around those issues and seems to have a really nice interface and extremely good documentation.
>
>The other issue we're dealing with is a power concern. We want to power each robot with a single 9V battery, though we've realized that we have more than a couple things to power and none of them work directly on 9V. Firstly we want to power the motors through the H-bridge with ample power so that we maintain some form of speed control over them.
>
>Secondly we want to power the arduino with the same 9V, assuming we will use a 5V regulator to make sure nothing gets burned out. We want to power the mouse encoder through the arduino, but since it only draw about 10mA it shouldn't be a big issue. Since our protoboard doesn't have a lot of space we're going to try to put the regulator inline with the power source. Hopefully this electrical system will all be done tonight.

{% include disqus.html %}
