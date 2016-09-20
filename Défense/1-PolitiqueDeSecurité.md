# La politique de Sécurité

## Introduction
Ce document est une reprise des bases de la politique de sécurité traitées dans Ethical Hack - Apprendre l'attaque pour mieux se défendre - ENI

## Les mots de passe
Le MDP est un moyen d'authentification, il doit être
* fort
* changé régulièrement
* idéalement généré aléatoirement.

## La formation du personnel
Il faut faire comprendre au personnel son rôle dans le SI. Il faut le sensibiliser aux techniques d'ingenierie social ou à la simple sécurité du poste de travail. On n'écrit pas les MDP sur des post-its, on vérouille sa session (Bien faire comprendre à l'utilisateur que même s'il n'a rien à caché sur sa machine, sa machine donne accès au reste du réseau). Ils doivent comprendre les risques spécifiques à leur poste. Une comptable est exposée à l'attaque au président par exemple.

## A chacun son rôle
Bonne gestion des droits
* accès
* fichiers
* accès salle serveurs
* etc

## Chiffrer les échanges
Puisqu'il est possible de sniffer sur le réseau et récupérer des informations sensible telles que des identifiants ou le contenu de message, il est important d'utiliser le chiffrement. Par exemple, utiliser PGP lors d'envoi de mail est une bonne chose.

## Mises à jour
Afin de corriger de potentielles failles de sécurité, mettre continuellement à jour :
* les OS
* les programmes
* les firmwares des équipements  

Ne pas mettre à jour, c'est laisser les portes ouvertes

## Emprisonner les services
Il est possible de cloisonner les services à un environnement très limité à tel point que le service n'ait pas connaissance du reste du système. Le service est ainsi confiné à l'interieur d'un repertoire du systeme de fichier et ne peut s'en échapper. Ainsi, si un pirate exploite une vulnérabilité de ce service, il est lui aussi cloisonner et c'est donc un moindre mal. (Voir `chroot` et `jail`)

## Empêcher les scans et les attaques
* Firewall (Fermer les ports inutiles)
* Fail2ban (par ex placer ssh en 2222 et ban toutes les connexions en 22)
* Port Knocking (connexion autorisée si le bon schéma d'ouverture de ports a eu lieu)
* Bien configurer (pas de connexion ssh root, privilégier les connexion par échange de clés, etc)

## Ne garder que l'essentiel
Couper les services inutilisés pour limiter le nombre de vulnérabilités potententielles

## Surveillance passive
* Utiliser la journalisation externalisée.
* Utiliser rkhunter pour déceler rootkit, permissions anormales, fichiers cachés étranges, etc.
* Utiliser Lynis pour détecter les problèmes de configuration, les problèmes de version logiciel, noyaux, etc.
* Utiliser chkrootkit pour détecter les rootkit, vérifier les logs supprimés/modifiés de wtmp, utmp, lastlog, etc.

## Les tests d'intrusion
* Par le sysadmin lui même
* Par des spécialistes

## Pour aller plus loin
La sécurité d'un SI est celle de son maillon le plus faible. Le risque 0 n'existant pas, il faut donc envisager une politique de sauvegarde.
