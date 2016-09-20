# Netcat

## I. Introduction
Nmap est un formidable outil pour scanner les ports d'une machine, obtenir les services et leur version pour dénicher des failles potentielles. L'idée va être ici de forgé des paquets spécifique afin que le server réponde d'une façon précise (cf RFC 793), afin d'interpreter la réponse du server pour savoir si le port est ouvert ou fermé.

## II. Le SYNScan
On envoie un paquet TCP SYN au server (demande de connexion).
* Si le serveur répond SYN/ACK à 1, alors le port en question est ouvert
* Si le serveur répond RST/ACK à 1, alors aucun service actif sur ce port
* Si le serveur ne répond rien, alors le port (ouvert ou fermé) est protégé par un parfeu (filtered)
Même si le SS est rapide, cette technique laisse des trace lorsque le port est ouvert : connexion en 2 temps au lieu de 3. Pour une connexion en trois temps, utiliser le Scan connecT -sT.
```{r, engine='bash'}
aas@ubuntu:~$ nmap -sS 192.168.1.20
```

## III. Le FINScan
On envoi un paquet FIN au serveur
* Si le serveur ne répond rien, alors le port est ouvert ou filtré
* Si le serveu répond RST/ACK à 1, alors le port est fermé
Incapable de savoir si un port est filtered, et peu fiable.
```{r, engine='bash'}
aas@ubuntu:~$ nmap -sF 192.168.1.20
```

## IV. Le XMASScan
On envoi un paquet URG/PUSH/FIN à 1 au serveur
* Si le serveur ne répond rien, alors le port est ouvert ou filtré
* Si le serveu répond RST/ACK à 1, alors le port est fermé
Incapable de savoir si un port est filtered, pas discret car inhabituel, et peu fiable.
```{r, engine='bash'}
aas@ubuntu:~$ nmap -sX 192.168.1.20
```

## V. Le TCPNullScan
On envoi un paquet avec tous les flags à 0
* Si le serveur ne répond rien, alors le port est ouvert ou filtré
* Si le serveu répond RST/ACK à 1, alors le port est fermé
Incapable de savoir si un port est filtered, pas discret car inhabituel, et peu fiable.
```{r, engine='bash'}
aas@ubuntu:~$ nmap -sN 192.168.1.20
```

## VI. Le ACKScan
On envoi un paquet avec ACK à 1
* Si le serveur ne répond rien, alors le port est filtré
* Si le serveu répond RST à 1, alors le port est non filtré
Incapable de savoir si un port est filtered, pas discret car inhabituel, et peu fiable.
```{r, engine='bash'}
aas@ubuntu:~$ nmap -sA 192.168.1.20
```

## VII. L'IdleScan
L'idée est de faire scanner une machine par une machine zombie à notre place.
* Je trouve un zombie avec un port TCP ouvert
* Je lui envoi une demande de connexion avec un SYN
* Il me répond avec SYN+ACK et je lis dans l'en-tête de couche 3 son IPID (eg 100)
* J'envoi un SYN à la victime avec comme IP source celle du zombie

Je re-SYN mon zombie et là, deux solutions :
* Son IPID vaut 102 (+2) donc le port de la victime était ouvert
* Son IPID vaut 101 (+1) donc le port de la victime était fermé

```{r, engine='bash'}
aas@ubuntu:~$ nmap -P0 -sI ip_zombie:port_zombie ip_victime
```
P0 oblige nmap à ne pas faire de ping avant de scanner une machine. Sans cela, nous sommes grillés...  
L'idleScan est donc pas mal pour rester furtif ou essayer de bypasser un firewall à l'aide d'un zombie "autorisé".

## VIII. Découvrir les machines d'un réseau
On effectue un ScanPing
```{r, engine='bash'}
aas@ubuntu:~$ nmap -sP 192.168.0.0/24
```

## IX. Détection d'OS
```{r, engine='bash'}
aas@ubuntu:~$ nmap -O 192.168.1.20
```

## X. Pour l'attaquant
* Eviter le scan de masse et choisir stratégiquement des ports (ex. 22, 25, 80, 161, 162 etc)
* Preferer l'Idle Scan si possible
* Sinon utiliser -sT pour etre discret
* Pour tenter de contourner le firewall, utiliser les XMASScan + ACKScan

## XI. Pour le defenseur
* Utiliser des sondes IDS/IPS
* Le firewall doit fermer les ports inutiles et gérer les XMASScan, NULLScan, etc.
* Utiliser des ports différents que ceux communs (exemple : au lieu du port 22, utiliser 2938 pour SSH).
* Utiliser un service comme fail2ban
* Empecher le banner grabbing
