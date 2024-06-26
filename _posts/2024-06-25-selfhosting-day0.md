---
layout: post
title: "Self-Hosting: Day 0"
author: "spockmay"
categories: journal
tags: [computing, programming]
image: cyber_tree.png
---

I've already done some pretty trivial self-hosting by using a Raspberry Pi3B for DNS and whole-home ad block using [recursive PiHole](https://docs.pi-hole.net/guides/dns/unbound/). It's a pretty fully baked solution and runs well. I also have a local Plex server that hosts all of the movies that I've ripped over the decades. Over the past couple months I've been thinking about upping my self-hosting game. There are a few places where I would really like a better solution than I currently have, in particular image hosting and music hosting. I've never really used Google Photos other than what my Android phone has done by default. It's nice, but I don't want to upload all my old photos as well as all the expected privacy concerns about giving _more_ info to Google. On the music side, as Spotify keeps increasing their price I keep being more drawn to my old music collection and considering dropping that service.

To make any more progress though, I need a machine to run the services. Since I'm still intimidated by containers I'm going to go that route. I have an old PC from work that I'm planning to use but I need to first install Linux on it. I'm trying Debian Bookworm, but have been running into issues getting grub installed all day long. Frustratingly the issue only presents when the installer would `grub-install dummy` which is after about 15 minutes of configuration and waiting. After some more research I think the problem is due to this PC requiring MBR instead of EFI. I downloaded [Rufus](https://rufus.ie/) since it allows you to select the partition type and will try again tomorrow...

Once I get the PC setup, the first order of business is to install TailScale. Then I need to dig up my research on Photos and Music replacement tools!