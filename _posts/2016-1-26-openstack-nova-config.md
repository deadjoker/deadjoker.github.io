---
layout: post
title: Openstack(Juno) Nova config params
---

## Controller (with network node)

* nova.conf

`egrep -v '^$|^#' nova.conf`

    [DEFAULT]
    verbose=False
    debug=False
    dhcpbridge_flagfile=/etc/nova/nova.conf
    dhcpbridge=/usr/bin/nova-dhcpbridge
    logdir=/var/log/nova
    state_path=/var/lib/nova
    lock_path=/var/lock/nova
    force_dhcp_release=True
    libvirt_use_virtio_for_bridges=True
    api_paste_config=/etc/nova/api-paste.ini
    enabled_apis=osapi_compute,metadata
    auth_strategy = keystone
    memcached_servers=controller1,controller2,controller3
    rpc_backend = rabbit
    rabbit_hosts = controller1,controller2,controller3
    rabbit_userid = openstack
    rabbit_password = password
    rabbit_ha_queues = true
    my_ip = x.x.x.x
    vnc_enabled = True
    vncserver_listen = 0.0.0.0
    vncserver_proxyclient_address = $my_ip
    novncproxy_base_url = http://xx.xx.xx.xx:6080/vnc_auto.html
    network_api_class = nova.network.neutronv2.api.API
    security_group_api = neutron
    linuxnet_interface_driver = nova.network.linux_net.LinuxBridgeInterfaceDriver
    firewall_driver = nova.virt.firewall.NoopFirewallDriver
    resize_confirm_window=1
    allow_resize_to_same_host=true
    [glance]
    host = controller
    [neutron]
    url = http://controller:9696
    auth_strategy = keystone
    admin_auth_url = http://controller:35357/v2.0
    admin_tenant_name = service
    admin_username = neutron
    admin_password = neutron_pass
    service_metadata_proxy = True
    metadata_proxy_shared_secret = share_secret
    [database]
    connection=mysql://nova:password@controller/nova
    idle_timeout=240
    [keystone_authtoken]
    auth_uri = http://controller:5000
    identity_uri = http://controller:35357
    admin_tenant_name = service
    admin_user = nova
    admin_password = nova_pass
    [libvirt]
    images_type = rbd
    images_rbd_pool = vms
    images_rbd_ceph_conf = /etc/ceph/ceph.conf
    rbd_user = cinder
    rbd_secret_uuid = ceph_secret
    inject_password = false
    inject_key = false
    inject_partition = -2
    disk_cachemodes = network=writeback
    live_migration_flag="VIR_MIGRATE_UNDEFINE_SOURCE,VIR_MIGRATE_PEER2PEER,VIR_MIGRATE_LIVE,VIR_MIGRATE_PERSIST_DEST"
    iscsi_use_multipath=true
    use_usb_tablet=false

## Compute

* nova.conf

`egrep -v '^#|^$' nova.conf`

    [DEFAULT]
    verbose=False
    debug=False
    dhcpbridge_flagfile=/etc/nova/nova.conf
    dhcpbridge=/usr/bin/nova-dhcpbridge
    logdir=/var/log/nova
    state_path=/var/lib/nova
    lock_path=/var/lock/nova
    force_dhcp_release=True
    libvirt_use_virtio_for_bridges=True
    api_paste_config=/etc/nova/api-paste.ini
    enabled_apis=osapi_compute,metadata
    auth_strategy = keystone
    memcached_servers=controller1,controller2,controller3
    rpc_backend = rabbit
    rabbit_hosts = controller1,controller2,controller3
    rabbit_userid = openstack
    rabbit_password = password
    rabbit_ha_queues = true
    my_ip = x.x.x.x
    vnc_enabled = True
    vncserver_listen = 0.0.0.0
    vncserver_proxyclient_address = $my_ip
    novncproxy_base_url = http://xx.xx.xx.xx:6080/vnc_auto.html
    network_api_class = nova.network.neutronv2.api.API
    security_group_api = neutron
    linuxnet_interface_driver = nova.network.linux_net.LinuxBridgeInterfaceDriver
    firewall_driver = nova.virt.firewall.NoopFirewallDriver
    resize_confirm_window=1
    allow_resize_to_same_host=true
    [glance]
    host = controller
    [neutron]
    url = http://controller:9696
    auth_strategy = keystone
    admin_auth_url = http://controller:35357/v2.0
    admin_tenant_name = service
    admin_username = neutron
    admin_password = neutron_pass
    service_metadata_proxy = True
    metadata_proxy_shared_secret = share_secret
    [database]
    connection=mysql://nova:password@controller/nova
    idle_timeout=240
    [keystone_authtoken]
    auth_uri = http://controller:5000
    identity_uri = http://controller:35357
    admin_tenant_name = service
    admin_user = nova
    admin_password = nova_pass
    [libvirt]
    images_type = rbd
    images_rbd_pool = vms
    images_rbd_ceph_conf = /etc/ceph/ceph.conf
    rbd_user = cinder
    rbd_secret_uuid = ceph_secret
    inject_password = false
    inject_key = false
    inject_partition = -2
    disk_cachemodes = network=writeback
    live_migration_flag="VIR_MIGRATE_UNDEFINE_SOURCE,VIR_MIGRATE_PEER2PEER,VIR_MIGRATE_LIVE,VIR_MIGRATE_PERSIST_DEST"
    iscsi_use_multipath=true
    use_usb_tablet=false

* nova-compute.conf

`cat nova-compute.conf`

    [DEFAULT]
    compute_driver=libvirt.LibvirtDriver
    [libvirt]
    virt_type=kvm

