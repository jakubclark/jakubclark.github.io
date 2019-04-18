---
layout: post
title:  "Building a Minimal Kernel"
date:   2019-04-18 22:27:13 +0200
categories: jekyll update
---
As part of my Computer Science Bachelor at VU University Amsterdam, I have to do what is called a 'Bachelor Project'. Essentially, I just had to pick a computer science related topic and do a project on it, along with a written report and presentation.

The topic I chose, was writing a small kernel. I decided to write the kernel in C, but was tempted to try and write it in Go or Rust.

The requirments were not very heavy:
- Enter Kernel Program from assembly
- Set up the CPU (GDT, MMU, Long Mode, etc.)
- Add some useful functionality, such as a network driver, filesystem etc.

The language the kernel is written in, is C.

## Table of Contents:
1. [Bootstrapping](./bootstrapping.html)
2. Printing to the Screen
3. Setting up the GDTs
