## I. Exploration locale en solo


### 1. Affichage d'informations sur la pile TCP/IP locale


🌞 Affichez les infos des cartes réseau de votre PC
```
j'utilise la commande : $ ipconfig /all et ipconfig

Carte réseau sans fil Wi-Fi :

Adresse physique . . . . . . . . . . . : 38-7A-0E-C6-72-0D
Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.37

Carte Ethernet Ethernet 2 :

il n'y a pas d'adresse ip
Adresse physique . . . . . . . . . . . : 00-FF-71-93-73-6B
```
🌞 Affichez votre gateway
```
j'utilise la commande $ ipconfig
Passerelle par défaut. . . . . . . . . : 10.33.51.254
```
🌞 Déterminer la MAC de la passerelle
```
j'utilise la commande : $ arp -a
10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique
```
🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)
```
Il faut aller dans : panneau de configuration, reseau et internet, connexion reseau, wifi, detail

adresse IP : 10.33.48.37
adresse MAC : 38-7A-0E-C6-72-0D
passerelle : 10.33.51.254
```


### 2. Modifications des informations


🌞 Utilisez l'interface graphique de votre OS pour changer d'adresse IP :
```
Il faut aller dans : panneau de configuration, reseau et internet, connexion reseau, wifi, propriété

l'adresse est devenu 10.33.48.37
le masque sous reseaux est devenu 255.255.255.0
```
🌞 Il est possible que vous perdiez l'accès internet. Que ce soit le cas ou non, expliquez pourquoi c'est possible de perdre son accès internet en faisant cette opération.
```
Il est possible de perdre l'accès internet si on utilise la même adresse IP que quelqu'un d'autre.
```


## II. Exploration locale en duo


### 1. Prérequis

### 2. Câblage

### 3. Modification d'adresse IP


🌞 Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau
```
ip -> 10.10.10.20
masque -> 255.255.255.0
```
🌞 Vérifier à l'aide d'une commande que votre IP a bien été changée
```
commande = $ ipconfig /all
ipv4 = 10.10.10.20 (modifié)
auto-configuration ipv4 = 169.254.130.203 (de base)

masque sous réseau = 255.255.255.0 (modifié)
masque sous réseau = 255.255.0.0 (de base)
```

🌞 Vérifier que les deux machines se joignent

```
$ ping 10.10.10.24
Envoi d’une requête 'Ping'  10.10.10.24 avec 32 octets de données :
Réponse de 10.10.10.24 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.24 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.24 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.24 : octets=32 temps<1ms TTL=128
```
🌞 Déterminer l'adresse MAC de votre correspondant
```
commande = $ arp -a
 2c-f0-5d-66-be-f2
 ```


 ### 4. Petit chat privé


🌞 Sur le PC serveur avec par exemple l'IP 192.168.1.1
```
$ .\nc.exe -l -p 192.168.1.1
 Il ne se passe rien
 ```

🌞 Sur le PC client avec par exemple l'IP 192.168.1.2
 ```
$ nc.exe 10.10.10.24 8833
hello dear
Le pc client envoie un message au serveur
 ```

🌞 Visualiser la connexion en cours
 ```
$netstat -a -n -b | Select-String 8833

 TCP    10.10.10.20:8833       10.10.10.24:54261      ESTABLISHED
 ```

🌞 Pour aller un peu plus loin
 ```
$.\nc.exe -l -p 8833 -s 10.10.10.20
On peut se connecter en ethernet grace a l'IP mais plus en Wifi
 ```

### 5. Firewall
 ```
On a créé des ports en entrée et en sortie autorisant l'accès a une autre IP. J'ai enlevé mon WIFI et me suis connecté au WIFI grace au pc de ma camarade avec qui j'étais branché en ethernet.

j'ai testé que javais bien internet avec la commande suivante :
$ ping 1.1.1.1
Réponse de 1.1.1.1 : octets=32 temps=15 ms TTL=56
 ```

 ### 6. Utilisation d'un des deux comme gateway
  ```
X
 ```


## III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP

🌞Exploration du DHCP, depuis votre PC
 ```
(Cette partie n'a pas été faite à Ynov mais chez moi, par conséquent les résultat seront probablement différents)

$ ipconfig /all
Serveur DHCP : 192.168.1.254

Bail obtenu. . . . . . . . . . . . . . : lundi 16 octobre 2023 19:02:50
Bail expirant. . . . . . . . . . . . . : mardi 17 octobre 2023 07:02:40
 ```
### 2. DNS

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**
 ```
$ ipconfig /all | findstr "Serveurs DNS."
Serveurs DNS. . .  . . . . . . . . . . : 192.168.1.254
 ```

 🌞 Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main
```
$ nslookup google.com
142.250.179.110

$ nslookup ynov.com
104.26.10.233
104.26.11.233
172.67.74.226

Address:  192.168.1.254
On fait la requête au DHCP
```

## IV. Wireshark
```
X
```