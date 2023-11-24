## 1. Fingerprint

🌞 Effectuez une connexion SSH en vérifiant le fingerprint

```
PS C:\Users\brend> ssh brendan@10.7.1.11
The authenticity of host '10.7.1.11 (10.7.1.11)' can't be established.
ED25519 key fingerprint is SHA256:u24zpi6nROeSu8kLVJT79WUYGVAYrEcpXrxdP7FDA3s.
This host key is known by the following other names/addresses:
    C:\Users\brend/.ssh/known_hosts:1: 10.3.1.11
    C:\Users\brend/.ssh/known_hosts:4: 10.3.1.12
    C:\Users\brend/.ssh/known_hosts:5: 10.3.2.12
    C:\Users\brend/.ssh/known_hosts:6: 10.3.1.254
    C:\Users\brend/.ssh/known_hosts:7: 10.3.2.254
    C:\Users\brend/.ssh/known_hosts:8: 10.4.1.253
    C:\Users\brend/.ssh/known_hosts:9: 10.4.1.254
    C:\Users\brend/.ssh/known_hosts:10: 10.4.1.137
    (11 additional names omitted)
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.7.1.11' (ED25519) to the list of known hosts.
brendan@10.7.1.11's password:
Last login: Thu Nov 23 13:54:25 2023 from 10.7.1.30
[brendan@john ~]$



[brendan@john ~]$ sudo ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_key
[sudo] password for brendan:
256 SHA256:u24zpi6nROeSu8kLVJT79WUYGVAYrEcpXrxdP7FDA3s /etc/ssh/ssh_host_ed25519_key.pub (ED25519)
```

## 2. Conf serveur SSH

🌞 Consulter l'état actuel

```
[brendan@router ~]$ sudo ss -lnptu
Netid State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port Process

tcp   LISTEN  0       128            0.0.0.0:22          0.0.0.0:*     users:(("sshd",pid=696,fd=3))
```

🌞 Modifier la configuration du serveur SSH

```
[brendan@router ~]$ sudo nano /etc/ssh/sshd_config

Port 22000
ListenAddress 10.7.1.254
```

🌞 Prouvez que le changement a pris effet

```
[brendan@router ~]$ sudo ss -lnptu

Netid State  Recv-Q  Send-Q   Local Address:Port    Peer Address:Port Process
udp   UNCONN 0       0            127.0.0.1:323          0.0.0.0:*     users:(("chronyd",pid=680,fd=5))
udp   UNCONN 0       0                [::1]:323             [::]:*     users:(("chronyd",pid=680,fd=6))
tcp   LISTEN 0       128         10.7.1.254:22000        0.0.0.0:*

```

🌞 Effectuer une connexion SSH sur le nouveau port

```
sudo firewall-cmd --add-port=22000/tcp --permanent
sudo firewall-cmd --reload


PS C:\Users\brend> ssh brendan@router.tp7.b1 -p 22000
The authenticity of host '[router.tp7.b1]:22000 ([10.7.1.254]:22000)' can't be established.
ED25519 key fingerprint is SHA256:u24zpi6nROeSu8kLVJT79WUYGVAYrEcpXrxdP7FDA3s.
This host key is known by the following other names/addresses:
    C:\Users\brend/.ssh/known_hosts:1: 10.3.1.11
    C:\Users\brend/.ssh/known_hosts:4: 10.3.1.12
    C:\Users\brend/.ssh/known_hosts:5: 10.3.2.12
    C:\Users\brend/.ssh/known_hosts:6: 10.3.1.254
    C:\Users\brend/.ssh/known_hosts:7: 10.3.2.254
    C:\Users\brend/.ssh/known_hosts:8: 10.4.1.253
    C:\Users\brend/.ssh/known_hosts:9: 10.4.1.254
    C:\Users\brend/.ssh/known_hosts:10: 10.4.1.137
    (12 additional names omitted)
Are you sure you want to continue connecting (yes/no/[fingerprint])?yyes
Warning: Permanently added '[router.tp7.b1]:22000' (ED25519) to the list of known hosts.
Last login: Thu Nov 23 14:28:27 2023 from 10.7.1.30
```


## 3. Connexion par clé

### B. Manips

🌞 Générer une paire de clés

```
$ ssh-keygen -t rsa -b 4096
```

🌞 Déposer la clé publique sur une VM

```
brend@HP-Brendan MINGW64 ~
$ ssh-copy-id brendan@john.tp7.b1
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/c/Users/brend/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed

/usr/bin/ssh-copy-id: ERROR: ssh: Could not resolve hostname john.tp7.b1: Name or service not known
```

