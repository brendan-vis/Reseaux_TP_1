## I. Setup IP

🌞 Mettez en place une configuration réseau fonctionnelle entre les deux machines
```
Nous avons choisi les IP suivantes :
mon ip =10.10.10.21
ip camarade = 10.10.10.22

l'adresse réseau 
10.10.10.20

L'adresse de broadcast
10.10.10.23
```

🌞 Prouvez que la connexion est fonctionnelle entre les deux machines
```
$ ping 10.10.10.22
Réponse de 10.10.10.22 : octets=32 temps=2 ms TTL=128
```

🌞 Wireshark it

[capture Wireshark](./wireshark_ex_1.pcapng)
```
ping = ICMP type 8
pong = ICMP type 0
```

## II. ARP my bro

🌞 Check the ARP table
```
$ arp -a
MAC du binome = 2c-f0-5d-66-be-f2
MAC de la gateway = 7c-5a-1c-d3-d8-76
```

🌞 Manipuler la table ARP
```
$ netsh interface IP delete arpcache
On peut voir les adresses MAC des ports de notre PC.
Quand on ping notre binome, d'autre adresse MAC s'affiche
```

🌞 Wireshark it

[capture 2 Wireshark](./wireshark_TP2.pcapng)
```
(1)adresse source : 10.10.10.21       (2)adresse destination : 10.10.10.21
adresse source : 2c-f0-5d-66-be-f2       adresse destination : 10.10.10.21

L'adresse (1) va envoyer une requête au routeur pour trouver à qui appartient l'adresse 2. Une fois trouvé, le proprietaire de l'adresse (2) renvoi son adresse MAC à l'adresse (1).
```

## III. DHCP

🌞 Wireshark it
```
X
```