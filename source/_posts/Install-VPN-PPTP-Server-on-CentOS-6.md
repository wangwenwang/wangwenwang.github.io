---
title: Install VPN PPTP Server on CentOS 6
date: 2016-06-10 00:53:49
tags:
- PPTP
- VPN
- CentOS
---

Install ppp via yum:

```
$ yum install ppp -y

```
Download and We need to allinstall pptpd (the daemon for point-to-point tunneling). You can find the correct package at this website

```
$ cd /usr/local/src
$ wget http://poptop.sourceforge.net/yum/stable/packages/pptpd-1.3.4-2.el6.x86_64.rpm
$ rpm -Uhv pptpd-1.3.4-2.el6.x86_64.rpm

```

Once installed, open /etc/pptpd.conf using text editor and add following line:

```
localip 209.85.227.26
remoteip 209.85.227.27-30

```

Open /etc/ppp/options.pptpd and add  authenticate method, encryption and DNS resolver value:

```
require-mschap-v2
require-mppe-128
ms-dns 8.8.8.8

```

Lets create user to access the VPN server. Open /etc/ppp/chap-secrets and add the user as below:

```
vpnuser pptpd pwd *

```

We need to allow IP packet forwarding for this server. Open /etc/sysctl.conf via text editor and change line below:

```
net.ipv4.ip_forward = 1

```

Run following command to take effect on the changes:

```
$ sysctl -p

```

Allow IP masquerading in IPtables by executing following line:

```
$ iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
$ service iptables save
$ service iptables restart
```

Update: Once you have done with step 8, check the rules at /etc/sysconfig/iptables. Make sure that the POSTROUTING rules is above any REJECT rules.

Turn on the pptpd service at startup and reboot the server:

```
$ chkconfig pptpd on
$ init 6

```

Once the server is online after reboot, you should now able to access the PPTP server from the VPN client. You can monitor /var/log/messages for ppp and pptpd related log. Cheers!
