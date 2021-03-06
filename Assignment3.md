# Assignment 3

### Task 1

1.a 

```bash
sudo yum install firewalld
sudo systemctl enable firewalld

```

1.b 

```bash
firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.3.0/24' reject"
```

1.c

```bash
sudo firewall-cmd --zone=public --permanent --add-port=80/tcp
sudo firewall-cmd --zone=public --permanent --add-port=22/tcp
sudo firewall-cmd --zone=public --permanent --add-port=443/tcp
```

### Task 2

Host Only Network ID: 192.168.4.0/24

Bridged Network ID: 192.168.1.0/24

### On Bridged Host:

Enable IP forwarding:

Open the config file /etc/sysctl.conf

then insert the following line

```bash
net.ipv4.ip_forward = 1
```

To enable the changes made in sysctl.conf run  to load the new values from the file

```bash
sysctl -p
```

```bash
firewall-cmd —permanent —direct —add-rule ipv4 nat POSTROUTING O -o eth0 -j MASQUERADE
```

```bash
firewall-cmd —permanent —direct —add-rule ipv4 filter FORWARD O -i eth0 eth1 -j ACCEPT
```

```bash
firewall-cmd —permanent —direct —add-rule ipv4 filter FORWARD O -i eth1 eth0 -m state —state RELATED,ESTABLISHED -j ACCEPT
```

```bash

```

### Alternative:

(trying with DNAT and SNAT )

```bash
sudo iptables --append PREROUTING --table nat --protocol tcp --destination 192.168.4.12 --jump DNAT --to-destination 192.168.0.12
sudo iptables --append POSTROUTING --table nat --protocol tcp --destination 192.168.0.10  --jump SNAT --to-source 192.168.4.12
```

### On the network machines:

Change the default gateway to the server machine.

```bash
sudo route add default gw 192.168.1.12 eth0
```
