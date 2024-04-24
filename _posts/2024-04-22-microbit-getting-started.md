---
layout: post
title: "BBC micro:bit v2 - Getting Started"
author: "spockmay"
categories: journal
tags: [computing, hardware, programming]
image: cyber_tree.png
---

I was anxious to get started playing with the capabilities of the micro:bit, so I loaded up their online [IDE](https://python.microbit.org/v/3/) and started playing! Unfortunately I very quickly discovered that the download functionality only works in Chrome...grr.

I did some small silly projects first to get familiar with the interface.
- roll a d6 when you push a button
- roll a d20 when you push a button (this was a bit annoying as you can only display a single digit/character on the screen, so the value has to be displayed using the scroll functionality
- display the current temperature
- display the heading

I then decided to make it into a rudimentary compass (since I've been doing a lot of orienteering lately). The basic idea is anytime the A button is pressed to draw an arrow pointing as close to North as possible. Since the micro:bit default arrows only point in the eight directions, I'm going to define "as close as possible" to mean within plus/minus 22.5deg of North.

Since there's only eight arrows I could write out all eight equations, but that's not fun. 

<details><summary>So what's the appropriate mapping from orientation (takes values \[0, 360) ) and produces an arrow Image? </summary>

If we number the arrows 0 to 8 starting at North and moving clockwise (with N being both 0 and 8) we can write:

arrow_i = \ceil(\frac{orientation - 22.5}{45})
</details>

With that, it's a simple matter to convert the index into an Image object of the correct type. The only "gotcha" was the fact that the arrow direction had to be flipped E->W from what you'd normally expect.  For example, if the micro:bit is "facing" due East, then heading is 45, but we want to draw an arrow point to the "left" on the micro:bit. To draw an arrow pointing left we use an Arrow_W.

## Compass Code
```
from microbit import *
from math import ceil

arrow_map =['N', 'NW', 'W', 'SW', 'S', 'SE', 'E', 'NE', 'N']

def which_arrow(direction)->Image:
    if direction == 'N':
        return Image.ARROW_N
    elif direction == 'NE':
        return Image.ARROW_NE
    elif direction == 'E':
        return Image.ARROW_E
    elif direction == 'SE':
        return Image.ARROW_SE
    elif direction == 'S':
        return Image.ARROW_S
    elif direction == 'SW':
        return Image.ARROW_SW
    elif direction == 'W':
        return Image.ARROW_W
    elif direction == 'NW':
        return Image.ARROW_NW
    else:
        return Image.ANGRY

while True:
    if button_a.is_pressed():
        if not compass.is_calibrated():
            compass.calibrate()
        orient = compass.heading()
        i = int(ceil((orient - 22.5)/45))
        display.show(which_arrow(arrow_map[i]))
```