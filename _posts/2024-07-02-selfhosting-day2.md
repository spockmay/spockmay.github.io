---
layout: post
title: "Self-Hosting: Day 2"
author: "spockmay"
categories: journal
tags: [computing, programming]
image: cyber_tree.png
---

# Immich
One of the big goals of my self-hosting project is to have a better way to view my photos. I've taken many thousands over the years and currently the only way for me to look at them (or look through them) is by browsing the files individually. Thankfully tools like Irfanview make this pretty easy but I still have to basically scroll through every photo individually in high-res. This makes it impossible to do in a fun way for friends or family. I also don't have a way to share access with anyone outside the household. Until recently I didn't even have access to the photos from outside my house until I setup Tailscale on the NAS. The other slightly annoying part of the process is that when I take photos on my phone, they are automatically uploaded to my Dropbox. I then have to manually copy the files out of Dropbox to the NAS. I could have scripted this, but I do it so rarely it's just not worth it - [XKCD 1205](https://xkcd.com/1205/).

## Installation
[Installing](https://immich.app/docs/install/docker-compose) the application was shockingly easy, even for a Docker-based deployment. I think there are literally three options to configure to get a default instance up and running.

![immich welcome screen](https://spockmay.github.io/assets/img/_welcome_to_immich.jpg)

After install, there are a few steps you perform using the local webapp [guide](https://immich.app/docs/install/post-install). This took maybe another 5 minutes and then I was ready to upload photos!

## Photo Storage
I just grabbed some stuff out of my Camera folder in Dropbox and threw them into Immich. Even over Tailscale the process was pretty quick, most of the time was spent in the "uploading" phase with less than a second spent processing each uploaded image. I uploaded 906MB of images and they take up 1023MB on disk. That's about 13% overhead so not bad. Given that my recent-ish photos take up approx 150GB, I'll need an additional 170GB of disk space to store the immich image data.

The files aren't stored in a flat file system (at least as of version 1.107.0). This means that we can't just "scan" an existing directory like Jellyfin, rather each photo has to be uploaded into immich directly. Luckily, there's a new-ish [merge request](https://github.com/immich-app/immich/pull/3124) that adds this feature as well as provides the documentation. Basically it allows you to create one or more external libraries that use that filesystem as the truth model and then all the metadata/thumbnails/etc are generated. The original files aren't modified at all but still show up in the UI like any other uploaded photo. There's even a nice way to ignore specific files, e.g. RAW format stuff, which I will need to test out.

## UI
The User Interface is really slick, I'm seriously impressed. There is very granular control over user accounts, even OAuth integration. You can style the page to fit into other apps as well!

# Next Steps
I need to move my data storage from the local disk to a NFS share on my NAS. That will require rebuilding the container and making modifications to both the system's fstab and the yaml file, see [details](https://immich.app/docs/features/libraries). Once I'm done running a fresh photo backup to Glacier, I'll test out the External Library to import all my old photos and see how that works out. 

Other random tasks:
- [ ] Save and auto-load the config file [guide](https://immich.app/docs/install/config-file)
- [ ] Test out the Android app and the [automatic backup](https://immich.app/docs/features/automatic-backup) feature
