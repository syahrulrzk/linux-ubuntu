# OpenStack Xena : Configure Horizon

Configure OpenStack Dashboard Service (Horizon).
It's possible to control OpenStack on Web GUI to set Dashboard.

# [1]	Install Horizon.
<pre>root@dlp ~(keystone)# apt -y install openstack-dashboard</pre>

# [2]	Configure Horizon.
<pre>
root@dlp ~(keystone)# vi /etc/openstack-dashboard/local_settings.py
# line 99 : change Memcache server
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
        'LOCATION': '10.0.0.30:11211',
    },
}

# line 113 : add
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
# line 126 : set Openstack Host
# line 127 : comment out and add a line to specify URL of Keystone Host
OPENSTACK_HOST = "10.0.0.30"
#OPENSTACK_KEYSTONE_URL = "http://%s/identity/v3" % OPENSTACK_HOST
OPENSTACK_KEYSTONE_URL = "http://10.0.0.30:5000/v3"
# line 131 : set your timezone
TIME_ZONE = "Asia/Tokyo"
# add to the end
OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = True
OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = 'Default'

root@dlp ~(keystone)# systemctl restart apache2
# this is the optional setting
# if you allow common users to access to instances details or console on the Dashboard web, set like follows
root@dlp ~(keystone)# vi /etc/nova/policy.json
# create new
# default is [rule:system_admin_api], so only admin users can access to instances details or console
{
  "os_compute_api:os-extended-server-attributes": "rule:admin_or_owner",
}

root@dlp ~(keystone)# chgrp nova /etc/nova/policy.json
root@dlp ~(keystone)# chmod 640 /etc/nova/policy.json
root@dlp ~(keystone)# systemctl restart nova-api</pre>

## [3]	Access to the URL below with any web browser.

⇒ http://(Dashboard server's hostname or IP address)/horizon/
After accessing, following screen is displayed, then you can login with a user in Keystone.
It's possible to use all features if you login with [admin] user when you set it on keystone bootstrap.
If you login with a common user, it's possible to use or manage own instances.

<p align="center"><img src="https://drive.google.com/uc?export=view&id=1lepuLmDoEdzml0bonu2GG5T6xtIa1FoT"></p>

## [4]	After login successfully, following screen is displayed (with common user). You can control Openstack on this Dashboard.
<p align="center"><img src="https://drive.google.com/uc?export=view&id=1OlFkmrf6O8B5f9946A0SkDlsUhduB9LT"></p>

## [5]	To confirm own instances, Click [Instances] on the left pane, then they are diplayed on the right.
If you did not configure Nova Policy like [2] above, common users can access to here only.
If you configured Nova Policy like [2] above, it's possible to confirm details of instance to click an instance name.
<p align="center"><img src="https://drive.google.com/uc?export=view&id=1_mJEiSd9UlonAb-CD5MyK1W5TIrChrKj"></p>

## [6]	The details of instance is displayed. To Click [Console] tab, it's possible to access to instance console.
<p align="center"><img src="https://drive.google.com/uc?export=view&id=1CIoypnhdAq5refONS2VE4ABwlk7pLzn4"></p>

## [7]	On instance console, it's possible to operate instance on Dashboard web.
<p align="center"><img src="https://drive.google.com/uc?export=view&id=1YsKbW6w5p3Fujn-CidRoGBpGjq9SkMwJ"></p>
