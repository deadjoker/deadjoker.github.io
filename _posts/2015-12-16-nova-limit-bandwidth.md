---
layout: post
title: How to limit bandwidth in nova by editing flavor
---

From early release, nova can limit bandwidth by using kvm xml files. Just add extra specs within flavors in order to limit vm quota.

`nova flavor-key nlimit set quota:vif_outbound_average=20`

This makes vm using flavor *nlimit* have only 20KB/s incoming rate.

Besides, using flavor extra specs, we can have more quota limitation except for incoming bandwidth.

**For Bandwidth I/O, the vif I/O options are:**


* vif_inbound_ average
* vif_inbound_burst
* vif_inbound_peak
* vif_outbound_ average
* vif_outbound_burst
* vif_outbound_peak


**CPU limits:**

* cpu_shares. Specifies the proportional weighted share for the domain. If this element is omitted, the service defaults to the OS provided defaults. There is no unit for the value; it is a relative measure based on the setting of other VMs. For example, a VM configured with value 2048 gets twice as much CPU time as a VM configured with value 1024.
* cpu_shares_level. On VMWare, specifies the allocation level. Can be custom, high, normal, or low. If you choose custom, set the number of shares using cpu_shares_share.
* cpu_period. Specifies the enforcement interval (unit: microseconds) for QEMU and LXC hypervisors. Within a period, each VCPU of the domain is not allowed to consume more than the quota worth of runtime. The value should be in range (1000, 1000000). A period with value 0 means no value.
* cpu_limit. Specifies the upper limit for VMware machine CPU allocation in MHz. This parameter ensures that a machine never uses more than the defined amount of CPU time. It can be used to enforce a limit on the machine’s CPU performance.
* cpu_reservation. Specifies the guaranteed minimum CPU reservation in MHz for VMware. This means that if needed, the machine will definitely get allocated the reserved amount of CPU cycles.
* cpu_quota. Specifies the maximum allowed bandwidth (unit: microseconds). A domain with a negative-value quota indicates that the domain has infinite bandwidth, which means that it is not bandwidth controlled. The value should be in range (1000, 18446744073709551) or less than 0. A quota with value 0 means no value. You can use this feature to ensure that all vCPUs run at the same speed.

**Memory limits:**

* memory_limit: Specifies the upper limit for VMware machine memory allocation in MB. The utilization of a virtual machine will not exceed this limit, even if there are available resources. This is typically used to ensure a consistent performance of virtual machines independent of available resources.
* memory_reservation: Specifies the guaranteed minimum memory reservation in MB for VMware. This means the specified amount of memory will definitely be allocated to the machine.
* memory_shares_level: On VMware, specifies the allocation level. This can be custom, high, normal or low. If you choose custom, set the number of shares using memory_shares_share.
* memory_shares_share: Specifies the number of shares allocated in the event that custom is used. There is no unit for this value. It is a relative measure based on the settings for other VMs.

**Disk I/O limits:**

* disk_io_limit: Specifies the upper limit for disk utilization in I/O per second. The utilization of a virtual machine will not exceed this limit, even if there are available resources. The default value is -1 which indicates unlimited usage.
* disk_io_reservation: Specifies the guaranteed minimum disk allocation in terms of IOPS.
* disk_io_shares_level: Specifies the allocation level. This can be custom, high, normal or low. If you choose custom, set the number of shares using disk_io_shares_share.
* disk_io_shares_share: Specifies the number of shares allocated in the event that custom is used. When there is resource contention, this value is used to determine the resource allocation.

**Disk tuning:**

* disk_read_bytes_sec
* disk_read_iops_sec
* disk_write_bytes_sec
* disk_write_iops_sec
* disk_total_bytes_sec
* disk_total_iops_sec

But for disk quota, it's no use if you use storage on network (e.g. ceph).

[openstack docs reference](http://docs.openstack.org/admin-guide-cloud/compute-flavors.html)
