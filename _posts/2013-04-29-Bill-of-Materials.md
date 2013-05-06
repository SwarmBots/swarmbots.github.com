---
layout: post
comments: true
title: Bill of Materials (for each Robot) 
type: blog
date: 2013-04-29 2:01
---


##Bill of Materials (per robot)
| Subsystem     | Item (file name if applicable)   | Qty | Material| Website (if Applicable)    |
| Mechanical    |Wheel (wheel_2)                   | 2   |:----------:|:----------:| 
| Mechanical    |spacer .650" diam (spacer_625diam)| 2   |:----------:| :----------:| 
| Mechanical    |spacer .500" diam (spacer_500diam)| 2   |:----------:| :----------:| 
| Mechanical    |side panel (SidePanel_lasercut_2) | 2   |:----------:| :----------:| 
| Mechanical    |top panel (top_lasercut_2)        | 1   |:----------:| 
| Mechanical    |cross braces (crossbrace)         | 2   |:----------:| 
| Electrical    | H Bridges                        | 1   | 5V         |
| Electrical    | Motors                           | 2   | 5V         |
| Electrical    | LEDs                             | 2   | 5V         | 
| Electrical    | Mouse Encoder                    | 1   | 5V         |
| Electrical    | AtMega Chip                      | 1   | 5V         |

Total Wattage: 2.65 Watts
Total Amperage: 467 mA

So our plans for power are using 6 AAA alkaline batteries (which each have 1.5 V and 180 mAh)
These consist of packs of three batteries in series with two of said packs in parallel for extra capacity.
3 batteries gives us 4.5V with 180 mAh and 6 batteries in our configuration gives 360 mAh.
Note that alkaline batteries under 500mA draw die quickly...

{% include disqus.html %}