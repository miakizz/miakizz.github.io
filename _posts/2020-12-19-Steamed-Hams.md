---
title: Steamed Hams
date: 2020-12-19 12:00:00 -0500
categories: [Portfolio]
image:
  path: assets/hams/thumb.jpg
  alt: Steamed Hams still
---
This one is going to be tough to explain. In 1996, the American TV show "The Simpsons" aired an episode "22 Short Films about Springfield". The title is a reference to _Thirty Two Short Films About Glenn Gould_, and the episode is a loose (_loose_) parody of Pulp Fiction. 20 years later, one of the longer segements which came to be known as "Steamed Hams" became a meme on Youtube. I have a few theories about why this happened, mainly that it's an incredibly short self contained story, that has the over-the-top line delivery and framing of a very short film, but with the narrative authority of a show like The Simpsons. and it's funny. Steamed Hams memes take the source material and modifies it in some drastic ways. Searching "Steamed Hams But" on Youtube will give you an idea.

The first one I made was inspired by [this video](https://www.youtube.com/watch?v=P0ZfgYvAP3A). I thought it would be neat to recreate it in hardware, as is the instinct of any self-respecting electrical engineer. And since I've always been known as the computer kid, I've collected/been given many computers/devices that can techincally play video. So the plan was to get as many computers I could together and play the clip in some kind of coordinated way. So, here is the breakdown of every device used
- My Dell XPS 13, which was briefly my daily driver before I got completely sick of it's battery life and Windows sleep problems
- A Ryzen 3600 desktop which drove 4 monitors: my fancy 32" Samsung, a monitor salvaged from a 2008 Dell, a Gateway display stolen from work while we were moving, and a projector given to me by a friend who was helping the local high school upgrate (thanks Peter!)
- An iBook G3 also salvaged from the same high school
- An iMac G4 bought at a storage auction
- A Raspberry Pi Zero driving a composite TV with BRAIN WASHER written on it which I found under a gazebo in Dover, New Hampshire. I can only presume that it was used in some yet unencountered punk music video
- A Dell Dimension whatever-the-fuck (Imagine a computer from 2004. That's it) driving a Macintosh II monitor with a custom VGA to CSYNC adapter, and an RF tv driven by the video card's S-Video output through an SVHS machine
- A Dell Insperion 1545 donated from my grandfather when he upgraded to an iPad
- A Dell Insperion One also donated from a grandparent
None of these were particularly coordinated, I just kept adding screens until I ran out of desk space and got scared of tripping a breaker. All the machines were hooked up to power and the local Ethernet.

![Computers being set up](assets/hams/lightson.jpg){: w="350"}
_Computers being set up_
![Mac Video Converter](assets/hams/vgaconv.jpg){: w="350"}
_Mac Video Converter_

The software stack for this was pretty interesting, since this had to coordinate playback of devices spanning 15 years and three operating systems. I drafted the videos in Premiere (a brief and scary two years when I didn't daily drive a Mac), and got the timings for when each video stream should start, and the speed it would need to play at in order to arrive at the reflex point at the same time. I settled on mplayer to play the streams for three reasons, which I found out as I was doing the project.
1. It's cross platform, and old enough to have builds for Mac OS Leopard, which was the oldest OS present
2. It plays a ton of different file formats 
3. It has a pipe interface that accepts ASCII commands to control playback in real time. I have no idea why this was added, it's barely documented, and incredibly useful for hacky applications like this. Combined with netcat means you can control mplayer over TCP.
Instead of keeping track of 12 different video files, the original was placed on all of the machines. Most could play the original h.264 without issue, some had to be scaled down to their native resolution, but the PPC macs and the Raspberry Pi Zero had issues playing any file. After LOTS of different encoding attempts, I found that a Cinepak in a MOV container worked the best. Cinepak was originally designed to be played off of 2x CD-ROMs, so it makes sense it worked well. However, ffmpeg has a nasty regression with its cinepak encoder (can you believe no one was updating that?) meaning it took almost 40 minutes to encode the 2 minutes of video. I have no idea how that even happens. Each IP address (and port for the machines running multiple displays) were put into a shell script that had 12 lines that would 
- connect to the listening socket of each machine
- set the speed to its assigned value
- seek to the beginning and pause
- Sleep for the time given
- and play the video

![Terminal running each netcat](assets/hams/lightsoff.jpg){: w="350"}
_Terminal running each netcat_

The playback went well, and I recorded it on my Sony a6300 with room audio. Despite being the dead of winter, at night, in Maine, my room was well over 80 degrees. I tried to open a window, but the plastic creeking of the CRTs dissuaded me from doing so.
{% include embed/youtube.html id='0x6OGMh4i98' %}

My other Steamed Hams project involved a board I layed out from this schematic that converts a composite video signal into an X/Y/Intensity map to display on an analog oscilliscope. My one clever "trick" was using a triple composite video connector instead of 3 BNC connectors, since RCA cables are way cheaper, and obviously work at video frequencies. The ladybugs in the video were a complete accident, and just a result of living in a poorly isolated house during the winter in Maine.

![Composite to X/Y/Intensity converter](assets/hams/compconv.jpg){: w="350"}
_Composite to X/Y/Intensity converter_
{% include embed/youtube.html id='MA7j0pZlcpQ' %}
- 