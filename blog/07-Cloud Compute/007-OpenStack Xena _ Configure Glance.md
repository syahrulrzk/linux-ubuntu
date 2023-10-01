# OpenStack Xena : Configure Glance

	
Install and Configure OpenStack Image Service (Glance).
This example is based on the environment like follows.
<pre>        eth0|10.0.0.30 
+-----------+-----------+
|    [ Control Node ]   |
|                       |
|  MariaDB    RabbitMQ  |
|  Memcached  httpd     |
|  Keystone   Glance    |
+-----------------------+</pre>

## [1]	Add users and others for Glance in Keystone.
<pre>
# create [glance] user in [service] project
root@dlp ~(keystone)# openstack user create --domain default --project service --password servicepassword glance
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| default_project_id  | 611e3ba8938b475fb9dd8124603f18d3 |
| domain_id           | default                          |
| enabled             | True                             |
| id                  | 67b144163ad54b4fb3bad7c731fe0330 |
| name                | glance                           |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+

# add [glance] user in [admin] role
root@dlp ~(keystone)# openstack role add --project service --user glance admin
# create service entry for [glance]
root@dlp ~(keystone)# openstack service create --name glance --description "OpenStack Image service" image
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | OpenStack Image service          |
| enabled     | True                             |
| id          | 63a0a52491594f1ea219faed41ea9ce5 |
| name        | glance                           |
| type        | image                            |
+-------------+----------------------------------+

# define Glance API Host
root@dlp ~(keystone)# export controller=10.0.0.30
# create endpoint for [glance] (public)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne image public http://$controller:9292
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 00db443012864f039b41a83bf54a792f |
| interface    | public                           |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 63a0a52491594f1ea219faed41ea9ce5 |
| service_name | glance                           |
| service_type | image                            |
| url          | http://10.0.0.30:9292            |
+--------------+----------------------------------+

# create endpoint for [glance] (internal)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne image internal http://$controller:9292
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | cc9e3fa8e50d44f1bc2c0ca175d74e6a |
| interface    | internal                         |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 63a0a52491594f1ea219faed41ea9ce5 |
| service_name | glance                           |
| service_type | image                            |
| url          | http://10.0.0.30:9292            |
+--------------+----------------------------------+

# create endpoint for [glance] (admin)
root@dlp ~(keystone)# openstack endpoint create --region RegionOne image admin http://$controller:9292
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 5173ec7e98674d06a3561125b9941975 |
| interface    | admin                            |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 63a0a52491594f1ea219faed41ea9ce5 |
| service_name | glance                           |
| service_type | image                            |
| url          | http://10.0.0.30:9292            |
+--------------+----------------------------------+</pre>

## [2]	Add a User and Database on MariaDB for Glance.
<pre>
root@dlp ~(keystone)# mysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 44
Server version: 10.3.31-MariaDB-0ubuntu0.20.04.1 Ubuntu 20.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database glance; 
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> grant all privileges on glance.* to glance@'localhost' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all privileges on glance.* to glance@'%' identified by 'password'; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges; 
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit
Bye</pre>

## [3]	Install Glance.
<pre>
root@dlp ~(keystone)# apt -y install glance</pre>

## [4]	Configure Glance.
<pre>
root@dlp ~(keystone)# mv /etc/glance/glance-api.conf /etc/glance/glance-api.conf.org
root@dlp ~(keystone)# vi /etc/glance/glance-api.conf
# create new
[DEFAULT]
bind_host = 0.0.0.0

[glance_store]
stores = file,http
default_store = file
filesystem_store_datadir = /var/lib/glance/images/

[database]
# MariaDB connection info
connection = mysql+pymysql://glance:password@10.0.0.30/glance

# keystone auth info
[keystone_authtoken]
www_authenticate_uri = http://10.0.0.30:5000
auth_url = http://10.0.0.30:5000
memcached_servers = 10.0.0.30:11211
auth_type = password
project_domain_name = default
user_domain_name = default
project_name = service
username = glance
password = servicepassword

[paste_deploy]
flavor = keystone

root@dlp ~(keystone)# chmod 640 /etc/glance/glance-api.conf
root@dlp ~(keystone)# chown root:glance /etc/glance/glance-api.conf
root@dlp ~(keystone)# su -s /bin/bash glance -c "glance-manage db_sync"
root@dlp ~(keystone)# systemctl restart glance-api
root@dlp ~(keystone)# systemctl enable glance-api</pre>
