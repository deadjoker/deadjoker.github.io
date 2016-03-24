---
layout: post
title: Ceph resize journal disk
---

* set osd with 'noout' so that the data will not migrate

`ceph osd set noout`

* stop osd which need resize journal

`service ceph stop osd.N`

* flush his journal to the filestore

`ceph-osd -i N --flush-journal`

* reconfigure the journal config and resize

`journal_uuid=$(sudo cat /var/lib/ceph/osd/ceph-N/journal_uuid)`

`sgdisk --new=0:0:+20480M --change-name=0:'ceph journal' --partition-guid=0:$journal_uuid --typecode=0:$journal_uuid --mbrtogpt -- /dev/sdx`

* initialize the new journal

`ceph-osd -i N --mkjournal`

* restart the osd

`service ceph start osd.N`

* reset 'noout' flag

`ceph osd unset noout`
