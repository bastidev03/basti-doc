# Introduction Docker ([Documentation officielle](https://docs.docker.com/))

Docker permet d'exécuter des sources/applications dans un environnement isolé (`Environnement docker`) de la machine qui l'exécute que l'on appelle `Conteneur`.

Chaque `conteneur` possède son propre système de fichiers, et toutes les dépendances/logiciels nécessaires pour exécuter le code source y sont installés.  
Ce système de fichiers suit la même arborescence que celui d'un `environnement Linux` sur lequel docker s'appuie pour construire sont écosystème.

**Cela permet d'avoir une base de code qui fonctionne toujours de la même façon quelquesoit la machine physique/OS sur lesquels elle est exécutée.**

**Docker est très bien adapté pour du développement CI/CD**

# Architecture Docker

![Image](./docker_architecture.jpeg)

La commande client **`docker`** (`Docker CLI`) permet d'interagir avec le `démon Docker` qui gère en temps réel tout les objets qui composent l'environnement Docker en cours d'exécution (`conteneurs`, `images`, `volumes`, ...).

# Concepts

## Image Docker

Une `Image Docker`, est un `binaire` qui contient l'état et les données d'un système de fichiers spécifique, ainsi que les valeurs de certaines configurations systèmes (linux) spécifiques, et également une liste de commandes à exécuter au moment de l'instanciation de l'image dans un `Conteneur Docker` afin de lancer certains `services` (Un serveur web par exemple).  

Cette `image` est composée de plusieurs `couches` qui représentent une suite de modifications à apporter à ce système de fichiers ou à l'environnement OS (linux) du futur `Conteneur Docker` dans lequel elle sera instanciée (Un peu comme une suite de commits dans un arbre `Git`).

Une `Image Docker` peut donc soit être créée de toutes pièces, soit être créée à partir d'une autre `Image Docker` (`Image perso` ou `Image officielle` téléchargée depuis [`Docker Hub`](https://hub.docker.com/)) à laquelle on ajoute nos propres couches personnelles.

>**!!! Important !!!**
> - Une `Image Docker` est **`immutable`** : Elle ne peut pas être modifiée par le `Conteneur` qui l'exécute
>
> - Si le `Conteneur`, durant son exécution, fait des modifications dans le système de fichier sur lequel il tourne, ces modifications seront supprimées dès que le `Conteneur` sera stoppé.  
>Lorsqu'il sera redémarré, le système de fichier se retrouvera dans le même état que celui qui se trouve dans l'image

## Conteneur Docker

Un `Conteneur Docker` est un environnement système (basé sur linux) isolé du système de la machine sur lequel il se trouve (Une sorte de machine virtuelle légère).  
En POO, on considérerait qu'un `Conteneur Docker` est une `instance` d'une `Image Docker` de laquelle il est issu.

Un `Conteneur Docker`, une fois instancié, peut être démarré ou arrêté de la même façon qu'un démon/service applicatif.

Un `Conteneur Docker` peut communiquer avec l'extérieur en ouvrant des `ports` sur la machine locale. C'est ce que l'on appelle un **`port exposé`**.

Un `Conteneur Docker` peut également communiquer avec d'autres `Conteneurs` qui tournent au sein du même `Environnement Docker`. Pour cela, les conteneurs peuvent ouvrir des `ports` sur le réseau local de l'`Environnement Docker` de la machine. Ces `ports` ne sont pas accessibles depuis la machine, c'est ce que l'on appelle des **`ports internes`**. 

## Volume Docker

TODO