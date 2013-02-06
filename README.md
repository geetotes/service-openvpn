service-openvpn
===============

A SystemV Init script for OpenVpn Client


Requirements
-------------

1. Your `client.conf` is stored in `/etc/openvpn/client.conf` (if it's not you can just change the script)
2. `client.conf` has the line `daemon` to tell openvpn to start in daemon mode

Install Instructions
--------------------

1. Make executable the openvpn script and set the ownership to root
2. Link the script into your `/etc/init.d` directory
3. Reload the scripts in `/etc/init.d`


````
chown root:root openvpn
chmod a+x openvpn
ln -s openvpn /etc/init.d/openvpn
systemctl daemon-reload
````


Call the Openvpn client as a service now with `service openvpn`


Known Bugs
----------

Calling `service openvpn stop` pukes a bunch of shit on the screen.

Sometimes you get in really big trouble with SELinux using this script. If that's the case:

1. Run restorecon
2. Set the default label to `openvpn_etc_t`
3. Run restorecon again

````
/sbin/restorecon -v /etc/openvpn/client.conf
semanage fcontext -a -t openvpn_etc_t '/etc/openvpn/client.conf
restorecon -v '/etc/openvpn/client.conf'
````

You may have to do more crazy SELinux stuff. Check `/var/log/messages`
