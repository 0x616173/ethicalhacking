# Iptables

## Introduction
Iptables est le firewall le plus utilisé sur Linux. Nous nous limiterons aux tables Filter et Nat.

Filter = INPUT, OUTPUT, FORWARD
Nat    = PREROUTING, INPUT, OUTPUT, POSTROUTING

La table Filter est utilisée quand un paquet est destiné au firewall (INPUT, OUTPUT) ou qu'il traverse le firewall (FORWARD).
La table Nat est utilisée pour rediriger un paquet.

L'ordre des règles est important. Par défaut, on drop tout, puis on ajoute nos règles permissives.

## Sauvegarde / Restauration
```{r, engine='bash'}
aas@ubuntu:~$ iptables-save > mes_regles.txt   # Sauvegarde les règles d'iptables
aas@ubuntu:~$ iptables-restore < mes_regles.txt   # Restaure les règles d'iptables
```
## Afficher les chaines
```{r, engine='bash'}
aas@ubuntu:~$ iptables -t filter  -Lv --line-numbers   # Liste les règles de la table filter (défaut)
```

## Flush
```{r, engine='bash'}
aas@ubuntu:~$ iptables -F   # Flush les tables
aas@ubuntu:~$ iptables -X   # Supprime les chaines personnalisées
```

## Politique par défaut
```{r, engine='bash'}
aas@ubuntu:~$ iptables -P OUTPUT DROP   # Politique par défaut sur Output de dropper tout
```

## Ajout d'une règle
```{r, engine='bash'}
aas@ubuntu:~$ iptables -I INPUT 1 -p tcp -i eth0 --dport 20:23 -j ACCEPT   # Ajout d'une règle
aas@ubuntu:~$ iptables -I INPUT 1 -p tcp -i eth0 -m multiport --dport 20,23,80 -j ACCEPT   # Ajout d'une règle
```

## Suppression d'une règle
```{r, engine='bash'}
aas@ubuntu:~$ iptables -D "INPUT" 3   # Suppression de la 3eme règle de la chaine INPUT de la table Filter (sous-entendu)
```

## Modification d'une règle
```{r, engine='bash'}
aas@ubuntu:~$ iptables -R INPUT 3 -i eth0 -p tcp --dport XX -j ACTION   # Modification de la 3ème règle de la chaine INPUT
```

## Redirection
Prérouting pour les flux entrant (car se passe avant le routage) : ipPublique -> ipPrivé
Postrouting pour les flux sortant (car se passe après le routage) : ipPrivé -> ipPublique

## Logs
La journalisation doit être placée avant la règle de filtrage associée
```{r, engine='bash'}
aas@ubuntu:~$ iptables -I INPUT -i eth0 -p tcp --dport 22 -j LOG --log-prefix "ConnexionDetecteeEnSSH: "   # Journalisation des connexions SSH
```