---
layout: post
title: Attach usb device to virtual machine
---

1. find the usb device on the host machine

`lsusb`

`Bus 002 Device 003: ID 1a56:dd01`

2. edit a xml file for attaching

`cat attatch-usb.xml`

    <hostdev mode='subsystem' type='usb' managed='yes'>
      <source>
          <vendor id='0x1a56'/>
          <product id='0xdd01'/>
          <address bus='2' device='3'/>
      </source>
    </hostdev>

3. attach the usb into the virtual machine

`virsh attach-device instance-00000460 /path/to/attach-usb.xml --persistent`

4. now you can find the usb device in the virtual machine named `instance-00000460`

