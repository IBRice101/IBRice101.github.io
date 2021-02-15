# Linux From Scratch: Raspberry Pi style - Part 0: Prerequisites

## Intro

Hi there! I'm Isaac and in this file I'm going to take you through the absolute ordeal I had to go through in order to go from literally nothing at all to a working Linux-based OS running an Apache web server. If you're familiar with LFS (Linux From Scratch) you may be wondering why this is such a big deal... Well, I bloody hope that's not what you're thinking actually, because it's hard enough to do already without you breathing down my neck. Where this differs from what I might have done otherwise though is that I decided, like an idiot, to develop this LFS system for Raspberry Pi, which as you may know doesn't use the standard x86 CPU architecture, but instead uses an ARM chip, similar to the architecture of most mobile phones. This made things... Interesting, to say the least, and I hope you'll stick around to figure out exactly how and why this was the case :).

Before we start, my specs, as they were when I first began this journey:

* OS: Manjaro Linux x86 64 bit
* Host: Inspiron 5570
* Kernel: 5.4.95-1-MANJARO
* Shell: zsh 5.8
* DE: KDE Plasma 5.20.5
* WM: KWin
* Theme: Breeze Dark [Plasma], Breath [GTK2/3] 
* Icons: breeze-dark [Plasma], breeze-dark [GTK2/3] 
* CPU: Intel i3-6006U (4) @ 2.000GHz 
* GPU: Intel Skylake GT2 [HD Graphics 520]
* Memory: 8GB

## What do you need?

In order to create this system I had to abandon my much more powerful Windows machine in favour of my smaller Linux based laptop, this is for a whole load of reasons, primarily it's because the LFS Book recommends it anyway. However the other primary reason for me wanting to develop on Linux is, due to the fact that the RPi has an ARM based CPU architecture, I have to develop in a similarly ARM oriented OS, (Raspberry Pi OS in this case)... I cannot do this using standard windows-based software hypervisors such as VMWare Workstation because they only offer virtualisation, not emulation which is what is required to emulate differing architectures.

"So Isaac" you may say with unwavering curiosity, "How the hell are you gonna get out of this one???", and to that I have one answer: QEMU.

## QEMU

[QEMU](https://www.qemu.org/) is a "generic and open source machine emulator and virtualizer." Before this project started I thought, naiively, that I could just download a raspberry pi iso file and pop it in VMware Workstation without a care in the world. As mentioned before, this obviously isn't the case. However, this (sort of) is the case for QEMU, thankfully! Its role as an emulator means that the hardware architecture required to run it can be... well... emulated with relative ease. I didn't actually have qemu installed at this point though so I thought I probably should get on that.

The other packages I istalled alongside QEMU were:

* `virt-manager`, a GUI interface for managing Virtual Machines
* `virt-viewer`, graphical viewer for the guest OS display
* `dnsmasq`, a lightweight package for network infrastructure over small networks (i.e. the connection between guest and host/outside world)
* `vde2`, Virtual Distributed Ethernet, a toolbox to manage virtual networks.
* `bridge-utils`, Utilities for configuring the Linux ethernet bridge (a way to connect two ethernet segments together via ethernet address as opposed to IP address)
* `openbsd-netcat`, jack-of-all-trades networking utility

Because I'm a total noob I had to use [this](https://computingforgeeks.com/complete-installation-of-kvmqemu-and-virt-manager-on-arch-linux-and-manjaro/) extremely handy walkthrough. It should be noted though that the only issue I experienced here was in step 2, the installation of `libguestfs`. These issues being that the walkthrough encourages you to install through yaourt, which is a deprecated and insecure package manager for the Arch User Repository (AUR), after trying to be fancy and install through the standard `git clone [url.git]; cd [repo]; makepkg -si`, I encountered some issues to do with Java that to be quite honest I couldn't get around, so I instead opted for the Manjaro `Add/Remove packages` program, which functioned absolutely lovely.

## Actually getting Raspberry Pi OS

With QEMU/KVM and virt-manager set up, it was now time to set up the host system, the ARM-based Raspberry Pi OS, a fork of the standard Debian Linux OS customised for use on Raspberry Pi microcomputers, the Download can be found [here](https://www.raspberrypi.org/downloads/raspberry-pi-os/). I used [this](https://epicchasgamer.com/2020/10/18/how-to-emulate-raspberry-pi-in-qemu/) tutorial to get through the ordeal of running an arm-based OS through QEMU, thanks, Epic Chas Gamer!

Note: due to the size of the launch command I had to keep it in a separate shell script file lol, it looks like this:

```sh
qemu-system-arm -M versatilepb -cpu arm1176 -m 256 -kernel qemu-rpi-kernel/kernel-qemu-4.19.50-buster -hda 2021-01-11-raspios-buster-armhf-full.img -append "dwc_otg.lpm_enable=0 root=/dev/sda2 console=tty1 rootfstype=ext4 elevator=deadline rootwait" -dtb qemu-rpi-kernel/versatile-pb-buster.dtb -no-reboot -serial stdio
```

## Recommended Reading

Finally for this project there is some recommended reading, LFS is the book that I will be using to construct the operating system, but there is some prerequisite knowledge I needed to brush up on that was recommended by the book itself, namely how to build software and a beginner's guide to installing from source. All three guides can be found below. 

* reading:
	* [LFS v10.0](http://linuxfromscratch.org/lfs/view/stable/)
	* [Software-Building-HOWTO](http://www.tldp.org/HOWTO/Software-Building-HOWTO.html)
	* [Beginner's Guide to Installing from Source](http://moi.vonos.net/linux/beginners-installing-from-source/)

If all of the steps in this guide are followed correctly (and I've followed them correctly because I wrote the bloody thing) then you should be in a pretty good position to be able to begin creating a raspberry pi based linux from scratch distribution! THis is a pretty daunting task and one that I don't take lightly so I may take some extra time to prepare this, I also have uni work to do so this is very much either going to be a "leave it until summmer" project or a "time to procrastinate harder than I've ever procrastinated before" project... I guess only time will tell.
