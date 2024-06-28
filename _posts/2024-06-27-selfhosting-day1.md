---
layout: post
title: "Self-Hosting: Day 1"
author: "spockmay"
categories: journal
tags: [computing, programming]
image: cyber_tree.png
---

# Jellyfin
[Installation](https://jellyfin.org/docs/general/installation/container#docker) is trivial given that I'm using Docker. But configuration of the system is going to be the trick. I have my music files on a NAS, so I need to get Jellyfin to be able to read from there. 

After an excessive amount of research it looks like the "best" way to do this is a tool called `sshfs`, however my NAS doesn't support sshfs. It does however support NFS, so let's go in that direction!

## NFS Configuration
Using a [guide](https://www.howtoforge.com/how-to-install-nfs-server-and-client-on-ubuntu-22-04/) I need to setup my system as an NFS client. Turns out this is actually quite easy. The only gotcha was the need to configure the server to allow connections from the system's IP address. I then modified the system's fstab to mount on boot :)

## Setting up Jellyfin
I decided to use Docker compose to build my Jellyfin container. This means defining the `compose.yaml` file, which, thanks to [guide](https://pimylifeup.com/jellyfin-docker/) was frankly trivial.

In about ten minutes I was streaming my music through the local web interface that Jellyfin ships with (Agony by The Tossers).

## Android integration
There is an official Jellyfin app for Android, so I will start there. Installed via PlayStore, connected via TailScale, and boom, Budos Band is playing. Slick.
