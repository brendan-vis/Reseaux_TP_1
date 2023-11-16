## I. First steps

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP
```
Discord (UDP)
ip et port = 66.22.196.53      50002
port local = 49305

youtube (UDP)
ip et port = 91.68.245.77      443
port local = 56023

spotify (UDP)
ip et port = 35.186.224.18      443
port local = 62944

ynov bordeaux (TCP)
ip et port = 52.214.115.213      443
port local = 51070

twitter ou X (TCP)
ip et port = 199.232.56.159      443
port local = 50691
```

🌞 Demandez l'avis à votre OS


    discord :

PS C:\WINDOWS\system32> netstat -n -a -b | Select-String dis -Context 1,0


Proto       Adresse locale           Adresse distante        État

TCP    10.33.70.187:50735     162.159.136.234:443    ESTABLISHED

[Discord.exe]
TCP    127.0.0.1:6463         0.0.0.0:0              LISTENING

[Discord.exe]
TCP    127.0.0.1:49679        127.0.0.1:49680        ESTABLISHED

[NVDisplay.Container.exe]
TCP    127.0.0.1:49680        127.0.0.1:49679        ESTABLISHED

[NVDisplay.Container.exe]


[capture discord](./discord.pcapng)


    youtube :

PS C:\WINDOWS\system32> netstat -n -a -b | Select-String chrome -Context 1,0

TCP    10.33.70.187:50738     74.125.133.188:5228    ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51219     172.67.74.226:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51220     172.67.74.226:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51221     178.32.154.7:443       LAST_ACK

[chrome.exe]
TCP    10.33.70.187:51223     104.26.10.233:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51224     178.32.154.7:443       LAST_ACK

[chrome.exe]
TCP    10.33.70.187:51226     178.32.154.7:443       LAST_ACK

[chrome.exe]
TCP    10.33.70.187:51227     52.222.149.81:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51229     151.101.1.44:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51231     51.104.148.203:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51234     151.101.1.44:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51239     204.79.197.200:443     ESTABLISHED


[capture youtube](./youtube.pcapng)


    spotify :

PS C:\WINDOWS\system32> netstat -n -a -b | Select-String spo -Context 1,0

[Spotify.exe]
TCP    0.0.0.0:57621          0.0.0.0:0              LISTENING

[Spotify.exe]
TCP    10.33.70.187:51186     10.33.83.226:8009      SYN_SENT

[Spotify.exe]
TCP    10.33.70.187:51188     104.199.65.124:4070    ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51189     34.149.154.214:443     ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51190     95.100.252.17:443      ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51191     34.149.154.214:443     ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51192     142.250.201.161:443    ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51193     45.60.153.218:443      ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51194     172.217.20.194:443     ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51195     172.217.20.164:443     ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51196     216.239.36.178:443     ESTABLISHED

[Spotify.exe]
TCP    10.33.70.187:51198     216.58.214.65:443      ESTABLISHED


[capture spotify](./spotify.pcapng)


    ynov :

PS C:\WINDOWS\system32> netstat -n -a -b | Select-String chrome -Context 1,0

TCP    10.33.70.187:50738     74.125.133.188:5228    ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51197     172.217.18.195:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51219     172.67.74.226:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51220     172.67.74.226:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51221     178.32.154.7:443       CLOSE_WAIT

[chrome.exe]
TCP    10.33.70.187:51223     104.26.10.233:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51224     178.32.154.7:443       CLOSE_WAIT

[chrome.exe]
TCP    10.33.70.187:51226     178.32.154.7:443       CLOSE_WAIT

[chrome.exe]
TCP    10.33.70.187:51227     52.222.149.81:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51228     204.79.197.200:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51229     151.101.1.44:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51231     51.104.148.203:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51232     52.51.81.37:443        ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51233     54.72.40.201:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51234     151.101.1.44:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51236     44.206.70.113:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51237     44.206.70.113:443      ESTABLISHED


[capture ynov](./ynov.pcapng)


    twitter ou X :

PS C:\WINDOWS\system32> netstat -n -a -b | Select-String chrome -Context 1,0

