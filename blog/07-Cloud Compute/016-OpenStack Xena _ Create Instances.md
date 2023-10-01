# OpenStack Xena : Create Instances
	
Create and Start Virtual Machine Instances.
[1]	Login as a common user and create a config for authentication of Keystyone.
The username or password in the config are just the one you added in keystone like here.
Next Create and run an instance.
<pre>
ubuntu@dlp:~$ vi ~/keystonerc
export OS_PROJECT_DOMAIN_NAME=default
export OS_USER_DOMAIN_NAME=default
export OS_PROJECT_NAME=hiroshima
export OS_USERNAME=serverworld
export OS_PASSWORD=userpassword
export OS_AUTH_URL=http://10.0.0.30:5000/v3
export OS_IDENTITY_API_VERSION=3
export OS_IMAGE_API_VERSION=2
export PS1='\u@\h \W(keystone)\$ '
ubuntu@dlp:~$ chmod 600 ~/keystonerc
ubuntu@dlp:~$ source ~/keystonerc
ubuntu@dlp ~(keystone)$ echo "source ~/keystonerc " >> ~/.bash_profile
# confirm available [flavor] list
ubuntu@dlp ~(keystone)$ openstack flavor list
+----+----------+------+------+-----------+-------+-----------+
| ID | Name     |  RAM | Disk | Ephemeral | VCPUs | Is Public |
+----+----------+------+------+-----------+-------+-----------+
| 0  | m1.small | 2048 |   10 |         0 |     1 | True      |
+----+----------+------+------+-----------+-------+-----------+

# confirm available image list
ubuntu@dlp ~(keystone)$ openstack image list
+--------------------------------------+------------+--------+
| ID                                   | Name       | Status |
+--------------------------------------+------------+--------+
| ba8781c7-85ef-4872-957e-2c27b55275b2 | Ubuntu2004 | active |
+--------------------------------------+------------+--------+

# confirm available network list
ubuntu@dlp ~(keystone)$ openstack network list
+--------------------------------------+------------+--------------------------------------+
| ID                                   | Name       | Subnets                              |
+--------------------------------------+------------+--------------------------------------+
| 1aae3fcd-2964-4729-aeb1-ec364642b68f | sharednet1 | 64569780-a460-4cd9-8456-ca580448f061 |
+--------------------------------------+------------+--------------------------------------+

# create a security group for instances
ubuntu@dlp ~(keystone)$ openstack security group create secgroup01
+-----------------+-----------------------------------------------------------------------------+
| Field           | Value                                                                       |
+-----------------+-----------------------------------------------------------------------------+
| created_at      | 2021-10-07T01:29:02Z                                                        |
| description     | secgroup01                                                                  |
| id              | 836ea363-79c3-44b3-ad32-32c736f35566                                        |
| name            | secgroup01                                                                  |
| project_id      | 9da4979990a24fe1a7a41c9d93b87383                                            |
| revision_number | 1                                                                           |
| rules           | created_at='2021-10-07T01:29:02Z', direction='egress', ethertype='IPv6..... |
|                 | created_at='2021-10-07T01:29:02Z', direction='egress', ethertype='IPv4..... |
| stateful        | True                                                                        |
| tags            | []                                                                          |
| updated_at      | 2021-10-07T01:29:02Z                                                        |
+-----------------+-----------------------------------------------------------------------------+

ubuntu@dlp ~(keystone)$ openstack security group list
+--------------------------------------+------------+------------------------+----------------------------------+------+
| ID                                   | Name       | Description            | Project                          | Tags |
+--------------------------------------+------------+------------------------+----------------------------------+------+
| 836ea363-79c3-44b3-ad32-32c736f35566 | secgroup01 | secgroup01             | 9da4979990a24fe1a7a41c9d93b87383 | []   |
| f5b4e8b4-6b58-4adb-acfc-3d5ad349d144 | default    | Default security group | 9da4979990a24fe1a7a41c9d93b87383 | []   |
+--------------------------------------+------------+------------------------+----------------------------------+------+

# create a SSH keypair for connecting to instances
ubuntu@dlp ~(keystone)$ ssh-keygen -q -N ""
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
# add public-key
ubuntu@dlp ~(keystone)$ openstack keypair create --public-key ~/.ssh/id_rsa.pub mykey
+-------------+-------------------------------------------------+
| Field       | Value                                           |
+-------------+-------------------------------------------------+
| created_at  | None                                            |
| fingerprint | 20:f0:47:11:85:6a:62:73:8e:59:ed:a6:32:8b:12:ea |
| id          | mykey                                           |
| is_deleted  | None                                            |
| name        | mykey                                           |
| type        | ssh                                             |
| user_id     | 9bd544cf5ed14a1ab2b1e3f90bc0b801                |
+-------------+-------------------------------------------------+

