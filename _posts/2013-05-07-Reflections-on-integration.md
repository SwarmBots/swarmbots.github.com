---
layout: post
comments: true
title: Reflections on Integration
type: post
date: 2013-05-07 10:50
---

##Reflections on Integration

One of the major benefits to taking PoE before SCOPE is that it allows students who aren't usually exposed to a full integration of a mechatronics/robotics project to participate in the design, fabrication, and execution of a complex system.

Important pivot points:


> PoE teams Assigned 2/28
>Decided to order equipment 3/11/13
>Spring Break 3/18/13 -- hardware arrived this week. Did not end up scheduling team meetings because 1 team member was on vacation.  In hindsight our very demanding plan we had created was thrown off because we didn't schedule any meetings.
>Design Review 1 3/28/13 -- demonstrated first prototype. Not perfect mechanically.  In hindsight we should have started to tackle the "make an arduino from scratch" problem about here.
>Design Review 2 4/18/13-- demonstrated next iteration of prototype in Solidworks. Radios are able to pass code back and forth. Able to send tweets to robot.
>First pass of lasercut parts done 4/22/13-- we manufactured bot out of acrylic instead of foamcore.  Bot worked but was hard to put together and was more flexible than desired, so we made plans for another revision. 
>Additionally, we wanted to engrave "Olin SwarmBots" on our robot, which turned out to be much more difficult than anticipated (changing line thickness/color of text is not trivial in Solidworks).
>2nd and 3rd pass of lasercut parts done 4/29/13 -- manufactured two different designs to see if one would work better than the other. Spring loaded iteration worked well but still needed glue to hold in place.
>Crossbraces allowed for easier gluing of pieces at 90 degree angles. Much stiffer box.
>Robots assembled with electronics 5/1/13. Radios suddenly broke; not enough experience on team to know if it was due to radio chips being fragile or code library changes. Changed to Mirf library and luckily Luke Metz was free to drop by and help with the C code.
>Demo Day 5/2/13. Could only move robot forwards through a twitter command. Nominally had the capabilities for more complex turning and speed control, but our eventual radio message relied heavily on bit-hacking, which wasn't amenable to substantive message changes.


>Recommendations for future teams:

>Start with the point between two projects. We started building out software and individual electronics and didn't really work towards the radio communication and on-chip Arduino code until later. We should have started with a robust radio connection, and then filled out the software and electrical components the project would have gone more smoothly.
>Make a fully integrated prototype early on.  Even if you have lots of iterations on the mechanical side, having iterations on the electrical side would help a lot with the process too.  
>There are always bugs no matter what system you're building.  The electrical system went through many hiccups because we were unclear about the types of batteries we would be able to use to drive our robot (the amperage your batteries can give without dying instantaneously is hugely important!), we didn't buy the right type of Arduino compatible AT Mega 328 chips (and if you pay a dollar more you can get them flashed), and we didn't fabricate the boards early enough to realize most of these issues until pretty late in the semester.
>Follow through with the plan you make. Edit the plan as necessary but keep referring to it.  If we had designated someone to keep us on track we probably would have been more efficent throughout the project.

{% include disqus.html %}
