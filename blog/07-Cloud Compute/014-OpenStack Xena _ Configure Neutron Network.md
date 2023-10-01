# OpenStack Xena : Configure Neutron Network
Configure Networking for Virtual Machine Instances.
Configure basic settings first for Neutron Services like All in One Settings like here.
For example, configure FLAT type of networking on here.
The Node has 2 network interfaces like follows.
<pre>
        eth0|10.0.0.30 
+-----------+-----------+
|    [ Control Node ]   |
|                       |
|  MariaDB    RabbitMQ  |
|  Memcached  httpd     |
|  Keystone   Glance    |
|   Nova API,Compute    |
|    Neutron Server     |
|  L2,L3,Metadata Agent |
+-----------+-----------+
        eth1|(UP with no IP)</pre>

## [1]	Configure Neutron services.
<pre>
# create a setting file for anonymous interface
# replace the name [eth1] to your environment
root@dlp ~(keystone)# vi /etc/systemd/network/eth1.network
[Match]
Name=eth1

[Network]
LinkLocalAddressing=no
IPv6AcceptRA=no

root@dlp ~(keystone)# ip link set eth1 up
root@dlp ~(keystone)# vi /etc/neutron/plugins/ml2/ml2_conf.ini
# line 206 : add
[ml2_type_flat]
flat_networks = physnet1
root@dlp ~(keystone)# vi /etc/neutron/plugins/ml2/linuxbridge_agent.ini
# line 190 : add
[linux_bridge]
physical_interface_mappings = physnet1:eth1
root@dlp ~(keystone)# systemctl restart neutron-linuxbridge-agent</pre>

## [2]	Create virtual network.
<pre>
root@dlp ~(keystone)# projectID=$(openstack project list | grep service | awk '{print $2}')
# create network named [sharednet1]
root@dlp ~(keystone)# openstack network create --project $projectID \
--share --provider-network-type flat --provider-physical-network physnet1 sharednet1
+---------------------------+--------------------------------------+
| Field                     | Value                                |
+---------------------------+--------------------------------------+
| admin_state_up            | UP                                   |
| availability_zone_hints   |                                      |
| availability_zones        |                                      |
| created_at                | 2021-10-07T01:23:49Z                 |
| description               |                                      |
| dns_domain                | None                                 |
| id                        | 1aae3fcd-2964-4729-aeb1-ec364642b68f |
| ipv4_address_scope        | None                                 |
| ipv6_address_scope        | None                                 |
| is_default                | False                                |
| is_vlan_transparent       | None                                 |
| mtu                       | 1500                                 |
| name                      | sharednet1                           |
| port_security_enabled     | True                                 |
| project_id                | 611e3ba8938b475fb9dd8124603f18d3     |
| provider:network_type     | flat                                 |
| provider:physical_network | physnet1                             |
| provider:segmentation_id  | None                                 |
| qos_policy_id             | None                                 |
| revision_number           | 1                                    |
| router:external           | Internal                             |
| segments                  | None                                 |
| shared                    | True                                 |
| status                    | ACTIVE                               |
| subnets                   |                                      |
| tags                      |                                      |
| updated_at                | 2021-10-07T01:23:49Z                 |
+---------------------------+--------------------------------------+

# create subnet [10.0.0.0/24] in [sharednet1]
root@dlp ~(keystone)# openstack subnet create subnet1 --network sharednet1 \
--project $projectID --subnet-range 10.0.0.0/24 \
--allocation-pool start=10.0.0.200,end=10.0.0.254 \
--gateway 10.0.0.1 --dns-nameserver 10.0.0.10
+----------------------+--------------------------------------+
| Field                | Value                                |
+----------------------+--------------------------------------+
| allocation_pools     | 10.0.0.200-10.0.0.254                |
| cidr                 | 10.0.0.0/24                          |
| created_at           | 2021-10-07T01:24:09Z                 |
| description          |                                      |
| dns_nameservers      | 10.0.0.10                            |
| dns_publish_fixed_ip | None                                 |
| enable_dhcp          | True                                 |
| gateway_ip           | 10.0.0.1                             |
| host_routes          |                                      |
| id                   | 64569780-a460-4cd9-8456-ca580448f061 |
| ip_version           | 4                                    |
| ipv6_address_mode    | None                                 |
| ipv6_ra_mode         | None                                 |
| name                 | subnet1                              |
| network_id           | 1aae3fcd-2964-4729-aeb1-ec364642b68f |
| project_id           | 611e3ba8938b475fb9dd8124603f18d3     |
| revision_number      | 0                                    |
| segment_id           | None                                 |
| service_types        |                                      |
| subnetpool_id        | None                                 |
| tags                 |                                      |
| updated_at           | 2021-10-07T01:24:09Z                 |
+----------------------+--------------------------------------+

# confirm settings
root@dlp ~(keystone)# openstack network list
+--------------------------------------+------------+--------------------------------------+
| ID                                   | Name       | Subnets                              |
+--------------------------------------+------------+--------------------------------------+
| 1aae3fcd-2964-4729-aeb1-ec364642b68f | sharednet1 | 64569780-a460-4cd9-8456-ca580448f061 |
+--------------------------------------+------------+--------------------------------------+

root@dlp ~(keystone)# openstack subnet list
+--------------------------------------+---------+--------------------------------------+-------------+
| ID                                   | Name    | Network                              | Subnet      |
+--------------------------------------+---------+--------------------------------------+-------------+
| 64569780-a460-4cd9-8456-ca580448f061 | subnet1 | 1aae3fcd-2964-4729-aeb1-ec364642b68f | 10.0.0.0/24 |
+--------------------------------------+---------+--------------------------------------+-------------+</pre>
