# OpenStack Xena : Add VM images to Glance2021/10/07
 	
Add Virtual Machine images to Glance.
## [1]	For example, Create a Virtual Machine image of ubuntu 20.04.

<pre>
# create a directory for disk image
root@dlp ~(keystone)# mkdir -p /var/kvm/images
# download the official disk image
root@dlp ~(keystone)# wget http://cloud-images.ubuntu.com/releases/20.04/release/ubuntu-20.04-server-cloudimg-amd64.img -P /var/kvm/images
# if you'd like to change settings in the image, configure like follows
# mount disk image
root@dlp ~(keystone)# modprobe nbd
root@dlp ~(keystone)# qemu-nbd --connect=/dev/nbd0 /var/kvm/images/ubuntu-20.04-server-cloudimg-amd64.img
root@dlp ~(keystone)# mount /dev/nbd0p1 /mnt
root@dlp ~(keystone)# vi /mnt/etc/cloud/cloud.cfg
# line 13 : add
# only the case you'd like to allow SSH password authentication
ssh_pwauth: true
# line 96: change
# only the case if you'd like to allow [ubuntu] user to use SSH password auth
system_info:
   # This will affect which distro class gets used
   distro: ubuntu
   # Default user name + that default users groups (if added/used)
   default_user:
     name: ubuntu
     lock_passwd: False
     gecos: Ubuntu

root@dlp ~(keystone)# umount /mnt
root@dlp ~(keystone)# qemu-nbd --disconnect /dev/nbd0p1
/dev/nbd0p1 disconnected
# add image to Glance
root@dlp ~(keystone)# openstack image create "Ubuntu2004" --file /var/kvm/images/ubuntu-20.04-server-cloudimg-amd64.img --disk-format qcow2 --container-format bare --public
+------------------+-----------------------------------------------------------------------------+
| Field            | Value                                                                       |
+------------------+-----------------------------------------------------------------------------+
| container_format | bare                                                                        |
| created_at       | 2021-10-07T00:13:48Z                                                        |
| disk_format      | qcow2                                                                       |
| file             | /v2/images/ba8781c7-85ef-4872-957e-2c27b55275b2/file                        |
| id               | ba8781c7-85ef-4872-957e-2c27b55275b2                                        |
| min_disk         | 0                                                                           |
| min_ram          | 0                                                                           |
| name             | Ubuntu2004                                                                  |
| owner            | f4431102f2d9415590e3ac11c616858a                                            |
| properties       | os_hidden='False', owner_specified.openstack.md5='', owner_specified.o..... |
| protected        | False                                                                       |
| schema           | /v2/schemas/image                                                           |
| status           | queued                                                                      |
| tags             |                                                                             |
| updated_at       | 2021-10-07T00:13:48Z                                                        |
| visibility       | public                                                                      |
+------------------+-----------------------------------------------------------------------------+

root@dlp ~(keystone)# openstack image list
+--------------------------------------+------------+--------+
| ID                                   | Name       | Status |
+--------------------------------------+------------+--------+
| ba8781c7-85ef-4872-957e-2c27b55275b2 | Ubuntu2004 | active |
+--------------------------------------+------------+--------+</pre>
