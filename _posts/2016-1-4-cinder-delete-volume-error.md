---
layout: post
title: Cinder delete volume with error state
---

While I'm using tgt+lvm as cinder backend, it's possible that the volume goes into ``error-deleting`` after delete the volume. 

Log shows

`Stderr: 'device-mapper: remove ioctl failed: Device or resource busy\nCommand failed\n'`

But how does it stay in 'busy' whereas I've already deasscoiated the volume from the instance?

`tgtadm --lld iscsi --mode target --op show`

Tells that it remains some connections on the lun.

Remove the target with

`tgtadm --lld iscsi --mode target --op delete --force --tid x`

Remove the lvm with

`lvremove /dev/mapper/volumes-xxxxxx`

Delete database record with

`mysql -h controller -u cinder -p cinder_pass -e "update volumes set deleted = 1, deleted_at = now() where id = 'xxxxxxx'"`

Finally, `cinder list --all-tenant` will show clean list :).
