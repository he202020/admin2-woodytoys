# admin2-woodytoys

Voici le repository des TPs de Admin 2.

On retrouve ici la mise en place d'un réseau pour une entreprise fictive woody-toys.

## Infos utiles

Le site web est accessible sur 
- www.net.woody-toys.world
- blog.net.woody-toys.world
- www.net.woody-toys.world/products.php

Tout est redirigé en HTTPS

Le serveur web est sur
- mail.net.woody-toys.world

Le serveur DNS est sur 
- ns1.net.woody-toys.world

Il n'y a qu'un seul vps en fonction dans ce réseau avec différents services.

1. Un DNS autoritaire de la zone net.woody-toys.world
2. Un serveur Web
3. Un serveur Mail

## Structure du projet

Les 2 services sont répartis dans 3 dossiers différents:
- *dns-primary* contient les fichiers de zone, la config de bind9 et le fichier compose
- *web-services* contient les 3 services différents qui font tourner le serveur web et le fichier compose
  - Le service nginx
  - Le service php
  - Le service mariadb
- *mail* contient le fichier compose et les variables d'environnement pour le service mail

### Configuration DNS

1. Se placer dans le dossier *dns-primary*
2. lancer la commande *docker compose up -d*
3. Le service DNS se lancera

### Configuration web

1. Se placer dans le dossier *web* et créer un dossier appeler *certificate*
2. Il faut maintenant créer les certificats avec openssl (la liste des commandes se trouve dans le fichier *openssl-comand.txt)
3. Avant d'initialiser le service web il faut aller modifier le fichier *nginx.conf* (il sera modifé par après avec certbot)

Les directives doivent être modifiéés comme suit pour les 2 blocks serveurs:

*ssl_certificate /etc/ssl/private/nginx-selfsigned.crt*
*ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key*

Maintenant le tout peut être lancé avec le fichier compose et la commande *docker compose up -d*

Il reste cepandant une dernière chose à faire, c'est de faire certifier le site avec certbot.

Pour ce faire on onvoie cette commande *docker exec web certbot --nginx --agree-tos --email minilepin@gmail.com -n -d www.net.woody-toys.world -d blog.net.woody-toys.world* qui va permettre d'envoyer au conteneur docker la commande certbot pour certifier le site web en HTTPS

### Configuration mail

1. Se placer dans le dossier *mail*
2. lancer la commande *docker compose up -d*
3. Le service mail se lancera

Pour ajouter des user on peut utiliser cette commande:

*docker exec -ti \<CONTAINER NAME\> setup email add user@example.com apassword*



