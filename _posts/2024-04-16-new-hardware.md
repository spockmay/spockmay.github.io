---
layout: post
title: "New Hardware to Play With!"
author: "spockmay"
categories: journal
tags: [computing, hardware, programming]
image: cyber_tree.png
---

I picked up a Raspberry Pi pico, 4B, and 5 from MicroCenter a couple days back. I also got one of the BBC micro:bit v2 boards!

## Raspberry Pi pico
I picked up this little guy for the RP2040 micro. As usual with all things Pi related there are other bits and bobs required, so I purchased the Pi debugger board/cables from [Sparkfun](https://www.sparkfun.com/products/21802) and (for kicks) an eInk display.

We haven't done much with this yet, but I have a ton of ideas how to use it around the office.

## Raspberry Pi 4B
This system is intended to run a giant 64" monitor that I have at the office for customer demonstrations. It needs to display high-def videos at clear framerates, run some pretty basic websites, and show presentations. I had been using a Pi3B for this role but it seriously struggled with the video. On good days I was able to get 1080p at 20fps, but that was pretty much peak operation. 

I had read that the Pi4 gets toasty, so in addition to a nice high-current power supply, I also purchased a passively cooled case. I really don't want fan whine coming from this TV all day every day. The case I picked is the <$10 Geekworm Aluminum Case [amazon](https://www.amazon.com/gp/product/B07ZVJDRF3). I am really impressed with the case's build quality. The aluminum is nicely shaped, there are no rough edges and all the ports were accessible easily. It even came with extra hex screws, a small hex driver, thermal gap pads, and a copper pad. The most confusing thing is why the screws on the Pi header side don't align with the PCB. I mean, seriously?! In all other respects, just beautiful design and build quality.

Sadly, I didn't realize that the Pi4 has no HDMI ports, rather it has two micro-HDMI ports. Argh, more adapters now have to be purchased so I can't even test the thing yet.

## Raspberry Pi 5
I'm debating about using this system as either a PC or an automation script server. We will have to see. I didn't buy a case yet because I really don't want to have to use active cooling. So we will test that out here soon.

## BBC micro:bit v2
Of all of the toys I got, this is the one I'm most excited about. I've been wanting to learn Rust and it looks like both this and the RP2040 have good [Rust support](https://docs.rust-embedded.org/discovery/microbit/). Since this is more "kid friendly" I'm going to start here then move to the RP2040. When I unboxed it I was pleasantly suprised to see that batteries were included! I got the kit connected and it played some really annoying beeping sounds and the LEDs did dances based on pushing buttons and shaking it.

I'm planning on giving one of these to the kids as they look to have a really nice web UI for writing both [code blocks](https://makecode.microbit.org/) as well as [Python](https://python.microbit.org/v/3). They also have a TON of ideas and courses and demos on their website. 