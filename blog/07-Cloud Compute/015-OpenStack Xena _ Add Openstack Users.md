# OpenStack Xena : Add Openstack Users

Add User accounts in keystone who can use Openstack System.

## [1]	Any names are OK you like for user-name or project-name.
Also Add flavors which define vCPU or Memory of an instance, too.
<pre>
# create a project
root@dlp ~(keystone)# openstack project create --domain default --description "Hiroshima Project" hiroshima
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | Hiroshima Project                |
| domain_id   | default                          |
| enabled     | True                             |
| id          | 9da4979990a24fe1a7a41c9d93b87383 |
| is_domain   | False                            |
| name        | hiroshima                        |
| options     | {}                               |
| parent_id   | default                          |
| tags        | []                               |
+-------------+----------------------------------+

# create a user
root@dlp ~(keystone)# openstack user create --domain default --project hiroshima --password userpassword serverworld
+---------------------+----------------------------------+
| Field               | Value                            |
+---------------------+----------------------------------+
| default_project_id  | 9da4979990a24fe1a7a41c9d93b87383 |
| domain_id           | default                          |
| enabled             | True                             |
| id                  | 9bd544cf5ed14a1ab2b1e3f90bc0b801 |
| name                | serverworld                      |
| options             | {}                               |
| password_expires_at | None                             |
+---------------------+----------------------------------+

# create a role
root@dlp ~(keystone)# openstack role create CloudUser
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | None                             |
| domain_id   | None                             |
| id          | 42c383944cc44dd289db3d808ae62eaa |
| name        | CloudUser                        |
| options     | {}                               |
+-------------+----------------------------------+

# create a user to the role
root@dlp ~(keystone)# openstack role add --project hiroshima --user serverworld CloudUser
# create a [flavor]
root@dlp ~(keystone)# openstack flavor create --id 0 --vcpus 1 --ram 2048 --disk 10 m1.small
+----------------------------+----------+
| Field                      | Value    |
+----------------------------+----------+
| OS-FLV-DISABLED:disabled   | False    |
| OS-FLV-EXT-DATA:ephemeral  | 0        |
| description                | None     |
| disk                       | 10       |
| id                         | 0        |
| name                       | m1.small |
| os-flavor-access:is_public | True     |
| properties                 |          |
| ram                        | 2048     |
| rxtx_factor                | 1.0      |
| swap                       |          |
| vcpus                      | 1        |
+----------------------------+----------+</pre>
