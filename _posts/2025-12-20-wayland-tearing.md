---
layout: post
title: we need to talk about Wayland's tearing protocol
date: 2025-12-20 13:00:00 -0400
---

Wayland is everyone's favorite modern display protocol for Linux-based operating systems. a couple of years ago, it was in a very immature state and people who were experimenting with the protocol often had to switch back to an Xorg session to do their usual things.

however, even today, i am having to switch back and forth between two window managers—Niri and i3wm—for one simple reason:

**screen tearing on Wayland absolutely sucks.**

> but Laith, why would you ever want screen tearing? isn't it good that this behavior has poor support on Wayland?

in theory, yes, preventing screen tearing is a good thing, but it has its downsides. for the gamers reading this, the term "V-sync" may sound familiar to you. screen tearing is what happens when you do not have V-sync enabled. V-sync is short for "vertical sync", and when it is enabled, it does exactly what it sounds like it does; it ensures that when pixels are being rendered to your screen, they all render at the exact same time, and the drawing process is synced vertically with the rest of the screen in correspondence to your monitor's refresh rate. when V-sync is disabled, you experience what is called "screen tearing" or "immediate presentation" which draws pixels to the screen as soon as they are ready instead of waiting for them to sync with each other in any way.

do you see where this is going?

there is a very clear cost to using V-sync technology; **it takes time to complete the syncing process**.  many people (myself included) who play difficult video games that require split-second accuracy (e.g. precision platformers, rhythm games, etc.) heavily rely on screen tearing because it reduces their input latency by up to an entire tenth of a second.

one of Wayland's main goals is to eliminate screen tearing, but as time has progressed, people have realized that simply disabling "V-sync" in their target video game does not actually do anything, and that Wayland keeps it enabled compositor-wide, behind the scenes. as a result, these individuals have expressed distaste with not even being given the option to disable V-sync. since then, Wayland has exposed a "tearing" protocol that allows the screen to tear. problem solved, right?

well, not exactly. far from it, actually.

99% of the time, when enabled, the tearing protocol either doesn't work (it has no effect on the screen's output and V-sync remains effectively enabled) or it half-ass works and the screen barely tears (consequently barely eliminating any input latency in the process).

Wayland compositors that support this protocol are aware of its lack of compatibility. for example, [Hyprland's documentation](https://wiki.hypr.land/Configuring/Tearing/) states the following:

![](https://i.postimg.cc/k5BQHf1c/image.png)

*yikes*.

not to mention, the only compositors that support this protocol are only able to apply it in full-screen programs/games where *no other graphical elements are visible on the screen*; and we're still talking about compositors that *support* this tearing protocol. there are plenty that don't, including my favorite window manager, [Niri](https://github.com/YaLTeR/niri).

because of this, whenever i want to play a game, i find myself logging out of my Niri session and logging into my i3wm session, because it uses Xorg. this is very frustrating, and i fault the Wayland maintainers for being so rigid on not supporting tearing from the beginning. i hope Wayland gets stable tearing support in the future. if it does, i will give it a spin and create another blogpost with my experiences.