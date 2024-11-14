---
title: Snooping on Voice Modems
date: 2024-11-14T00:32:29.722Z
categories: []
image:
    path: "assets/voicemodem/preview.jpg"
    alt: "A soviet style advertising that reads JOIN US ROBOTICS (https://jacfodor.com/U-S-Robotics)"
---
Dialup modems are split into three classes of capability: data, fax, and voice. Data modems exclusively transmit serial data, used for dial-up internet and server access. Fax modems have data capability, but can be put into a special mode to transmit and receive faxes. This is quite natural from a technical perspective since fax machines are essentially scanners connected to modems. Voice modems are data/fax compatible, and when set to a special mode, can pass digitized audio between the phone line and computer. 

> A modem's capability can be determined through `AT+FCLASS=?`: a return code of 0 corresponds to data, 1/2 correspond to level 1&2 faxes, and 8 corresponds to voice.  

Voice functionality is most commonly found in "Winmodems", which instead of relying on dedicated hardware demodulators, digitize the audio and rely on a Digital Signal Processor or the attached computer to actually demodulate the data. USB modems, and late-model external modems are also often voice capable. Voice capabilities were rarely used, and many owners likely didn't even realize their modem had the option. The most common use-case seems to be as a "computer answering machine", where a modem could pick up calls, record a message, and notify its owner via local alert, email, or calling a pager number. However, most people were fine with normal answering machines, and leaving a PC on 24/7 was uncommon in that era. 

I'm not sure if this software has become some kind of [third shaker](https://mavengame.com/2019/04/the-third-shaker/), or I need to improve my archivist skills, but I had a hell of a time finding any examples online. My perception is that it was often included with computers from manufacturers like Gateway and AST, or packed-in with voice modems, and rarely used. As such, there are plenty of archived webpages and forum posts mentioning how this kind of software is ubiquitous, but very few copies actually archived. Other types of program that use voice modems are similarly hard to run today – it seems to be split between ancient UNIX utilities and scammy Windows programs. There's a [Wikipedia article](https://en.wikipedia.org/wiki/Voice_modem_command_set), but it's sadly quite unhelpful in actually getting audio transmission working. Since I wanted to use my modem to record a phone call, it fell to me to do some reverse engineering. I installed an Lucent PCI Winmodem in a Windows 2000 box, and found a copy of _FaxTalk Communicator_ on a driver disc for another modem. After confirming the [voice functionality worked](/assets/voicemodem/meow.wav), I used [AccessPort](http://www.sudt.com/en/ap/) to snoop on the serial traffic to the modem to determine how voice data was transmitted and received.

Below is the information I was able to glean from documentation and the snooping. It isn't meant as a complete guide, but I hope it helps in getting something working which can be tweaked to suit your use-case. I've annotated the commands mainly using [this document](https://www.perle.com/support_services/documentation_pdfs/5500158.pdf), which assumes some knowledge of how AT commands are parsed. The inital run of commands were executed as the modem was receiving a call (denoted by a `RING` message). It can also be used when a call on the line is already in progress. It will start audio transmission from the computer – voice modems are half-duplex meaning they can either transmit or receive, not both simultaneously.

```c
//ATV:Human readable return-codes, 
//E0: No command echo
//S0=0: auto-answer after one ring,
//K3: hardware flow control
//M2: Modem speaker is always on
//L2: Modem speaker volume medium
ATV1E0S0=0&&K3M2L2

//ATE0: No command echo (not sure why this is sent twice, probably unneccessary)
//+FCLASS=8; set voice mode, 
//+VIT=0; disable DTE/DCE Inactivity Timer, 
//+VLS=1: Mode 1 – Modem off-hook, and connected to Telco.
//        Local phone provided with power to detect hook condition.
ATE0+FCLASS=8;+VIT=0;+VLS=1

//Unity gain for audio Rx
AT+VGR=128

//Unity gain for audio Tx
AT+VGT=128

//Set voice mode to mode 128 (8 bit signed PCM) at 8000Hz, and no silence compression
AT+VSM=128,8000,0,0

//Start voice transmission
AT+VTX 
```
At this point the modem will reply with `CONNECT`, and you can start sending 8-bit PCM audio. Be sure the data is sent raw, not hex encoded. `0x10` is used as an escape character, so each `0x10` in the audio stream must be replaced with `0x10 0x10`. I'm not sure exactly how framing works: the Wikipedia article suggests hardware flow control or stuffing your audio data to be exactly 115200 bps is common.

Receiving data is similar, but the final two commands are:
```c
//Set voice mode to mode 128 (8 bit signed PCM) at 8000Hz, and no silence compression
//+VSD:127,60 Enables silence detection at nominal level, after 60 seconds
AT+VSM=128,8000,0,0;+VSD=127,60
//Start voice reception
AT+VRX
```
Voice transmission is ended by sending `CRTL-C`, or the ended by the modem with DLE+! . 

Send and receive modes can be switched between during a call. I was unable to find information on how to originate a voice call, and _FaxTalk Communicator_ doesn't make calls, but I found worked well is dialing using `ATD5551212+VRX`, and proceeding as above once the modem returns `OK`, including a second `VRX`.