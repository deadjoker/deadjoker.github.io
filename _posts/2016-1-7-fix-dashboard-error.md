---
layout: post
title: Fix "unable to retrieve details for instance" error on dashboard
---

After I remove volumes as previous post does, when I click the instance name my dashboard claims 

`Error: Unable to retrive details for instance xxxxx`

In `nova show` output, I find something stange:


`os-extended-volumes:volumes_attached  | [{"id": "111"}, {"id": "222"}]`

The two volumes are the ones I removed last time and as a result the detail page gets error.
Get into database and delete related records:

`
[nova]> select id,instance_uuid,volume_id,deleted from nova.block_device_mapping where volume_id in (select id from cinder.volumes where deleted=1) and deleted=0;
[nova]> delete from nova.block_device_mapping where volume_id in (select id from cinder.volumes where deleted=1) and deleted=0;
`

After these action, the dashboard detail page can be accessed now.
