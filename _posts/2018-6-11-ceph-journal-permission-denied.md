---
layout: post
title: Ceph journal disk permission wrong after reboot
---

I use `ceph-deploy` install ceph Jewel, with journal disk on sdb and data on sdc to sdf. But the cluster fails after I reboot the node. The osd daemon fails to get up and the log shows the journal disk in a wrong permission. 

I change the permission manually with `chown ceph:ceph /dev/sdb?` whereas it doesn't sovle the matter and it'll still get wrong while rebooting. Google it and find the answer finally.

modify the udev rules making it correct the permission:
```
cat /etc/udev/rules.d/70-ceph-journal.rules
KERNEL=="sdb1", SUBSYSTEM=="block",OWNER:="ceph",GROUP:="ceph",MODE:="660"
KERNEL=="sdb2", SUBSYSTEM=="block",OWNER:="ceph",GROUP:="ceph",MODE:="660"
KERNEL=="sdb3", SUBSYSTEM=="block",OWNER:="ceph",GROUP:="ceph",MODE:="660"
KERNEL=="sdb4", SUBSYSTEM=="block",OWNER:="ceph",GROUP:="ceph",MODE:="660"
```

With this udev rules file, the cluster is healthy whether the node is reboot or not.