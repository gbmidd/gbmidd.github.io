---
layout: single
title:  "Partitioning a Drive for Linux"
date:   2021-12-15 11:00:15 -0700
categories: tips
read_time: true
---

When setting up a new Linux installing on a clean machine for personal use, partitioning the drive can be a very useful way of future-proofing yourself against updates to the operating system.  In particular, it is wise to separate the operating system from your home folder--in this way, you can reinstall the OS **without** losing user files.

Suppose `sda` is a 256 GB disk.  I would recommend partitioning it as follows:


| partition | mountpoint | size |
|-----------|------------|------|
|sda1       |/           |40 GB |
|sda2       |/home		 |200 GB|
|sda3       |swap        |16 GB |

The sizes are approximate and depend on what you expect your needs to be, particularly with respect to swap.

With an installation like this, one could format `sda1` and install a new operating system, keeping the mount point `/home` on `sda2`.  I've done this many times, never worried about losing my files when upgrading or wiping an OS.
