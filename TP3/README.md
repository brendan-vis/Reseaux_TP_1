## I.ARP

### 1. Echange ARP

🌞Générer des requêtes ARP
```
$ ping 10.3.1.11
$ ping 10.3.1.12
$ ip n s (pour les 2 machines)

MAC John = 08:00:27:8a:9b:81
MAC Marcel = 08:00:27:81:68:0d
```

### 2. Analyse de trames

🌞Analyse de trames

[capture Wireshark](./tp3_arp.pcap)
```
$ sudo tcpdump -i enp0s3  not port 22 -w toto.pcap
$ sudo ip neigh flush all
$ ping 10.3.1.11
```

## II. Routage

### 1. Mise en place du routage

🌞Ajouter les routes statiques nécessaires pour que john et marcel puissent se ping
```
route vers Marcel = $ ip route add 10.3.2.12 via 10.3.1.254
route vers John = $ ip route add 10.3.1.11 via 10.3.2.254
```

### 2. Analyse de trames
🌞Analyse des échanges ARP
```
ordre|   type trame  |  IP source  |   MAC source    |  IP destination   |  MAC destination
_____|_______________|_____________|________________ |___________________|_________________
     |  requete ARP  | 10.3.2.254  |    Broadcast    | 10.3.2.12         |  Marcel
  1  |               |             |08:00:27:C7:9F:AE|                   |00:00:00:00:00:00
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.12   |    Marcel       | 10.3.2.254        | Broadcast
  2  |  réponse ARP  |             |08:00:27:81:68:0d|                   |08:00:27:C7:9F:AE
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.12   |    Marcel       | 10.3.2.254        | Broadcast
  3  |  requete ARP  |             |08:00:27:81:68:0d|                   |00:00:00:00:00:00        
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.254  |    Broadcast    | 10.3.2.12         | Marcel
  4  |  réponse ARP  |             |08:00:27:C7:9F:AE|                   |08:00:27:81:68:0d
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.254  |    Broadcast    | 10.3.2.12         |  Marcel
  5  |     ping      |             |08:00:27:C7:9F:AE|                   |00:00:00:00:00:00
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.12   |    Marcel       | 10.3.2.254        | Broadcast
  6  |     pong      |             |08:00:27:81:68:0d|                   |08:00:27:C7:9F:AE
_____|_______________|_____________|_________________|___________________|_________________
```

### 3. Accès internet
🌞Donnez un accès internet à vos machines - config routeur
```
Sur virtualBox on ajoute une carte réseau NAT au routeur puis on tape ces commandes pour avoir accès à internet :
$ sudo firewall-cmd --add-masquerade --permanent
$ sudo firewall-cmd --reload

Je test la connexion avec : $ ping 8.8.8.8
```

🌞Donnez un accès internet à vos machines - config clients
```
Ajout du Gateway = 10.0.4.15
Puis $ ping 8.8.8.8 pour tester la connexion

Serveur DNS qui peut être utilisé
$ dig gitlab.com
```

🌞Analyse de trames

[capture Wireshark LAN 1](./tp3_routage_lan1.pcap)

[capture Wireshark LAN 2](./tp3_routage_lan2.pcap)
```
LAN 1 :


ordre|   type trame  |  IP source  |   MAC source    |  IP destination   |  MAC destination
_____|_______________|_____________|________________ |___________________|_________________
     |               | 10.3.1.11   |      John       | 10.3.1.254        |  Routeur
  1  |     ping      |             |08:00:27:8a:9b:81|                   |00:00:00:00:00:00
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.1.254  |    Routeur      | 10.3.1.11         | John
  2  |     pong      |             |08:00:27:4d:87:6c|                   |08:00:27:8a:9b:81
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.1.254  |    Routeur      | 10.3.1.11         | John
  3  |     ping      |             |08:00:27:4d:87:6c|                   |00:00:00:00:00:00        
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.1.11   |      John       | 10.3.1.254        | Routeur
  4  |     pong      |             |08:00:27:8a:9b:81|                   |08:00:27:4d:87:6c
_____|_______________|_____________|_________________|___________________|_________________


LAN 2 :


ordre|   type trame  |  IP source  |   MAC source    |  IP destination   |  MAC destination
_____|_______________|_____________|________________ |___________________|_________________
     |               | 10.3.2.12   |     Marcel      | 10.3.2.254        |  Routeur
  1  |     ping      |             |08:00:27:81:68:0d|                   |00:00:00:00:00:00
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.254  |    Routeur      | 10.3.2.12         | Marcel
  2  |     pong      |             |08:00:27:c7:9f:ae|                   |08:00:27:81:68:0d
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.254  |    Routeur      | 10.3.2.12         | Marcel
  3  |     ping      |             |08:00:27:c7:9f:ae|                   |00:00:00:00:00:00        
_____|_______________|_____________|_________________|___________________|_________________
     |               | 10.3.2.12   |     Marcel      | 10.3.2.254        | Routeur
  4  |     pong      |             |08:00:27:81:68:0d|                   |08:00:27:c7:9f:ae
_____|_______________|_____________|_________________|___________________|_________________
```