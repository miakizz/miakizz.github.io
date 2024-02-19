---
title: MarshwoodWX Weather Station
date: 2017-02-12 12:00:00 -0500
categories: [Portfolio]
image:
  path: assets/marshwoodwx/weather.png
  alt: MarshwoodWX layout
---
I was president of the Marshwood High School Science Club from 2015 to 2019. This may sound impressive, but it was more of an excuse to reserve a room after school for me and my nerdy friends to hang out in on afternoons that we didn't have math team practice. Our one club responsibility however, was management of the Marshwood Weather Station. A few administrations before us, the science club had purchased and installed a Davis Vantage Vue weather station, and got it installed on the roof.

![Weather Station mounted on roof](assets/marshwoodwx/roof.jpg){: w="350"}
_Weather Station mounted on roof_

To enhance the school expirence, the current data from the weather station was displayed on a TV in a well travelled hallway. The interface however...left something to be desired.

![Former interface](assets/marshwoodwx/old.webp)
_Former interface_

So we decided to write our own software. The weather station connects over ISM radio to the base station (looking back, the fact it could get through two floors of a cinderblock building and into a windowless room is impressive), which was connected to the computer over USB serial. The first attempt involved writing our own serial driver, which didn't work out for reasons I can't recall. Then, we found [the WeeWX Project](https://weewx.com/), which "interacts with your weather station to produce graphs, reports, and HTML pages". WeeWX is such a classic feeling early-2000s FOSS project, hulking codebase and all. Crucially, it included support for our receiver. WeeWX comes with many display skins, however none seemed well suited for our billboard application, so I wrote a custom one. I had recently fallen in love with Google's Material Design specifications (and still love it, to be honest) so we used their Materialize web framework to make a static webpage that the values could be inserted into using their web framework. 

![MarshwoodWX Running](assets/marshwoodwx/new.jpg)
_MarshwoodWX Running_

Looking back, it probably could've used a little more visual flare, but how much can a weather station really have? The club continued maintaining it, giving slight updates that mainly consisted of cron jobs and shell scripts to ensure the services would restart correctly on the ancient ThinkPad that was hosting it. Despite an aggressive postering campaign (which I still maintain I didn't approve), no one at school really got why there was a TV with the tempurature in the hallway, which the club liked. Hopefully someone followed Noble's maintaince instructions.

![MHS Science Club, circa 2019](assets/marshwoodwx/yearbook.jpg)
_MHS Science Club, circa 2019_
![Luckily instructions were provided](assets/marshwoodwx/noble.jpg)
_Luckily instructions were provided_