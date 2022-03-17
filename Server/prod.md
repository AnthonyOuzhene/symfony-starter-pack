# Mise en prod

## Notions de base

Un serveur reste en attente d'une demande d'un client
Un serveur reçoit donc des requêtes et reçoit des réponses

Serveur (machine = hardware)
- connecté et disponible

*2 types de serveurs :*

- Serveur web ( apache / php/ etc.)
- Serveur de BDD ( MariaDB / SQLServer / Oracle / etc.)
- Mail (gère l'envoi et réception d'email)
- Jeux 

*Différents types de serveur :*
- Physique : machine ou système explotation directement installé. (hôte)
- Virtuel : utilise les ressources d'une autre machien (VM).
- Mutualisé : en colocation sur une machine. 
- Dédié : on est le propriétaire du serveur. Place alloué uniquement pour nous

Pour éconnomiser les ressources hardware, on installe pas d'interface graphique (système de fenêtre). on utilisera uniquement la ligne de commande.

## Lancer le serveur de prod

1. Aller sur `https://kourou.oclock.io/ressources/vm-cloud/` et démarrer la VM

2. Vérifier la présence des logiciels suivants depuis le terminal :

`nom du logiciel --version` :

- git : `git --version`
- mysql : `mariadb--version`
- php
- apache : `apache2 -v`
- composer
- nano

## Installation du projet sur serveur prod

- git clone
- composer install (dans projet)
connexion à mysql : ``sudo mysql`
- créer un utilisateur en BDD
```sh
CREATE USER 'le nom du user a crer'@'localhost' IDENTIFIED BY 'le password à creer';
GRANT ALL PRIVILEGES ON *.* TO 'le nom du user crée'@'localhost'
```
- configurer la chaine de connexion à la BDD dans le .env.local

```sh
# Ecrire le database_URL avec le nouvel utilisateur et mot de passe
nano .env.local
```
- Creer la database
- aller sur 
- créer la structure BDD avec migrations
- créer un user ADMIN

INSERT INTO `user` (`email`, `roles`, `password`, `active`) VALUES ('admin@admin.com',	'[\"ROLE_ADMIN\"]',	'$2y$13$.PJiDK3kq2C4owW5RW6Z3ukzRc14TJZRPcMfXcCy9AyhhA9OMK3Li',	1);

- installer le apache-pack
```sh
`composer require symfony/apache-pack`
```
 - debuguer la connexion : 
    - mettre .env en prod i/o dev
    - faire un git pull