ubuntu@dlp ~(keystone)$ openstack keypair list
+-------+-------------------------------------------------+------+
| Name  | Fingerprint                                     | Type |
+-------+-------------------------------------------------+------+
| mykey | 20:f0:47:11:85:6a:62:73:8e:59:ed:a6:32:8b:12:ea | ssh  |
+-------+-------------------------------------------------+------+

ubuntu@dlp ~(keystone)$ netID=$(openstack network list | grep sharednet1 | awk '{ print $2 }')
# create and boot an instance
ubuntu@dlp ~(keystone)$ openstack server create --flavor m1.small --image Ubuntu2004 --security-group secgroup01 --nic net-id=$netID --key-name mykey Ubuntu-2004
+-----------------------------+---------------------------------------------------+
| Field                       | Value                                             |
+-----------------------------+---------------------------------------------------+
| OS-DCF:diskConfig           | MANUAL                                            |
| OS-EXT-AZ:availability_zone |                                                   |
| OS-EXT-STS:power_state      | NOSTATE                                           |
| OS-EXT-STS:task_state       | scheduling                                        |
| OS-EXT-STS:vm_state         | building                                          |
| OS-SRV-USG:launched_at      | None                                              |
| OS-SRV-USG:terminated_at    | None                                              |
| accessIPv4                  |                                                   |
| accessIPv6                  |                                                   |
| addresses                   |                                                   |
| adminPass                   | X3f83LvBkXCC                                      |
| config_drive                |                                                   |
| created                     | 2021-10-07T01:32:11Z                              |
| flavor                      | m1.small (0)                                      |
| hostId                      |                                                   |
| id                          | 00755654-0c45-4f68-9acc-af0ecee46b9a              |
| image                       | Ubuntu2004 (ba8781c7-85ef-4872-957e-2c27b55275b2) |
| key_name                    | mykey                                             |
| name                        | Ubuntu-2004                                       |
| progress                    | 0                                                 |
| project_id                  | 9da4979990a24fe1a7a41c9d93b87383                  |
| properties                  |                                                   |
| security_groups             | name='836ea363-79c3-44b3-ad32-32c736f35566'       |
| status                      | BUILD                                             |
| updated                     | 2021-10-07T01:32:11Z                              |
| user_id                     | 9bd544cf5ed14a1ab2b1e3f90bc0b801                  |
| volumes_attached            |                                                   |
+-----------------------------+---------------------------------------------------+

# show status ([BUILD] status is shown when building instance)
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+--------+----------+------------+----------+
| ID                                   | Name        | Status | Networks | Image      | Flavor   |
+--------------------------------------+-------------+--------+----------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | BUILD  |          | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+--------+----------+------------+----------+

# when starting noramlly, the status turns to [ACTIVE]
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| ID                                   | Name        | Status | Networks              | Image      | Flavor   |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | ACTIVE | sharednet1=10.0.0.250 | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+</pre>

## [2]	Configure security settings for the security group you created above to access with SSH and ICMP.
<pre>
# permit ICMP
ubuntu@dlp ~(keystone)$ openstack security group rule create --protocol icmp --ingress secgroup01
+-------------------------+--------------------------------------+
| Field                   | Value                                |
+-------------------------+--------------------------------------+
| created_at              | 2021-10-07T01:33:08Z                 |
| description             |                                      |
| direction               | ingress                              |
| ether_type              | IPv4                                 |
| id                      | 36593b59-b118-4ed2-b076-4728b58d4c18 |
| name                    | None                                 |
| port_range_max          | None                                 |
| port_range_min          | None                                 |
| project_id              | 9da4979990a24fe1a7a41c9d93b87383     |
| protocol                | icmp                                 |
| remote_address_group_id | None                                 |
| remote_group_id         | None                                 |
| remote_ip_prefix        | 0.0.0.0/0                            |
| revision_number         | 0                                    |
| security_group_id       | 836ea363-79c3-44b3-ad32-32c736f35566 |
| tags                    | []                                   |
| updated_at              | 2021-10-07T01:33:08Z                 |
+-------------------------+--------------------------------------+

# permit SSH
ubuntu@dlp ~(keystone)$ openstack security group rule create --protocol tcp --dst-port 22:22 secgroup01
+-------------------------+--------------------------------------+
| Field                   | Value                                |
+-------------------------+--------------------------------------+
| created_at              | 2021-10-07T01:33:22Z                 |
| description             |                                      |
| direction               | ingress                              |
| ether_type              | IPv4                                 |
| id                      | 21457da9-ba78-477c-a654-2d271db17b7a |
| name                    | None                                 |
| port_range_max          | 22                                   |
| port_range_min          | 22                                   |
| project_id              | 9da4979990a24fe1a7a41c9d93b87383     |
| protocol                | tcp                                  |
| remote_address_group_id | None                                 |
| remote_group_id         | None                                 |
| remote_ip_prefix        | 0.0.0.0/0                            |
| revision_number         | 0                                    |
| security_group_id       | 836ea363-79c3-44b3-ad32-32c736f35566 |
| tags                    | []                                   |
| updated_at              | 2021-10-07T01:33:22Z                 |
+-------------------------+--------------------------------------+

