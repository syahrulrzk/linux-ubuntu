# OpenStack Xena : Configure Neutron #2

Configure OpenStack Network Service (Neutron).
This example is based on the emvironment like follows.
If you'd like to install Neutron services on another Host, refer to here.
Neutron needs a plugin software, it's possible to choose it from some softwares.
This example chooses ML2 plugin. ( it uses LinuxBridge under the backend )
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
+-----------------------+</pre>

## [1]	Install Neutron services.
<pre>root@dlp ~(keystone)# apt -y install neutron-server neutron-plugin-ml2 neutron-linuxbridge-agent neutron-l3-agent neutron-dhcp-agent neutron-metadata-agent python3-neutronclient</pre>

## [2]	Configure Neutron.
<pre>
root@dlp ~(keystone)# mv /etc/neutron/neutron.conf /etc/neutron/neutron.conf.org
root@dlp ~(keystone)# vi /etc/neutron/neutron.conf
# create new
[DEFAULT]
core_plugin = ml2
service_plugins = router
auth_strategy = keystone
state_path = /var/lib/neutron
dhcp_agent_notification = True
allow_overlapping_ips = True
notify_nova_on_port_status_changes = True
notify_nova_on_port_data_changes = True
# RabbitMQ connection info
transport_url = rabbit://openstack:password@10.0.0.30

[agent]
root_helper = sudo /usr/bin/neutron-rootwrap /etc/neutron/rootwrap.conf

# Keystone auth info
[keystone_authtoken]
www_authenticate_uri = http://10.0.0.30:5000
auth_url = http://10.0.0.30:5000
memcached_servers = 10.0.0.30:11211
auth_type = password
project_domain_name = default
user_domain_name = default
project_name = service
username = neutron
password = servicepassword

# MariaDB connection info
[database]
connection = mysql+pymysql://neutron:password@10.0.0.30/neutron_ml2

# Nova connection info
[nova]
auth_url = http://10.0.0.30:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = nova
password = servicepassword

[oslo_concurrency]
lock_path = $state_path/tmp

root@dlp ~(keystone)# touch /etc/neutron/fwaas_driver.ini
root@dlp ~(keystone)# chmod 640 /etc/neutron/{neutron.conf,fwaas_driver.ini}
root@dlp ~(keystone)# chgrp neutron /etc/neutron/{neutron.conf,fwaas_driver.ini}
root@dlp ~(keystone)# vi /etc/neutron/l3_agent.ini
# line 21 : add
interface_driver = linuxbridge
root@dlp ~(keystone)# vi /etc/neutron/dhcp_agent.ini
# line 21 : add
interface_driver = linuxbridge
# line 43 : uncomment
dhcp_driver = neutron.agent.linux.dhcp.Dnsmasq
# line 52 : uncomment and change
enable_isolated_metadata = true
root@dlp ~(keystone)# vi /etc/neutron/metadata_agent.ini
# line 22 : uncomment and specify Nova API server
nova_metadata_host = 10.0.0.30
# line 34 : uncomment and specify any secret key you like
metadata_proxy_shared_secret = metadata_secret
# line 312 : add to specify Memcache Server
memcache_servers = 10.0.0.30:11211
root@dlp ~(keystone)# vi /etc/neutron/plugins/ml2/ml2_conf.ini
# line 154 : add
# OK with no value for [tenant_network_types] now (set later if need)
[ml2]
type_drivers = flat,vlan,vxlan
tenant_network_types =
mechanism_drivers = linuxbridge
extension_drivers = port_security
root@dlp ~(keystone)# vi /etc/neutron/plugins/ml2/linuxbridge_agent.ini
# line 225 : add
[securitygroup]
enable_security_group = True
firewall_driver = iptables
enable_ipset = True
# line 284 : add own IP address
local_ip = 10.0.0.30
root@dlp ~(keystone)# vi /etc/nova/nova.conf
# add follows into [DEFAULT] section
use_neutron = True
vif_plugging_is_fatal = True
vif_plugging_timeout = 300

# add follows to the end : Neutron auth info
# the value of [metadata_proxy_shared_secret] is the same with the one in [metadata_agent.ini]
[neutron]
auth_url = http://10.0.0.30:5000
auth_type = password
project_domain_name = default
user_domain_name = default
region_name = RegionOne
project_name = service
username = neutron
password = servicepassword
service_metadata_proxy = True
metadata_proxy_shared_secret = metadata_secret</pre>

## [3]	Start Neutron services.
<pre>
root@dlp ~(keystone)# ln -s /etc/neutron/plugins/ml2/ml2_conf.ini /etc/neutron/plugin.ini
root@dlp ~(keystone)# su -s /bin/bash neutron -c "neutron-db-manage --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugin.ini upgrade head"
root@dlp ~(keystone)# for service in server l3-agent dhcp-agent metadata-agent linuxbridge-agent; do
systemctl restart neutron-$service
systemctl enable neutron-$service
done
root@dlp ~(keystone)# systemctl restart nova-api nova-compute
# show status
root@dlp ~(keystone)# openstack network agent list
+--------------------------------------+--------------------+---------------+-------------------+-------+-------+---------------------------+
| ID                                   | Agent Type         | Host          | Availability Zone | Alive | State | Binary                    |
+--------------------------------------+--------------------+---------------+-------------------+-------+-------+---------------------------+
| 21e137a5-7bbc-40fe-9834-f7071c1be898 | Linux bridge agent | dlp.srv.world | None              | :-)   | UP    | neutron-linuxbridge-agent |
| 2cc09270-407f-4fcb-b022-dd8d008e8f26 | L3 agent           | dlp.srv.world | nova              | :-)   | UP    | neutron-l3-agent          |
| 46f04768-0cb6-4c67-9f0e-c1672bcf272d | Metadata agent     | dlp.srv.world | None              | :-)   | UP    | neutron-metadata-agent    |
| b46ead8c-a68e-4d29-a5f6-abc9d6edf4c4 | DHCP agent         | dlp.srv.world | nova              | :-)   | UP    | neutron-dhcp-agent        |
+--------------------------------------+--------------------+---------------+-------------------+-------+-------+---------------------------+</pre>