🌞 Connectez-vous en SSH à la machine

```
PS C:\Users\brend> ssh brendan@john.tp7.b1
The authenticity of host 'john.tp7.b1 (10.7.1.11)' can't be established.
ED25519 key fingerprint is SHA256:u24zpi6nROeSu8kLVJT79WUYGVAYrEcpXrxdP7FDA3s.
This host key is known by the following other names/addresses:
    C:\Users\brend/.ssh/known_hosts:1: 10.3.1.11
    C:\Users\brend/.ssh/known_hosts:4: 10.3.1.12
    C:\Users\brend/.ssh/known_hosts:5: 10.3.2.12
    C:\Users\brend/.ssh/known_hosts:6: 10.3.1.254
    C:\Users\brend/.ssh/known_hosts:7: 10.3.2.254
    C:\Users\brend/.ssh/known_hosts:8: 10.4.1.253
    C:\Users\brend/.ssh/known_hosts:9: 10.4.1.254
    C:\Users\brend/.ssh/known_hosts:10: 10.4.1.137
    (14 additional names omitted)
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'john.tp7.b1' (ED25519) to the list of known hosts.
Last login: Thu Nov 23 16:31:02 2023 from 10.7.1.30
[brendan@john ~]$
```


### C. Changement de fingerprint

🌞 Supprimer les clés sur la machine router.tp7.b1


```
[brendan@router ssh]$ sudo rm ssh_host_*
```

🌞 Regénérez les clés sur la machine router.tp7.b1

```
[brendan@router /]$ sudo ssh-keygen -A
```

🌞 Tentez une nouvelle connexion au serveur

```
PS C:\Users\brend> ssh brendan@10.7.1.254 -p 22000
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:DIJ4qikhDkKi6UAm7diTRMIrwqudBxBT/u43VhMfjWs.
Please contact your system administrator.
Add correct host key in C:\\Users\\brend/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in C:\\Users\\brend/.ssh/known_hosts:28
Host key for [10.7.1.254]:22000 has changed and you have requested strict checking.
Host key verification failed.




PS C:\Users\brend> ssh brendan@10.7.1.254 -p 22000
The authenticity of host '[10.7.1.254]:22000 ([10.7.1.254]:22000)' can't be established.
ED25519 key fingerprint is SHA256:DIJ4qikhDkKi6UAm7diTRMIrwqudBxBT/u43VhMfjWs.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?yyes
Warning: Permanently added '[10.7.1.254]:22000' (ED25519) to the list of known hosts.
Last login: Thu Nov 23 16:43:44 2023 from 10.7.1.30
[brendan@router ~]$
```

## III. Web sécurisé

🌞 Montrer sur quel port est disponible le serveur web

```
[brendan@web ~]$ sudo ss -ltunp
Netid  State   Recv-Q  Send-Q     Local Address:Port     Peer Address:Port  Process

tcp    LISTEN  0       511              0.0.0.0:80            0.0.0.0:*      users:(("nginx",pid=1744,fd=6),("nginx",pid=1743,fd=6))

tcp    LISTEN  0       511                 [::]:80               [::]:*      users:(("nginx",pid=1744,fd=7),("nginx",pid=1743,fd=7))



[brendan@john ~]$ curl 10.7.1.12
MEOW
```


🌞 Générer une clé et un certificat sur web.tp7.b1

```
[brendan@web ~]$ openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout server.key -out server.crt
[brendan@web ~]$ sudo mv server.key /etc/pki/tls/private/web.tp7.b1.key
[brendan@web ~]$ sudo mv server.crt /etc/pki/tls/certs/web.tp7.b1.crt
[brendan@web ~]$ sudo chown nginx:nginx /etc/pki/tls/private/web.tp7.b1.key
[brendan@web ~]$ sudo chown nginx:nginx /etc/pki/tls/certs/web.tp7.b1.crt
[brendan@web ~]$ sudo chmod 0400 /etc/pki/tls/private/web.tp7.b1.key
[brendan@web ~]$ sudo chmod 0444 /etc/pki/tls/certs/web.tp7.b1.crt
```


🌞 Conf firewall

```
[brendan@web ~]$ sudo firewall-cmd --add-port=80/tcp --permanent
```


🌞 Redémarrez NGINX

```
sudo systemctl restart nginx
```


🌞 Prouvez que NGINX écoute sur le port 443/tcp

```
[brendan@web ~]$ sudo ss -ltunp
```

🌞 Visitez le site web en https

```
[brendan@john ~]$ curl -k https://10.7.1.12
MEOW
```