
# Mini Projet Jenkins

Ce Projet dénommé Mini projet jenkins a été réalisé durant ma formation au cours du bootcamp DEVOPS numéro 15 de **EAZYTraining avec Dirane Tafen et Ulrich Monji** qui s'est deroulé de Septembre à Novembre 2023.


## Objectifs
Il s’agit ici de mettre en œuvre la chaîne d’intégration continue sur l'image en dessous à l’aide jenkins pour un site web static dont le code source est [ici](https://github.com/diranetafen/static-website-example) .

![](pipeline.png)
 
## À faire
Pour la réalisation du project j'ai procédé comme suit:
- 1 Provisionner 3 VPS 
- 2 installer docker sur chaque vps à partir de la documentation officielle qui se trouve [ici](https://docs.docker.com/engine/install/debian/) 
- 3 Installer jenkins sur le vps 
- 4 Produire le Dockerfile pour le Build
- 5 Mettre en place le Pipeline



## Plan de Travail
J'ai suivi les étapes sur ci-dessous pour la réalisation du projet.

J’ai choisi 03 VPS de type Lightsail chez *AWS* *(01 pour Jenkins, 01 pour staging et 01 pour la Prod)*

![](vps.png)
Sur chaque VPS j’ai installé  Debian 11.4, docker et docker-compose
Sur le VPS de jenkins :
  j’ai installé jenkins à l’aide de docker-compose. Dans ma configuration le conteneur n’est accessible que sur la machine hôte à partir de  **127.0.0.1:8080**
[](docker-ps.png)
Afin d’accéder à jenkins en dehors de l’hôte , j’ai installé Apache2 et fais un reverse proxy sur le **127.0.0.1:8080**

[](apache.png)

et à la fin configuré letsencrypt avec un sous-domaine.
[](jenkins-url.jpg)