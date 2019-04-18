---
layout: post
title:  "Minimal Kernel: Printing to the Screen"
date:   2019-04-18 23:27:13 +0200
categories: jekyll update
---

The easiest way to print to the screen, is using what is known as [VGA-compatible text mode](https://en.wikipedia.org/wiki/VGA-compatible_text_mode) or VGA-mode for short.

In this mode, it is possible to represent (colored) characters and a background for each character.

The screen can support 80x25 characters at one time.


## Colored Characters
Each character is really formed by two bytes: the character byte and the attribute byte. The character byte is the actual character we want to display, such as 'a'or 'O' or '+' etc. The attribute byte is a bit field, that can be used to change the background and foreground colors.

Setting the attribute byte, happens on a per character basis, so each character can have a different foreground-background combination.

On startup, the screen is initially cleared. This involves setting each character to the " " (space) character, with a black background. The black background is achieved with '0x0F' (really, this has a white foreground, but since " " is empty, it is not shown).

In total, there are 16 colors to chose from. The definitions can be found [here](https://github.com/jakubclark/vu-kernel/blob/9f63702b26cf6374b4ba99448203fcf84a7b9d1f/vu-kernel/colors.h).

These values will need to be shifted appropriately, to set the appropriate background/foreground bits in the attribute byte.

## VGA Address

The way that printing to the screen is done, is by modifying the VGA text buffer. The VGA text buffer is a specific place in memory, that contains the raw bytes that are being used when printing to the screen.

The VGA text buffer begins at address '0xB8000'. The byte at this location, is the attribute byte of the first character (at the top-left of the screen). The byte at 0xB8001 is the character byte of the first character (at the top-left of the screen).

If '0xB8000' is set to '0x0F' and '0xB8001' is set to 0x61, then the "a" character will be printed with a white foreground and a black background.

This is what that looks like:

![Example-VGA-Mode-output](/assets/minimal-kernel-vga-mode.png)


As was shown in the previous post, printing a variety of colored text looks like this:

![Example colored output](/assets/minimal-kernel-basic-output.png)


## Conclusion

VGA-Mode is a simple to use way to write to the screen. Although quite limited (only 16 colors), it is still a great way to communicate with the user of the kernel. No longer does the user see just a black screen. This is a big step towards an interactive system.

In the next post, the Global Descriptor Table will be explained, which is needed to support this VGA-Mode, as well as adding functionality to (de)allocate memory.