ubuntu@dlp ~(keystone)$ openstack security group rule list secgroup01
+--------------------------------------+-------------+-----------+-----------+------------+-----------+-----------------------+----------------------+
| ID                                   | IP Protocol | Ethertype | IP Range  | Port Range | Direction | Remote Security Group | Remote Address Group |
+--------------------------------------+-------------+-----------+-----------+------------+-----------+-----------------------+----------------------+
| 21457da9-ba78-477c-a654-2d271db17b7a | tcp         | IPv4      | 0.0.0.0/0 | 22:22      | ingress   | None                  | None                 |
| 36593b59-b118-4ed2-b076-4728b58d4c18 | icmp        | IPv4      | 0.0.0.0/0 |            | ingress   | None                  | None                 |
| 452496b0-a71b-491b-8df4-de0fb24586f0 | None        | IPv6      | ::/0      |            | egress    | None                  | None                 |
| 712e3954-11b4-43b3-a738-afc9e24d4fcc | None        | IPv4      | 0.0.0.0/0 |            | egress    | None                  | None                 |
+--------------------------------------+-------------+-----------+-----------+------------+-----------+-----------------------+----------------------+</pre>

## [3]	Login to the instance with SSH.
<pre>
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| ID                                   | Name        | Status | Networks              | Image      | Flavor   |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | ACTIVE | sharednet1=10.0.0.250 | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+

ubuntu@dlp ~(keystone)$ ping 10.0.0.250 -c3
PING 10.0.0.250 (10.0.0.250) 56(84) bytes of data.
64 bytes from 10.0.0.250: icmp_seq=1 ttl=64 time=1.76 ms
64 bytes from 10.0.0.250: icmp_seq=2 ttl=64 time=0.821 ms
64 bytes from 10.0.0.250: icmp_seq=3 ttl=64 time=0.545 ms

--- 10.0.0.250 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2021ms
rtt min/avg/max/mdev = 0.545/1.042/1.761/0.520 ms

ubuntu@dlp ~(keystone)$ ssh ubuntu@10.0.0.250
The authenticity of host '10.0.0.250 (10.0.0.250)' can't be established.
ECDSA key fingerprint is SHA256:5H/27oI+FCy9QkWjvU8KautFI4/kVHiMTAFPBVdabgQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.0.250' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-88-generic x86_64)

.....
.....

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ubuntu-2004:~$     # logined</pre>

## [4]	If you'd like to stop an instance, it's possible to control with openstack command like follows.
<pre>
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| ID                                   | Name        | Status | Networks              | Image      | Flavor   |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | ACTIVE | sharednet1=10.0.0.250 | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+

# stop instance
ubuntu@dlp ~(keystone)$ openstack server stop Ubuntu-2004
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+---------+-----------------------+------------+----------+
| ID                                   | Name        | Status  | Networks              | Image      | Flavor   |
+--------------------------------------+-------------+---------+-----------------------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | SHUTOFF | sharednet1=10.0.0.250 | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+---------+-----------------------+------------+----------+

# start instance
ubuntu@dlp ~(keystone)$ openstack server start Ubuntu-2004
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| ID                                   | Name        | Status | Networks              | Image      | Flavor   |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | ACTIVE | sharednet1=10.0.0.250 | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+</pre>

## [5]	It's possible to access with Web browser to get VNC console.
<pre>
ubuntu@dlp ~(keystone)$ openstack server list
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| ID                                   | Name        | Status | Networks              | Image      | Flavor   |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+
| 00755654-0c45-4f68-9acc-af0ecee46b9a | Ubuntu-2004 | ACTIVE | sharednet1=10.0.0.250 | Ubuntu2004 | m1.small |
+--------------------------------------+-------------+--------+-----------------------+------------+----------+

ubuntu@dlp ~(keystone)$ openstack console url show Ubuntu-2004
+----------+------------------------------------------------------------------------------------------+
| Field    | Value                                                                                    |
+----------+------------------------------------------------------------------------------------------+
| protocol | vnc                                                                                      |
| type     | novnc                                                                                    |
| url      | http://10.0.0.30:6080/vnc_auto.html?path=%3Ftoken%3Dd2612b0c-0f36-4d8f-96c1-8d7e29b8133a |
+----------+------------------------------------------------------------------------------------------+</pre>

## [6]	Access to the URL which was displayed by the command above. 

<p align="center"><img src="https://drive.google.com/uc?export=view&id=1Dds17F1PH-GNZRNPR1NX_celCWNsJJWy"></p>
