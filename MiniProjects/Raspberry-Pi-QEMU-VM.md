# Linux From Scratch: Raspberry Pi style - Part 0: Prerequisites

## Intro

Hi there! I'm IBR and in this file I'm going to take you through the absolute ordeal I had to go through in order to go from literally nothing at all to a working Raspberry Pi OS image running under QEMU on my laptop!

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

In order to create this system I had to abandon my much more powerful Windows machine in favour of my smaller Linux based laptop, this is for one main reason, and that is that, due to the fact that the RPi has an ARM based CPU architecture, I have to mount the OS in a similarly ARM oriented container... I cannot do this using standard windows-based software hypervisors such as VMWare Workstation because they only offer virtualisation, not emulation which is what is required to emulate differing architectures.

"So IBR" you may say with unwavering curiosity, "How the hell are you gonna get out of this one???", and to that I have one answer: QEMU.

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

If all of the steps in this guide are followed correctly (and I've followed them correctly because I wrote the bloody thing) then you should be in Raspberry Pi OS! Have a nice play around in there and have fun! 
