# OpenStack Xena : Configure Nova #1

OpenStack Xena : Configure Nova #12021/10/07
 	
Install and Configure OpenStack Compute Service (Nova).
This example is based on the environment like follows.
<pre>
        eth0|10.0.0.30 
+-----------+-----------+
|    [ Control Node ]   |
|                       |
|  MariaDB    RabbitMQ  |
|  Memcached  httpd     |
|  Keystone   Glance    |
+-----------------------+</pre>

## [1]	Add users and others for Nova in Keystone.

<pre>
# create [nova] user in [service] project
root@dlp ~(keystone)# openstack user create --domain default --project service --password servicepassword nova
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| default_project_id  | 611e3ba8938b475fb9dd8124603f18d3 |
| domain_id           | default                          |
| enabled             | True                             |
| id                  | 77db0f264e8647808f3823c2f20d5801 |
| name                | nova                             |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+

# add [nova] user in [admin] role
root@dlp ~(keystone)# openstack role add --project service --user nova admin
# create [placement] user in [service] project
root@dlp ~(keystone)# openstack user create --domain default --project service --password servicepassword placement
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| default_project_id  | 611e3ba8938b475fb9dd8124603f18d3 |
| domain_id           | default                          |
| enabled             | True                             |
| id                  | 73565cb500354a309fa8174450ead2cd |
| name                | placement                        |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+

# add [placement] user in [admin] role
root@dlp ~(keystone)# openstack role add --project service --user placement admin
# create service entry for [nova]
root@dlp ~(keystone)# openstack service create --name nova --description "OpenStack Compute service" compute
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | OpenStack Compute service        |
| enabled     | True                             |
| id          | 55c16f5a323e45f2a7a05118f97831a3 |
| name        | nova                             |
| type        | compute                          |
+-------------+----------------------------------+

# create service entry for [placement]
root@dlp ~(keystone)# openstack service create --name placement --description "OpenStack Compute Placement service" placement
+-------------+-------------------------------------+
| Field       | Value                               |
+-------------+-------------------------------------+
| description | OpenStack Compute Placement service |
| enabled     | True                                |
| id          | 6b6e58bc01e3474d90f4929f68f9afc8    |
| name        | placement                           |
| type        | placement                           |
+-------------+-------------------------------------+

# define Nova API Host
root@dlp ~(keystone)# export controller=10.0.0.30
# create endpoint for [nova] (public)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne compute public http://$controller:8774/v2.1/%\(tenant_id\)s
+--------------+------------------------------------------+
| Field        | Value                                    |
+--------------+------------------------------------------+
| enabled      | True                                     |
| id           | baf2621c314e4089baf32ec955242698         |
| interface    | public                                   |
| region       | RegionOne                                |
| region_id    | RegionOne                                |
| service_id   | 55c16f5a323e45f2a7a05118f97831a3         |
| service_name | nova                                     |
| service_type | compute                                  |
| url          | http://10.0.0.30:8774/v2.1/%(tenant_id)s |
+--------------+------------------------------------------+

# create endpoint for [nova] (internal)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne compute internal http://$controller:8774/v2.1/%\(tenant_id\)s
+--------------+------------------------------------------+
| Field        | Value                                    |
+--------------+------------------------------------------+
| enabled      | True                                     |
| id           | 6c9df580a2b1451dba604e029926a317         |
| interface    | internal                                 |
| region       | RegionOne                                |
| region_id    | RegionOne                                |
| service_id   | 55c16f5a323e45f2a7a05118f97831a3         |
| service_name | nova                                     |
| service_type | compute                                  |
| url          | http://10.0.0.30:8774/v2.1/%(tenant_id)s |
+--------------+------------------------------------------+

# create endpoint for [nova] (admin)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne compute admin http://$controller:8774/v2.1/%\(tenant_id\)s
+--------------+------------------------------------------+
| Field        | Value                                    |
+--------------+------------------------------------------+
| enabled      | True                                     |
| id           | 976c1eb0506645218e642725ad6b7ada         |
| interface    | admin                                    |
| region       | RegionOne                                |
| region_id    | RegionOne                                |
| service_id   | 55c16f5a323e45f2a7a05118f97831a3         |
| service_name | nova                                     |
| service_type | compute                                  |
| url          | http://10.0.0.30:8774/v2.1/%(tenant_id)s |
+--------------+------------------------------------------+

# create endpoint for [placement] (public)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne placement public http://$controller:8778
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 5d5ee9684ac942a0bdb23fd729f62fa6 |
| interface    | public                           |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 6b6e58bc01e3474d90f4929f68f9afc8 |
| service_name | placement                        |
| service_type | placement                        |
| url          | http://10.0.0.30:8778            |
+--------------+----------------------------------+

# create endpoint for [placement] (internal)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne placement internal http://$controller:8778
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | d571bc529f4a4a848fd5cf9c2f5c6d31 |
| interface    | internal                         |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 6b6e58bc01e3474d90f4929f68f9afc8 |
| service_name | placement                        |
| service_type | placement                        |
| url          | http://10.0.0.30:8778            |
+--------------+----------------------------------+

# create endpoint for [placement] (admin)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne placement admin http://$controller:8778
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | faf25b92080b41518a9a259f504ecb8a |
| interface    | admin                            |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 6b6e58bc01e3474d90f4929f68f9afc8 |
| service_name | placement                        |
| service_type | placement                        |
| url          | http://10.0.0.30:8778            |
+--------------+----------------------------------+</pre>

## [2]	Add a User and Database on MariaDB for Nova.

<pre>
root@dlp ~(keystone)# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 49
Server version: 10.3.31-MariaDB-0ubuntu0.20.04.1 Ubuntu 20.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database nova; 
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all privileges on nova.* to nova@'localhost' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on nova.* to nova@'%' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> create database nova_api; 
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all privileges on nova_api.* to nova@'localhost' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on nova_api.* to nova@'%' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> create database placement; 
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all privileges on placement.* to placement@'localhost' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on placement.* to placement@'%' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> create database nova_cell0; 
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all privileges on nova_cell0.* to nova@'localhost' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on nova_cell0.* to nova@'%' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit
Bye</pre>
