---
layout: post
title: "Self-Hosting: Day 0 Part II"
author: "spockmay"
categories: journal
tags: [computing, programming]
image: cyber_tree.png
---

Success! MBR vs GPT was the problem and my system got through the grub installation! The system now boots and is operational. 

System Specs:
* Intel Apollo Lake N4200 with dual Gb LAN ports
* 8GB DDR3
* 128GB mSATA SSD
* one extra m.2 slot :)
* fanless with auto-power on

I could have grabbed an ARM system with better specs, but am a bit worried about the support for arm-hd in this space. I'll keep my eyes open for ARM packages and may move that direction in the future for improved power consumption/performance.

## Tailscale
Tailscale is a bit of a cheat honestly. It's a free service that creates a SDWAN "private network" that easily allows devices on the same Tailscale network to communicate. There are self-hosted solutions that are similar or you could use a real VPN, but I've had trouble getting some of the inbound traffic to work though Spectrums CG-NAT in the past.

What this gets me is a trivial way to SSH into my box from my laptop or phone, regardless of where those systems are in the world or what network they are connected to. 

Setting up [Tailscale](https://tailscale.com/kb/1174/install-debian-bookworm) is super easy, just install the keys, setup the repo, and then apt install! 

# Security
Ah such a fun topic. Lol. In reality the first thing to consider here is how is my box going to be connected to the Internet and what risks does that open me up to. Initially, this system is going to be outbound connections only and anything inbound will be through Tailscale. This means I can skip quite a few of the normal steps.

## Basic Security
Out of the box Debian is pretty darn good, but there are a couple basic security things that simply have to be done to keep the box safe.
- disable password authentication over SSH [guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-debian-10)
- disable the root user from logging in [guide, see Step 4](https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/)
- firewall - lots of options here (ufw, firewalld, pfsense) but I'm leaning towards a simple ufw approach [guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands). For the moment my use of Tailscale means that I can keep the default "block all inbound traffic" rule enabled. Later if/when I decide to serve stuff to the open Internet I can adjust the rules.

## Security stuff I'm not going to do...yet
I'm not sure how far down this rabbit hole I want to go at the moment, but when I open up services I'll need to revisit this.

- move the SSH port to a "random" port number [guide](https://www.hostinger.com/tutorials/how-to-change-ssh-port-vps). This isn't really critical but it cuts down on a lot of the script kiddie nonsense if your box is open to the Internet.
- disable ICMP responses - basically don't respond to ping requests [ufw guide](https://www.configserverfirewall.com/ufw-ubuntu-firewall/block-pings/)
- fail2ban - used to dynamically block bad actors/port scanners
- [Snort](https://www.snort.org/) - used to monitor IP traffic for threats

# Applications
So the list of self-hosted applications is a bit...[extensive](https://github.com/awesome-selfhosted/awesome-selfhosted). My short/medium term goals are to replace Google Photos, Spotify, and then Dropbox. I would also like to move my Calibre ebook collection here eventually.

- Movies - I use Plex today hosted on my NAS. I'm pretty happy with it for now.
- Photos - [immich](https://immich.app) gets rave reviews and seems to be the most inline with what I'm looking for.
- Music - [Jellyfin](https://jellyfin.org/) is open-source and often touted as the best for both music and movies. Has good support for Roku and, more importantly, Android.
- File Share - I don't need/want the bloat of a NextCloud/ownCloud, but I do read good things about Seafile and Syncthing. They need to support Windows, Linux, and Android. More research required here...

# Install Docker
There are some strange behaviors you can get if you install the "native" version of Docker, so I prefer to install it from the [source](https://docs.docker.com/engine/install/debian/#install-using-the-repository).

Always good to see things work!
```
Hello from Docker!
```

There are some post-install tasks that need to be handled. I always use `sudo` with Docker for security reasons, but the services do need to be autostarted and log files need to be setup for [rotation](https://docs.docker.com/config/containers/logging/json-file/).
