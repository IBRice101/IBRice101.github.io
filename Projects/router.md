# Router Firmware Reversing

Over the years I've taken a real shining to IoT security, Recently though I was talking to my friend John (their [LinkedIn](https://www.linkedin.com/in/john-forbes-931682194/), and [GitHub](https://github.com/ThatGuyJohnForbes)) about some ideas I had around my dissertation, and they gave me the idea of dumping the firmware from a router and reversing that. I didn't think it was really dissertation material, at least not for me, but I thought that it's an excellent idea for a little project!

In this article I'll be going over the process of getting onto the board of the router, grabbing the firmware, dumping it, and then reversing it to see if there's anything interesting going on.

## Requirements

- Router (I have a Plusnet Hub One that John very kindly let me borrow)
- Soldering Iron + Solder (and everything that comes with that)
- Wires
- Breadboard
- USB to Serial TTL Dongle OR Arduino sufficient to handle that
- Laptop (I'm using a Manjaro Linux laptop so everything in here will be tailored to that)

## Preparation

I'm following [this walkthrough](https://graspingtech.com/install-openwrt-plusnet-router/) until I get a terminal, installing OpenWRT isn't really my goal here as I already have a router but if you wanna do that then this is an excellent guide, too