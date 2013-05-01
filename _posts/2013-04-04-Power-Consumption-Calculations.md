---
layout: post
comments: true
title: Power Consumption Calculations 
type: blog
date: 2013-04-04 2:01
---


##Power Consumption Calculations

> So our plans for power are using 6 AAA alkaline batteries (which each have 1.5 V and 180 mAh). These consist of packs of three batteries in series with two of said packs in parallel for extra capacity. 3 batteries gives us 4.5V with 180 mAh and 6 batteries in our configuration gives 360 mAh. Note that alkaline batteries under 500mA draw die quickly...
>
> **Update** 5/01: We came to the obvious realization that we would be much better off not trying to build our own unreliable battery packs. In liu of this, we are powering each one of out bots with a single 9V battery regulated to 5V and 3.3V where needed. We believe that having one fewer component to potentially debug will allow us a smoother testing and ultimately lead to fewer headaches down the road.


| Item         | Qty   | Voltage (V)| Amperage (mA) | Wattage   | Total Amperage| Total Wattage |
|:------------:| :---: |:----------:|:-------------:|:---------:|:-------------:|:-------------:|	
| H Bridges    | 2     | 5V         | 100 mA        | .5 Watts  |  200 mA       | 1 Watts       |
| Motors       | 2     | 5V         | 100 mA        | .5 Watts  |  200 mA       | 1 Watts       |
| LEDs         | 2     | 5V         | 20 mA         | .1 Watts  |  40 mA        | 0.2 Watts     |
| Mouse Encoder| 1     | 5V         | 25 mA         | .125 Watts|  25 mA        | .125 Watts    |
| AtMega Chip  | 1     | 5V         | 2 mA          | .1 Watts  |  2 mA         | .1 watts      |
<br/>
Total Wattage: 2.65 Watts
Total Amperage: 467 mA



{% include disqus.html %}