TCP    10.33.70.187:50738     74.125.133.188:5228    ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51229     151.101.1.44:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51234     151.101.1.44:443       ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51271     142.250.201.163:443    ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51274     104.244.42.193:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51275     104.244.42.66:443      ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51276     104.244.43.131:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51277     152.199.21.141:443     ESTABLISHED

[chrome.exe]
TCP    10.33.70.187:51278     93.184.220.70:443      ESTABLISHED


[capture twitter](./twitter.pcapng)



## II. Setup Virtuel

### 1. SSH

🌞 Examinez le trafic dans Wireshark

[capture SSH](./trame_SSH.pcapng)


🌞 Demandez aux OS
```
[brendan@node1 ~]$ ss -t
State        Recv-Q        Send-Q               Local Address:Port               Peer Address:Port        Process
ESTAB        0             52                       10.5.1.11:ssh                   10.5.1.30:52817
```


### 2. Routage

🌞 Prouvez que
```

[brendan@node1 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=18.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=19.4 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=20.6 ms


[brendan@node1 ~]$ ping google.com
PING google.com (172.217.18.206) 56(84) bytes of data.
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=1 ttl=115 time=17.2 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=2 ttl=115 time=21.6 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=3 ttl=115 time=22.4 ms
64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=4 ttl=115 time=21.0 ms
64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=5 ttl=115 time=21.7 ms
```


### 3. Serveur Web

🌞 Installez le paquet nginx
```
[brendan@web ~]$ sudo dnf install nginx
```

🌞 Créer le site web
```
[brendan@web ~]$ cd /var
[brendan@web var]$ sudo mkdir www
[sudo] password for brendan:
[brendan@web var]$ cd www/
[brendan@web www]$ sudo mkdir site_web_nul
[brendan@web www]$ cd site_web_nul/
[brendan@web site_web_nul]$ sudo nano index.html
[brendan@web site_web_nul]$ cat index.html
<h1>MEOW</h1>
```

🌞 Donner les bonnes permissions
```
[brendan@web site_web_nul]$ sudo chown -R nginx:nginx /var/www/site_web_nul
```

🌞 Créer un fichier de configuration NGINX pour notre site web
```
[brendan@web ~]$ sudo nano /etc/nginx/conf.d/site_web_nul.conf


[brendan@web ~]$ cat /etc/nginx/conf.d/site_web_nul.conf

server {
  # le port sur lequel on veut écouter
  listen 80;

  # le nom de la page d'accueil si le client de la précise pas
  index index.html;

  # un nom pour notre serveur (pas vraiment utile ici, mais bonne pratique)
  server_name www.site_web_nul.b1;

  # le dossier qui contient notre site web
  root /var/www/site_web_nul;
}
```

🌞 Démarrer le serveur web !
```
[brendan@web ~]$ sudo systemctl start nginx


[brendan@web ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Mon 2023-11-13 10:00:59 CET; 12s ago
    Process: 1319 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 1320 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 1322 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 1323 (nginx)
      Tasks: 2 (limit: 4672)
     Memory: 3.2M
        CPU: 38ms
     CGroup: /system.slice/nginx.service
             ├─1323 "nginx: master process /usr/sbin/nginx"
             └─1324 "nginx: worker process"

Nov 13 10:00:59 web.tp5.b1 systemd[1]: Starting The nginx HTTP and reverse proxy server...
Nov 13 10:00:59 web.tp5.b1 nginx[1320]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Nov 13 10:00:59 web.tp5.b1 nginx[1320]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Nov 13 10:00:59 web.tp5.b1 systemd[1]: Started The nginx HTTP and reverse proxy server.
```

🌞 Ouvrir le port firewall
```
[brendan@web ~]$ sudo firewall-cmd --add-port=80/tcp --permanent
```

🌞 Visitez le serveur web !
```
[brendan@node1 ~]$ curl 10.5.1.12
<h1>MEOW</h1>
```

🌞 Visualiser le port en écoute
```
[brendan@web ~]$ ss

tcp      ESTAB    0          0                                 10.5.1.12:http             10.5.1.30:58316
```

🌞 Analyse trafic

[Analyse du trafic](./analyse_trafic.pcapng)
