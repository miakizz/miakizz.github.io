---
title: Cellular Rotary Phone
date: 2017-02-12 12:00:00 -0500
categories: [Portfolio]
image:
  path: assets/phone.png
  alt: Phone Opened Up
---
I completed this project at roughly 14 and it was my claim to fame for a while after. I had been exposed to rotary phones during one of Maine's very common power outages, since our cordless phones didn't work through a power cut. The three components of the phone were 
- an Arduino Pro Micro, which interpreted dial/hook voltages, and coordinated calls
- an Adafruit Fona 2G, an amazing board that is basically a Nokia 3310 on a breakout board. It broke out speaker/mic pins, and had a serial interface. It's sadly been discontinued with the phase-out of 2G, and nothing VoLTE has really replaced it.
- a Sparkfun WAV trigger, for playing the ringer sound effects. It is basically a board that will play short audio effects when a pin is triggered.

This project originally used schematics from an ancient Sparkfun to generate a voltage to use an actual ringer (this project has been done a few times by other folks. Convergent evolution will get you every time). However, my electrics skills/equipement were woefully unprepared, and I kept burning out H-bridge chips. The WAV trigger connected to some salvaged laptop speakers did the trick.

Connected all together, with a LiPO battery stuffed inside, and it performed like an honest to god rotary cell phone. Nice going, 14 year old Mia. 

In a strange cota to this story, somehow my _very_ enthustiac science teacher found out about it, and had me bring it in to show off. Earlier that month, my school had to shut off the water supply due to fertalizer contamination. The local grocery stores saw the publicity opportuinity, was supplying us with pallets of water bottles. This is how I ended up showing this off to two Hannaford's executives, who reacted in a way I can only describe as confused amusement.