---
layout: post
title: Ceph recovery issue
---

Our ceph environment only has one network, so that internal and public uses the same network. 
When the number of osd changes, huge data transferring will cause client has no access to ceph. 
In this case, limit internal net flow would be a good choice.

| arg | discription | default |
| ------ | ------ | ------ |
| osd max backfills | The maximum number of backfills allowed to or from a single OSD | 1 |
| osd recovery max active|The number of active recovery requests per OSD at one time.More requests will accelerate recovery, but the requests places an increased load on the cluster. | 3 |
| osd recovery sleep | Time in seconds to sleep before next recovery or backfill op.Increasing this value will slow down recovery operation while client operations will be less impacted | 0 |
| osd recovery sleep hdd | Time in seconds to sleep before next recovery or backfill op for HDDs. | 0.1 |
| osd recovery sleep ssd | Time in seconds to sleep before next recovery or backfill op for SSDs. | 0 |
| osd recovery sleep hybrid | Time in seconds to sleep before next recovery or backfill op when osd data is on HDD and osd journal is on SSD. | 0.025 |

modify options on the fly

`ceph tell osd.* injectargs osd_recovery_max_active 1`

or

`ceph config set osd_recovery_max_active 1`