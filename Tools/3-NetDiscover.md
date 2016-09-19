# NetDiscover

## I. Introduction
NetDiscover est un petit outil permettant de connaitre les machines actives sur un réseau. Il se démarque de Nmap par sa passivité puisqu'il ne fait qu'intercepter les paquets ARP.

## II. Mode Passif
```{r, engine='bash'}
aas@ubuntu:~$ netdiscover -pr 192.168.0.0/24
```

## III. Mode Actif
```{r, engine='bash'}
aas@ubuntu:~$ netdiscover -r 192.168.0.0/24
```