# OpenStack Xena : Konfigurasi Neutron #1
	
Configure OpenStack Network Service (Neutron).
This example is based on the emvironment like follows.
<pre>
        eth0|10.0.0.30 
+-----------+-----------+
|    [ Control Node ]   |
|                       |
|  MariaDB    RabbitMQ  |
|  Memcached  httpd     |
|  Keystone   Glance    |
|  Nova API             |
+-----------------------+</pre>

## [1]	Add user or service for Neutron on Keystone.
<pre>
# create [neutron] user in [service] project
root@dlp ~(keystone)# openstack user create --domain default --project service --password servicepassword neutron
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| default_project_id  | 611e3ba8938b475fb9dd8124603f18d3 |
| domain_id           | default                          |
| enabled             | True                             |
| id                  | 04987f038948463eb2b237161342adde |
| name                | neutron                          |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+

# add [neutron] user in [admin] role
root@dlp ~(keystone)# openstack role add --project service --user neutron admin
# create service entry for [neutron]
root@dlp ~(keystone)# openstack service create --name neutron --description "OpenStack Networking service" network
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | OpenStack Networking service     |
| enabled     | True                             |
| id          | 209d9dfebe3d45fa937ba6bb2b34d94b |
| name        | neutron                          |
| type        | network                          |
+-------------+----------------------------------+

# define Neutron API Host
root@dlp ~(keystone)# export controller=10.0.0.30
# create endpoint for [neutron] (public)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne network public http://$controller:9696
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 2c3ddde372aa468c8f3795091346ed91 |
| interface    | public                           |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 209d9dfebe3d45fa937ba6bb2b34d94b |
| service_name | neutron                          |
| service_type | network                          |
| url          | http://10.0.0.30:9696            |
+--------------+----------------------------------+

# create endpoint for [neutron] (internal)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne network internal http://$controller:9696
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 435abae9ee41450096065bc3c44f061a |
| interface    | internal                         |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 209d9dfebe3d45fa937ba6bb2b34d94b |
| service_name | neutron                          |
| service_type | network                          |
| url          | http://10.0.0.30:9696            |
+--------------+----------------------------------+

# create endpoint for [neutron] (admin)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne network admin http://$controller:9696
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 295f8db6de3c4c4e9b7678206847b731 |
| interface    | admin                            |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 209d9dfebe3d45fa937ba6bb2b34d94b |
| service_name | neutron                          |
| service_type | network                          |
| url          | http://10.0.0.30:9696            |
+--------------+----------------------------------+</pre>

## [2]	Add a User and Database on MariaDB for Neutron.
<pre>
root@dlp ~(keystone)# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 103
Server version: 10.3.31-MariaDB-0ubuntu0.20.04.1 Ubuntu 20.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database neutron_ml2; 
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all privileges on neutron_ml2.* to neutron@'localhost' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on neutron_ml2.* to neutron@'%' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit 
Bye</pre>
