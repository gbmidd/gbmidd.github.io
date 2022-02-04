---
layout: single
title:  "wake-on-LAN: the Magic Packet"
date:   2021-12-28 10:43:15 -0700
categories: tips
read_time: true
---

We have the good fortune to live in a house with Ethernet wired in -- there's a drop in every room, which I've connected to a switch in the data panel, allowing us to get gigabit internet on all of our workstations.  With this, I'm able to move the beefier (and louder, and warmer) machine to a remote corner of the house, and use only a thin client to interact with it from the home office.

***Problem:*** I don't want the heavy machine powered on all the time, but I also don't want to have to trek downstairs into the basement to wake it from sleep when I need to use it.
{: .notice--danger}

***Solution:*** the Magic Packet.
{: .notice--info}

Once the heavy machine is properly configured (usually, a setting in the BIOS for "wake on LAN" must be enabled), the following command will wake the machine from an S3 sleep state.

```
sudo etherwake -D -i enxa0cec8188a36 18:C4:10:AB:35:F0
```

The `enxa...` is the name of my ethernet adaptor, and the mac address is for the server I want to wake up.  This uses `etherwake` (available in Ubuntu repositories) to send a Magic Packet over the enxa interface to the machine with specified MAC address, waking up my basement workstation.  When I'm finished using it, from an SSH connection, I can put it right back to sleep:

```
sudo systemctl suspend
```