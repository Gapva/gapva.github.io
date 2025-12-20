---
layout: post
title:  "we need to talk about wayland's tearing protocol"
date: 2025-12-20 13:00:00 -0400
---

wayland is everyone's favorite modern display protocol for linux-based operating systems. a couple of years ago, it was in a very immature state and people who were experimenting with the protocol often had to switch back to an Xorg session to do their usual things.

however, even today, i am having to switch back and forth between two window managers—niri and i3wm—for one simple reason:

**screen tearing on wayland absolutely sucks.**

> but laith, why would you ever want screen tearing? isn't it good that this behavior has poor support on wayland?

in theory, yes, preventing screen tearing is a good thing, but it has its downsides. for the gamers reading this, the term "v-sync" may sound familiar to you. screen tearing is what happens when you do not have v-sync enabled. v-sync is short for "vertical sync", and when it is enabled, it does exactly what it sounds like it does; it ensures that when pixels are being rendered to your screen, they all render at the exact same time, and the drawing process is synced vertically with the rest of the screen in correspondence to your monitor's refresh rate. when v-sync is disabled, you experience what is called "screen tearing" or "immediate presentation" which draws pixels to the screen as soon as they are ready instead of waiting for them to sync with each other in any way.

do you see where this is going?

there is a very clear cost to using v-sync technology; **it takes time to complete the syncing process**.  many people (myself included) who play difficult video games that require split-second accuracy (e.g. precision platformers, rhythm games, etc.) heavily rely on screen tearing because it reduces their input latency by up to an entire tenth of a second.

one of wayland's main goals is to eliminate screen tearing, but as time has progressed, people have realized that simply disabling "v-sync" in their target video game does not actually do anything, and that wayland keeps it enabled compositor-wide, behind the scenes. as a result, these individuals have expressed distaste with not even being given the option to disable v-sync. since then, wayland has exposed a "tearing" protocol that allows the screen to tear. problem solved, right?

well, not exactly. far from it, actually.

99% of the time, when enabled, the tearing protocol either doesn't work (it has no effect on the screen's output and v-sync remains effectively enabled) or it half-ass works and the screen barely tears (consequently barely eliminating any input latency in the process).

wayland compositors that support this protocol are aware of its lack of compatibility. for example, [hyprland's documentation](https://wiki.hypr.land/Configuring/Tearing/) states the following:

![[attachments/Pasted image 20251220143138.png]]

*yikes*.

not to mention, the only compositors that support this protocol are only able to apply it in full-screen programs/games where *no other graphical elements are visible on the screen*; and we're still talking about compositors that *support* this tearing protocol. there are plenty that don't, including my favorite window manager, [niri](https://github.com/YaLTeR/niri).

because of this, whenever i want to play a game, i find myself logging out of my niri session and logging into my i3wm session, because it uses Xorg. this is very frustrating, and i fault the wayland maintainers for being so rigid on not supporting tearing from the beginning. i hope wayland gets stable tearing support in the future. if it does, i will give it a spin and create another blogpost with my experiences.