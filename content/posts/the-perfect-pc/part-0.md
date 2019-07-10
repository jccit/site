---
title: The Perfect PC - Part 0
date: "2019-07-10"
---

For the last few years, I've been switching between Linux and Windows on my home PC. My reasoning is pretty simple, Linux is great to develop on. However I like playing games on my PC and not all games support Linux. For these games that don't support Windows, I end up having to switch back to my Windows partition.

This situation has improved since I started dual-booting years ago. Valve's Proton has allowed me to play more (but not all) of my games on Linux. WSL has improved the development experience on Windows by allowing me to use a Linux user-space. But these compatibility tools aren't perfect.

Proton can't play games with invasive anti-cheat or DRM, so that rules out some more popular multiplayer games. It also isn't perfectly compatabile with ever game on Steam yet. According to [ProtonDB](https://protondb.com), out of the top 1000 games on Steam, 57% have at least a gold rating (gold meaning "runs perfectly after tweaks"). It's certainly impressive, but it's not perfect.

WSL is a great solution for people like me who grew up with Windows and are familiar with it, but have switched to Linux recently. You get a full Linux terminal running in the Windows kernel. Microsoft has done a great job with WSL and from what I've seen, WSL2 looks pretty impressive. However I want to reduce my reliance on Windows as much as I can. Switching to Windows with a Linux userspace feels like a backwards step.

## UnRAID to the rescue(?)

My first attempt at solving this problem was [UnRAID](https://unraid.net/). I had heard about UnRAID in a Linus Tech Tips video and it seemed like the perfect solution to my problem.

If you are unfamiliar with what UnRAID is, here's the basics. It is a proprietary Linux distro designed for NAS systems. You run it off of a USB stick and control it via a web interface from another machine. It allows you to pool your drives together into a ZFS pool, run Docker containers and run QEMU based VMs.

The VM part of UnRAID interested me, I was not very familiar with how QEMU and libvirt worked so having a nice UI to control it all was pretty handy.

Using this system I could pool all of my hard drives together into a bunch of network shares and I'd get redundancy too, for the cost of 1 drive. The VM system allowed me to pass through PCI devices such as my GPU and a USB controller to the VM. This would allow me to run a full Windows VM inside UnRAID without any GPU performance loss. I had a Linux and Windows VM that I could switch between and I ran my Plex server using the built in Docker support. That meant I didn't have to shut down my server when I wanted to switch OS.

Performance was pretty great, it even worked with my Vive once I passed through a PCIe USB card.

However I encountered a bunch of issues with the network share system. In order to access any of my game library on the array, I had to mount the array as a network share within Windows. This caused issues with some game launchers (Origin and Battle.net) which refused to install to a network share, even if I symlinked them to the C drive.

There are ways to fix this. It is possible to create a virtual hard disk on the array and mount that to the VM at boot. In the end I decided against this approach and switched back to running 2 operating systems like I used to.

However I missed how easy it was to switch between operating systems in UnRAID and have my Docker containers running in the background. So recently, I've had an idea.

## What If I Build My Own UnRAID 🤔

Since UnRAID uses QEMU, libvirt and OMVF under the hood for their VM system, why can't I just build my own version of UnRAID on top of an existing Linux distro?

This would probably be a much better idea, since I could have Linux installed on my SSD and use that for my main OS. But if I wanted to play games, I could just open up the Windows VM from within Linux and start playing.

My plan is to get an Arch install up and running like I normally do for a Linux install, but run that through the Intel GPU built in to my CPU which is normally disabled. I can then pass my Nvidia GPU to Windows when I want to use Windows. Hopefully I can even get the Nvidia GPU working under Linux when I need it so I can use Steam in Linux.

That way I can spend most of my time using Linux and only ever have to turn on my Windows VM when I can't get a game working in Linux.

I'm a complete noob when it comes to this type of thing. I know I'm going to get stuff wrong. But knowing the internet, someone's going to point out everything I'm doing wrong. I'll be posting the first real part to this series soon where I start getting my Linux install up and running with virtio installed.
