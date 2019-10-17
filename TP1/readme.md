# Six-reseau-TP1

* 🌞 liste des cartes réseau avec leur nom, leur IP et leur adresse MAC
```
[hugo@centos ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c1:e1:cd brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86226sec preferred_lft 86226sec
    inet6 fe80::18f2:de7e:b56:a0e5/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:f4:d8:1d brd ff:ff:ff:ff:ff:ff
    inet 192.168.156.3/24 brd 192.168.156.255 scope global dynamic noprefixroute enp0s8
       valid_lft 1026sec preferred_lft 1026sec
    inet6 fe80::a00:27ff:fef4:d81d/64 scope link
       valid_lft forever preferred_lft forever
```
* 🌞 enp0s3 pour la connexion NAT qui est en DHCP 
```
[hugo@centos ~]$ sudo nmcli -f DHCP4 con show enp0s3
[sudo] password for hugo:
DHCP4.OPTION[1]:                        domain_name = auvence.co
DHCP4.OPTION[2]:                        domain_name_servers = 10.33.10.20 10.33.10.2 8.8.8.8 8.8.4.4
DHCP4.OPTION[3]:                        expiry = 1569594705
DHCP4.OPTION[4]:                        ip_address = 10.0.2.15
DHCP4.OPTION[5]:                        requested_broadcast_address = 1
DHCP4.OPTION[6]:                        requested_dhcp_server_identifier = 1
DHCP4.OPTION[7]:                        requested_domain_name = 1
DHCP4.OPTION[8]:                        requested_domain_name_servers = 1
DHCP4.OPTION[9]:                        requested_domain_search = 1
DHCP4.OPTION[10]:                       requested_host_name = 1
DHCP4.OPTION[11]:                       requested_interface_mtu = 1
DHCP4.OPTION[12]:                       requested_ms_classless_static_routes = 1
DHCP4.OPTION[13]:                       requested_nis_domain = 1
DHCP4.OPTION[14]:                       requested_nis_servers = 1
DHCP4.OPTION[15]:                       requested_ntp_servers = 1
DHCP4.OPTION[16]:                       requested_rfc3442_classless_static_routes = 1
DHCP4.OPTION[17]:                       requested_routers = 1
DHCP4.OPTION[18]:                       requested_static_routes = 1
DHCP4.OPTION[19]:                       requested_subnet_mask = 1
DHCP4.OPTION[20]:                       requested_time_offset = 1
DHCP4.OPTION[21]:                       requested_wpad = 1
DHCP4.OPTION[22]:                       routers = 10.0.2.2
DHCP4.OPTION[23]:                       subnet_mask = 255.255.255.0
```

* Enp0s8 carte réseaux privée pas en DHCP
```
[hugo@centos ~]$ sudo nmcli -f DHCP4 con show enp0s8
DHCP4.OPTION[1]:                        expiry = 1569509505
DHCP4.OPTION[2]:                        ip_address = 192.168.156.3
DHCP4.OPTION[3]:                        requested_broadcast_address = 1
DHCP4.OPTION[4]:                        requested_dhcp_server_identifier = 1
DHCP4.OPTION[5]:                        requested_domain_name = 1
DHCP4.OPTION[6]:                        requested_domain_name_servers = 1
DHCP4.OPTION[7]:                        requested_domain_search = 1
DHCP4.OPTION[8]:                        requested_host_name = 1
DHCP4.OPTION[9]:                        requested_interface_mtu = 1
DHCP4.OPTION[10]:                       requested_ms_classless_static_routes = 1
DHCP4.OPTION[11]:                       requested_nis_domain = 1
DHCP4.OPTION[12]:                       requested_nis_servers = 1
DHCP4.OPTION[13]:                       requested_ntp_servers = 1
DHCP4.OPTION[14]:                       requested_rfc3442_classless_static_routes = 1
DHCP4.OPTION[15]:                       requested_routers = 1
DHCP4.OPTION[16]:                       requested_static_routes = 1
DHCP4.OPTION[17]:                       requested_subnet_mask = 1
DHCP4.OPTION[18]:                       requested_time_offset = 1
DHCP4.OPTION[19]:                       requested_wpad = 1
DHCP4.OPTION[20]:                       subnet_mask = 255.255.255.0
```
* 🌞 La table de routage 
```
[hugo@centos ~]$ ip r
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.156.0/24 dev enp0s8 proto kernel scope link src 192.168.156.3 metric 101
```
La première ligne est la route par défault lorsqu'il n'y a aucune route vers l'adresse recherché  
cette route est vers le réseau 10.0.2.0/24, elle est utilisée pour une connexion externe, la passerelle de cette route est à l'IP 10.0.2.15 et cette IP est portée par (enp0s3).  
cette route est vers le réseau 192.168.156.0/24, elle est utilisée pour une connexion locale, la passerelle de cette route est à l'IP 
192.168.156.3 et cette IP est portée par (enp0s8).  


 table arp
```
[hugo@centos ~]$ arp
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.156.1            ether   0a:00:27:00:00:09   C                     enp0s8
_gateway                 ether   52:54:00:12:35:02   C                     enp0s3
```

* 🌞 la liste des ports en écoute
```
[hugo@centos ~]$ ss -tulpn
Netid      State        Recv-Q       Send-Q                     Local Address:Port              Peer Address:Port
udp        UNCONN       0            0                   192.168.156.3%enp0s8:68                     0.0.0.0:*
udp        UNCONN       0            0                       10.0.2.15%enp0s3:68                     0.0.0.0:*
tcp        LISTEN       0            128                              0.0.0.0:22                     0.0.0.0:*
tcp        LISTEN       0            128                                 [::]:22                        [::]:*
```
Les ports TCP sont connectés au ssh.
les UDP sont des ports en attente qui sont fait pour recevoir ou envoyer sans connexion établie et accepter.

* 🌞 la liste des DNS utilisés par la machine
```
[hugo@centos ~]$ dig www.reddit.com

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> www.reddit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 13176
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 13, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.reddit.com.                        IN      A

;; ANSWER SECTION:
www.reddit.com.         56      IN      CNAME   reddit.map.fastly.net.
reddit.map.fastly.net.  29      IN      A       151.101.193.140
reddit.map.fastly.net.  29      IN      A       151.101.129.140
reddit.map.fastly.net.  29      IN      A       151.101.1.140
reddit.map.fastly.net.  29      IN      A       151.101.65.140

;; Query time: 32 msec
;; SERVER: 10.33.10.20#53(10.33.10.20)
;; WHEN: Thu Sep 26 17:11:07 CEST 2019
;; MSG SIZE  rcvd: 935
```
```
[hugo@centos ~]$ cat /etc/resolv.conf
# Generated by NetworkManager
search auvence.co reseau
nameserver 10.33.10.20
nameserver 10.33.10.2
nameserver 8.8.8.8
# NOTE: the libc resolver may not support more than 3 nameservers.
# The nameservers listed below may not be recognized.
nameserver 8.8.4.4
```
* 🌞 état actuel du firewall


