---
layout: post
title: Openstack(Juno) Neutron config params
---

## Controller (with network node)

* neutron.conf

`egrep -v '^$|^#' neutron.conf`

    [DEFAULT]
    verbose = False
    debug = False
    state_path = /var/lib/neutron
    lock_path = $state_path/lock
    api_paste_config = api-paste.ini
    core_plugin = ml2
    service_plugins = router
    allow_overlapping_ips = True
    auth_strategy = keystone
    notify_nova_on_port_status_changes = True
    notify_nova_on_port_data_changes = True
    nova_url = http://controller:8774/v2
    nova_admin_username = nova
    nova_admin_tenant_id = service_tenant_id
    nova_admin_password = nova_pass
    nova_admin_auth_url = http://controller:35357/v2.0
    nova_api_insecure = False
    send_events_interval = 2
    rabbit_hosts=controller1,controller2,controller3
    rabbit_userid=openstack
    rabbit_password=password
    rabbit_virtual_host=/
    rabbit_ha_queues=true
    dhcp_agents_per_network = 3
    agent_down_time=15
    allow_automatic_l3agent_failover = True
    [agent]
    root_helper = sudo /usr/bin/neutron-rootwrap /etc/neutron/rootwrap.conf
    report_interval = 5
    [keystone_authtoken]
    auth_uri = http://controller:5000
    identity_uri = http://controller:35357
    admin_tenant_name = service
    admin_user = neutron
    admin_password = neutron_pass
    [database]
    connection = mysql://neutron:pass@controller/neutron
    idle_timeout=240

* ml2_conf.ini

`egrep -v '^$|^#' plugins/ml2/ml2_conf.ini`

    [ml2]
    type_drivers = flat,vlan
    tenant_network_types = vlan
    mechanism_drivers = openvswitch
    [ml2_type_flat]
    flat_networks = physnet1
    [ml2_type_vlan]
    network_vlan_ranges = physnet1:1000:2999
    [securitygroup]
    enable_security_group = True
    enable_ipset = True
    firewall_driver = neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver
    [ovs]
    tenant_network_type = vlan
    bridge_mappings = physnet1:br0

* dhcp_agent.ini

`egrep -v '^$|^#' dhcp_agent.ini `

    [DEFAULT]
    debug = False
    interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
    ovs_integration_bridge = br-int
    dhcp_driver = neutron.agent.linux.dhcp.Dnsmasq
    use_namespaces = True
    enable_isolated_metadata = False
    enable_metadata_network = False
    dnsmasq_dns_servers = 114.114.114.114

* metadata_agent.ini

`egrep -v '^$|^#' metadata_agent.ini`

    [DEFAULT]
    debug = False
    auth_url = http://controller:5000/v2.0
    auth_region = RegionOne
    admin_tenant_name = service
    admin_user = neutron
    admin_password = neutron_pass
    nova_metadata_ip = controller
    nova_metadata_port = 8775
    nova_metadata_protocol = http
    metadata_proxy_shared_secret = shared_secret

* l3_agent.ini

`egrep -v '^$|^#' l3_agent.ini`

    [DEFAULT]
    debug = False
    interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
    use_namespaces = True
    external_network_bridge = 

## Compute

* neutron.conf

`egrep -v '^#|^$' neutron.conf`

    [DEFAULT]
    verbose = False
    debug = False
    state_path = /var/lib/neutron
    lock_path = $state_path/lock
    api_paste_config = api-paste.ini
    core_plugin = ml2
    service_plugins = router
    allow_overlapping_ips = True
    auth_strategy = keystone
    notify_nova_on_port_status_changes = True
    notify_nova_on_port_data_changes = True
    nova_url = http://controller:8774/v2
    nova_admin_username = nova
    nova_admin_tenant_id = service_tenant_id
    nova_admin_password = nova_pass
    nova_admin_auth_url = http://controller:35357/v2.0
    nova_api_insecure = False
    send_events_interval = 2
    rabbit_hosts=controller1,controller2,controller3
    rabbit_userid=openstack
    rabbit_password=password
    rabbit_virtual_host=/
    rabbit_ha_queues=true
    dhcp_agents_per_network = 3
    agent_down_time=15
    allow_automatic_l3agent_failover = True
    [agent]
    root_helper = sudo /usr/bin/neutron-rootwrap /etc/neutron/rootwrap.conf
    report_interval = 5
    [keystone_authtoken]
    auth_uri = http://controller:5000
    identity_uri = http://controller:35357
    admin_tenant_name = service
    admin_user = neutron
    admin_password = neutron_pass
    [database]
    connection = mysql://neutron:pass@controller/neutron
    idle_timeout=240

* ml2_conf.ini

`egrep -v '^#|^$' plugins/ml2/ml2_conf.ini`

    [ml2]
    type_drivers = flat,vlan
    tenant_network_types = vlan
    mechanism_drivers = openvswitch
    [ml2_type_flat]
    flat_networks = physnet1
    [ml2_type_vlan]
    network_vlan_ranges = physnet1:1000:2999
    [securitygroup]
    enable_security_group = True
    enable_ipset = True
    firewall_driver = neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver
    [ovs]
    tenant_network_type = vlan
    bridge_mappings = physnet1:br0

## Architecture

![server with switches](../images/server-with-switch.jpg "server connect with switch")

![controller/network node virtual devices inside](../images/controller-devices.jpg "bridge inside controller")

![compute virtual devices insed](../images/compute-devices.jpg "bridge inside compute")

