## 2. Setup

🌞 Dans le rendu, je veux
```
[brendan@dns ~]$ sudo cat /etc/named.conf
[sudo] password for brendan:
options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";

# référence vers notre fichier de zone
zone "tp6.b1" IN {
     type master;
     file "tp6.b1.db";
     allow-update { none; };
     allow-query {any; };
};
# référence vers notre fichier de zone inverse
zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp6.b1.rev";
     allow-update { none; };
     allow-query { any; };
};
```

```
[brendan@dns ~]$ sudo cat /var/named/tp6.b1.db
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns       IN A 10.6.1.101
john      IN A 10.6.1.11
```

```
[brendan@dns ~]$ sudo cat /var/named/tp6.b1.rev
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Reverse lookup
101 IN PTR dns.tp6.b1.
11 IN PTR john.tp6.b1.
```

```
[brendan@dns ~]$ systemctl status named
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; preset: >
     Active: active (running) since Fri 2023-11-17 16:21:41 CET; 10min ago
   Main PID: 11834 (named)
      Tasks: 5 (limit: 4672)
     Memory: 18.9M
        CPU: 33ms
     CGroup: /system.slice/named.service
             └─11834 /usr/sbin/named -u named -c /etc/named.conf

Nov 17 16:21:41 dns.tp6.b1 named[11834]: network unreachable resolving './DNS>
Nov 17 16:21:41 dns.tp6.b1 named[11834]: network unreachable resolving './NS/>
Nov 17 16:21:41 dns.tp6.b1 named[11834]: zone localhost.localdomain/IN: loade>
Nov 17 16:21:41 dns.tp6.b1 named[11834]: zone tp6.b1/IN: loaded serial 201906>
Nov 17 16:21:41 dns.tp6.b1 named[11834]: zone localhost/IN: loaded serial 0
Nov 17 16:21:41 dns.tp6.b1 named[11834]: all zones loaded
Nov 17 16:21:41 dns.tp6.b1 systemd[1]: Started Berkeley Internet Name Domain >
Nov 17 16:21:41 dns.tp6.b1 named[11834]: running
Nov 17 16:21:41 dns.tp6.b1 named[11834]: managed-keys-zone: Initializing auto>
Nov 17 16:21:41 dns.tp6.b1 named[11834]: resolver priming query complete
```

```
[brendan@dns ~]$ ss -t -l -n | grep "10.6.1.101"
LISTEN 0      10        10.6.1.101:53        0.0.0.0:*
```

🌞 Ouvrez le bon port dans le firewall
```
[brendan@dns ~]$ sudo firewall-cmd --add-port=53/tcp --permanent
[sudo] password for brendan:
success
```

## 3. Test

🌞 Sur la machine john.tp6.b1
```
[brendan@dns ~]$ sudo firewall-cmd --add-port=53/udp --permanent
[sudo] password for brendan:
success
[brendan@dns ~]$ sudo firewall-cmd --reload
success



[brendan@john ~]$ ping google.com
PING google.com (142.250.201.174) 56(84) bytes of data.
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=1 ttl=115 time=11.2 ms


[brendan@john ~]$ ping www.ynov.com
PING www.ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=1 ttl=55 time=19.6 ms


[brendan@john ~]$ ping john.tp6.b1
PING john.tp6.b1 (10.6.1.11) 56(84) bytes of data.
64 bytes from john.tp6.b1 (10.6.1.11): icmp_seq=1 ttl=64 time=0.060 ms


[brendan@john ~]$ ping dns.tp6.b1
PING dns.tp6.b1 (10.6.1.101) 56(84) bytes of data.
64 bytes from 10.6.1.101 (10.6.1.101): icmp_seq=1 ttl=64 time=0.283 ms
```

🌞 Sur votre PC

[Trame Wireshark](./TrameTP6.pcapng)

```
PS C:\WINDOWS\system32> Set-DNSClientServerAddress "Wi-Fi" -ServerAddresses ("10.6.1.101")


PS C:\Users\brend> nslookup john.tp6.b1
Serveur :   UnKnown
Address:  10.6.1.101

Nom :    john.tp6.b1
Address:  10.6.1.11
```

## III. Serveur DHCP

🌞 Installer un serveur DHCP
```
[brendan@dhcp ~]$ sudo dnf -y install dhcp-server


[brendan@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
option domain-name     "srv.world";
# specify DNS server's hostname or IP address
option domain-name-servers     10.6.1.101;
# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.6.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.6.1.13 10.6.1.37;
    # specify broadcast address
    option broadcast-address 10.6.1.255;
    # specify gateway
    option routers 10.6.1.254;
}
```

🌞 Test avec john.tp6.b1
```
[brendan@john ~]$ ip a

2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:7f:c4:52 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.13/24 brd 10.6.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 494sec preferred_lft 494sec
    inet6 fe80::a00:27ff:fe7f:c452/64 scope link
       valid_lft forever preferred_lft forever




[brendan@john ~]$ sudo cat /etc/resolv.conf
[sudo] password for brendan:
# Generated by NetworkManager
search srv.world tp6.b1
nameserver 10.6.1.101
```

🌞 Requête web avec john.tp6.b1

[Trame web](./tp6_web.pcapng)