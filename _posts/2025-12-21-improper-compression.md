---
layout: post
title: you're compressing your files incorrectly
date: 2025-12-21 18:42:00 -0400
---

it doesn't matter what compression format you use; `.zip`, `.rar`, `.7z`, `.tar.gz` or whatever else it may be, you are probably doing it wrong.

> but how is there a "wrong way" to compress files? what does that even mean?

what i'm talking about is the way the root directory is stored in the compressed file. let me present you with a scenario:

![](https://i.postimg.cc/6pyKyYrN/image.png)

here, you have a folder (`MyFolder`) with some files that you want to compress. most people will instinctively right-click `MyFolder` and compress it. i'm here to tell you that this is **extremely inconvenient and problematic** due to the way your operating system handles this action.

when you do it this way, the resulting archive *does not* hold the file structure you would expect it to. instead, the resulting file structure will look something like this when you extract its contents:

![](https://i.postimg.cc/sfkpQnQc/image.png)

the resulting directory structure does not match the original one. do you see the problem? no, not yet? okay; here's a more critical example, and something that is one of the root causes of non-tech-savvy users complaining on forums:

suppose somebody wants to install mods for a video game they enjoy. the first thing they will do is approach a website that hosts these mods and follow the instructions for downloading whatever mods they want. these instructions are typically along the lines of "download this `.zip` file and extract it in your `Mods` folder". sounds simple enough, right?

so, that's exactly what the user does.

they download the mod, extract it into whatever folder they were told to, and refresh their in-game mod list. however, they don't see their mod listed. why is that?

well, here's what's going on from the perspective of the mod distributor:

![](https://i.postimg.cc/SNctJYs6/image.png)

they right-click their mod,

![](https://i.postimg.cc/wB4wShfq/image.png)

they compress it,

![](https://i.postimg.cc/8k6Z0fhN/image.png)

and finally, they upload that shiny `.zip` file to the modding website for others to enjoy.

now, let's understand what happens when the end user tries to extract that zip. their mod directory structure will look something like this after extraction:

![](https://i.postimg.cc/mDxHzkhr/image.png)

this is **horrible**.

when the video game's mod loader is scanning for mods, it briefly looks inside of each directory to check if it has modifications. in this case, it would check if the folder has, say, a sub-directory named `Assets`, and if there are any files in that sub-directory that could be applied to the game. but instead, this is what goes down:

1. the scanner sees `MyMod` in the mods folder
2. the scanner sees `MyMod` in the `MyMod` folder
3. the scanner stops looking because there is immediately nothing else in that root directory
4. it concludes that there is nothing to be done

if the user is not tech-savvy, they won't have any clue what went wrong. they followed all of the instructions just as they were told to, but they are unable to use the mod because of one simple misstep in the archival process.

> okay, but how does somebody properly compress their files?

this is super easy. instead of right-clicking the root folder and compressing that, you should instead do this:

![](https://i.postimg.cc/fbghBSY3/image.png)

first, select everything directly inside of the directory you are trying to compress

![](https://i.postimg.cc/g2g92LS4/image.png)

then, right-click on any of the selected files/directories (it does not matter as long as all of them are selected) and compress it as you normally would.

![](https://i.postimg.cc/3wkqrSD4/image.png)

you will get a singular archive. now, all that's left is to rename it however you want the resulting root folder to be named. in this case, we want it to be `MyMod`, so we will rename the archive to `MyMod.zip`.

![](https://i.postimg.cc/3N8CNNbC/image.png)

and that's it. this archive is the one you should send to others. when extracting it, you will realize that all of its contents are placed exactly where they should be.

before i go, there are a couple of other things i should briefly touch on:

- on Linux, some file managers (e.g. [Dolphin](https://apps.kde.org/dolphin/)) have a dedicated option to automatically de-nest these problematic sub-directories for you, consequently preventing the odd extraction results detailed above. way to go, Tux!
	- on the other hand, Windows does not have any quality-of-life features similar to this currently, which is why this issue is so widespread and problematic
- as you may know, there is more than one way to extract an archive. for example, "Extract Here" versus "Extract To". their behaviors differ depending on operating system and file manager. but, generally speaking, unless you know what you're doing and it is absolutely applicable to your use case, you should try to avoid "Extract Here" whenever possible as it could have [unintended side-effects](https://www.reddit.com/r/ProgrammerHumor/comments/v4g1xo/i_hate_it_when_i_press_extract_here/).