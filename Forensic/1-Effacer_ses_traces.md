# Effacer ses traces

## I. Introduction
Lors d'une attaque, il est primordiale d'effacer ses traces au niveau des logs afin de rester discret le plus longtemps possible.

## II. Linux
Les logs utils :

* /var/log/auth.log
* /var/log/messages
* /var/log/daemon.log
* /var/log/kern.log
* /var/log/apache2/acces.log

Pour supprimer nos traces du fichier de connexion Apache, nous ferons :

```{r, engine='bash'}
aas@ubuntu:~$ grep -Rv 'adresse_ip_de_connexion' /var/log/apache2/acces_log > fichier_log_sans_trace
aas@ubuntu:~$ mv fichier_log_sans_trace /var/log/apache2/access_log
```

Pour effacer les logs des fichiers binaires comme `utmp`, `wtmp` ou `lastlog`, il faut utiliser un log-wiper tels que `cloak`, `zap`ou `clear`.

## III. Windows

À venir