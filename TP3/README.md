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

```

### 3. Accès internet
🌞Donnez un accès internet à vos machines - config routeur
```

```

🌞Donnez un accès internet à vos machines - config clients
```

```

🌞Analyse de trames
```

```