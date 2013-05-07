---
layout: post
comments: true
title: Major Changes in SwarmBots Software
type: post
date: 2013-05-07 10:58
---

##Major Changes in SwarmBots Software

> Over the course of any project as ambitious as SwarmBots, you have to constantly re-evaluate your work, and possibly adjust your goals.
> In the case of SwarmBots, many changes were made for our final deliverable, including changes in software.
> While the communication architecture largely stayed the same, some of the most prominent changes came in how the bots are interacted with. In favor of having a working demonstration, we opted to have the user controls change from sending specific commands to simply making all bots listening move forward. This decision was made due to the amount of time that it took to get radio communication between the master and the bots, meaning we had less time to work on parsing messages on the Arduino.
> This leads into another major change - switching nRF libraries at the last minute. We had been having no problems with the library we had been using for weeks, but on the day before our demonstration, our radios simply stopped working. We feared that perhaps there was something wrong with the hardware, and all of our radios were useless. However, after testing the radios with a different library, we found that they did in fact still work. Seeing as the radios worked with one library and not the other, we re-wrote our Arduino code to use the new library, scrapping the the old one.

{% include disqus.html %}