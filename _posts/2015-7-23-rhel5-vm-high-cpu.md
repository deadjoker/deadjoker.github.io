---
layout: post
title: Issue with time drift and usb-tablet
---

When RHEL5 run on vm, it makes the physical machine high cpu load while the vm is still in idle state. 

Three ways to solve this problem:

1. upgrade kernel to 3.2

2. add divider=10 to grub kernal option

3. remove usb-tablet from qemu with 'virsh qemu-monitor-command <domain> --hmp del_device input0'
