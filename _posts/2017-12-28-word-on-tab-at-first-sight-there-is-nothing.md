---
layout: post
title: Word on ARM tablet
date: '2017-12-28T20:13:39+01:00'
tags:
- ChromeOs
- crouton
- remmina
- debian
- jessie
- vncviewer
- ssh
- rpd
- virtualbox
- lxde
- asus
---


### Word on ARM tablet 

At first sight there is nothing special in the picture, a netbook running MS Word. A more close view and one can observe that the word processor is really appearing inside one Chrome tab. In fact, it is really a tab in Chrome OS running on one Asus Chromebook C100 Flip. It is an inexpensive chromebook mounting an ARM type processor including ~9 hours of battery life. How is it done if at the moment neither wine or virtualbox programs have a stable ARM version? 

|   ![](/imgs/p1oqqrFbOZ1rsb0g7o1_640.png)   |
|:--:|
|*A client-server scheme showing the software layers needed to run MS Word in this C100 chromebook*|

The trick is that the chromebook is running [crouton](https://github.com/dnschneid/crouton) behind the scene. This crouton (a Debian chroot in my case) is running a rdp "ssheathed" connection managed by [Remmina](https://remmina.org/) to a headless virtualbox machine server running the Word of a W7. LAN connections are ok and fluid but WAN connections could be tricky. In this case of slow connections, I prefer running directly x11vnc (also managed by Remmina). There are few program utilities not runnning in ARM processors still there and needing this kind of nested/layered connections. 
