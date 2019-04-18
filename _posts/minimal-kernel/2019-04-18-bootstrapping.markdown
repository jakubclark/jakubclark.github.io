---
layout: post
title:  "Minimal Kernel: Bootstrapping"
date:   2019-04-18 22:27:13 +0200
categories: jekyll update
---

Bootstrapping essentially means writing the minimal amount of code, needed to actually enter the kernel program (the C program, not an assembly program).

This involves 3 parts:
- [`boot.S`][boot.S] - some assembly code to set up the CPU and enter the kernel program
- [`kernel.ld`][kernel.ld] - linker script, used to link all the compiled object files together
- [`kernel.c`][kernel.c] - the kernel program itself

Essentially, [`boot.S`][boot.S] and [`kernel.ld`][kernel.ld] are used to set up the CPU and memory and make the CPU enter the kernel program [`kernel.c`][kernel.c]


### Defining sections

The [linker script][kernel.ld] combines all the various output files, into a single file.

### Setting up the CPU

The CPU and the memory are set up using [`boot.S`][boot.S].

Since [`GRUB`][grub] is used as the boot-loader, the image that will be generated for the kernel must be [`multiboot2`](https://www.gnu.org/software/grub/manual/multiboot2/multiboot.html) compliant. The minimum requirements for this are to have specific 32-bit integers defined somewhere in the first 8KB of the image.

1. multiboot2 magic number: 0xe85250d6
2. multiboot2 header flags or tags
3. multiboot2 header length
4. multiboot2 checksum

The magic number essentially declares that the kernel is multiboot2 compliant, allowing [`GRUB`][grub] to act accordingly.

The header flags/tags can be used to provide extra information about the kernel image and make requests to the bootloader as well, such as requesting more bootloader information.

The header length is just how big the header section is.

The checksum is a way to check that everything has been compiled as expected. `-(MULTIBOOT2_HEADER_MAGIC + MULTIBOOT_ARCHITECTURE_I386 + HEADER_LENGTH)` must compute to an unsigned 0.

The relevant part of `boot.S` is [here](https://github.com/jakubclark/vu-kernel/blob/9f63702b26cf6374b4ba99448203fcf84a7b9d1f/vu-kernel/boot.S#L8-L22).

### The Kernel Itself

At the moment, the kernel program is very basic.

It simply clears the screen, and prints a number of colors characters. This is done using VGA mode. More details are in the next blog post.

Output:

![The printed output of the kernel](/assets/minimal-kernel-basic-output.png)

The text is kind of hard to see, since the background and foreground are so similar.

Printing formatted strings using [`printf()`](https://github.com/jakubclark/vu-kernel/blob/9f63702b26cf6374b4ba99448203fcf84a7b9d1f/vu-kernel/scrn.c#L120) is still broken though.

### Conclusion

In conclusion, the kernel at this point is very trivial. The GDT is set up, and some text is printed to the screen. In the next few blog posts, the GDT will be explained, as well as how text is actually printed to the screen, using VGA mode.

[boot.S]: https://github.com/jakubclark/vu-kernel/blob/9f63702b26cf6374b4ba99448203fcf84a7b9d1f/vu-kernel/boot.S
[kernel.ld]: https://github.com/jakubclark/vu-kernel/blob/9f63702b26cf6374b4ba99448203fcf84a7b9d1f/vu-kernel/kernel.ld
[kernel.c]: https://github.com/jakubclark/vu-kernel/blob/9f63702b26cf6374b4ba99448203fcf84a7b9d1f/vu-kernel/kernel.c

[grub]: https://www.gnu.org/software/grub/
