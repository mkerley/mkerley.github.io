---
layout: post
title: Pipes 1.4 released
date: "2009-02-08 23:44:01 -0800"
categories:
  - Games
  - Pipes
  - Pipes iPhone
tags:
  - "1.4"
excerpt_separator: <!--more-->
---

<aside>
  <figure>
     <img src="/assets/images/2009/02/screenshot-20090208-234124.png" alt="Tutorial Screen Shot" />

     <figcaption>Tutorial Screen Shot</figcaption>

  </figure>
</aside>

### What's New in Pipes 1.4

- Added two new board sizes: Super Insane (48x60) and Mega Insane (56x70)
- Added piece locking feature
- Added tutorial on how to play and basic strategies
- Rewrote graphics engine for much smoother zooming/scrolling on larger board sizes

<!--more-->

[![Download on the App Store](/assets/images/3rdparty/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/us/app/pipes/id296105712)

### Technical junk for my fellow iPhone nerds

The most interesting and difficult part of this update for me was the graphics engine rewrite. Basically I switched from using standard 2-dimensional rendering (based on CoreGraphics) to OpenGL-based rendering. I suspect that CoreGraphics doesn't provide much hardware acceleration, while OpenGL obviously does.

I don't have a benchmark of the old rendering engine handy, but it was somewhere in the neighborhood of 10-15 FPS during a zoom operation on the _Insane_ board size. Using OpenGL, I'm able to get 50-60 FPS on _Insane_ (according to the OpenGL ES tool in Instruments). That's why I was able to add 2 larger board sizes in this release; my old rendering engine simply couldn't have handled them adequately.

I'm using a sprite sheet to render all the pipe shapes using a single OpenGL texture. This optimization completely eliminates texture state switching, which helps to keep things running nice and fast. Also, I'm building up all the vertex/texture data and then rendering each frame with a single call to `glDrawArrays`. I'm sure there are other things that could be done to optimize things even further (probably switching to `glDrawElements` for starters), but I'm still fairly new to OpenGL, and still learning. If I find a way to speed things up more, I'll add at least one even more devious and demented board size to the game.
