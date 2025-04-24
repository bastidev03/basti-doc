# Introduction Docker ([Documentation officielle](https://docs.docker.com/))

Docker permet d'exécuter des sources/applications dans un environnement isolé (`Environnement docker`) de la machine qui l'exécute que l'on appelle `Conteneur`.

Chaque `conteneur` possède son propre système de fichiers, et toutes les dépendances/logiciels nécessaires pour exécuter le code source y sont installés.  
Ce système de fichiers suit la même arborescence que celui d'un `environnement Linux` sur lequel docker s'appuie pour construire sont écosystème.

**Cela permet d'avoir une base de code qui fonctionne toujours de la même façon quelquesoit la machine physique/OS sur lesquels elle est exécutée.**

**Docker est très bien adapté pour du développement CI/CD**

# Architecture Docker

![Image](./docker_architecture.jpeg)

La commande client **`docker`** (`Docker CLI`) permet d'interagir avec le `démon Docker` qui gère en temps réel tous les objets qui composent l'environnement Docker en cours d'exécution (`conteneurs`, `images`, `volumes`, ...).

# Concepts

## Image Docker

Une `Image Docker`, est un `binaire` qui contient l'état et les données d'un système de fichiers spécifique, ainsi que les valeurs de certaines configurations systèmes (linux) spécifiques qui seront instanciées au moment de l'instanciation de l'image dans un `Conteneur Docker` (`docker create` ou `docker run`).  

L'`Image Docker` contient également une liste de commandes/fichiers à exécuter afin de lancer certains `services` (Un serveur web par exemple) au moment où le `Conteneur Docker` sera lancé (`docker start` ou `docker run`).  

Cette `Image` est composée d'un empilement de `couches` qui représentent une suite de modifications à apporter à ce système de fichiers ou à l'environnement OS (linux) du futur `Conteneur Docker` dans lequel elle sera instanciée (Un peu comme une suite de commits dans un arbre `Git`).

Toutes les `Images Docker` ont pour couche de base, une couche que l'on appelle la **`couche scratch`** qui représente un système de fichiers vide.

Une `Image Docker` peut ainsi être créée, soit à partir de cette `couche scratch` (via un `Dockerfile`), soit à partir d'une autre `Image Docker` (`Image perso` ou `Image officielle` téléchargée depuis [`Docker Hub`](https://hub.docker.com/)), dans laquelle seront ajoutées nos propres `couches` personnelles (Cas le plus commun).

>**!!! Important !!!**
> - Une `Image Docker` est **`immutable`** : Elle ne peut pas être modifiée par le `Conteneur` qui l'exécute
>
> - Si le `Conteneur`, durant son exécution, fait des modifications dans le système de fichier sur lequel il tourne, ces modifications seront supprimées dès que le `Conteneur` sera stoppé.  
>Lorsqu'il sera redémarré, le système de fichier se retrouvera dans le même état que celui qui se trouve dans l'image
>
>- Pour sauvegarder des données, il faut utiliser des `volumes`

## Conteneur Docker

Un `Conteneur Docker` est un environnement système (basé sur linux) isolé du système de la machine sur lequel il se trouve (Une sorte de machine virtuelle légère).  
En POO, on considérerait qu'un `Conteneur Docker` est une `instance` d'une `Image Docker` de laquelle il est issu.

Un `Conteneur Docker`, une fois instancié, peut être démarré ou arrêté de la même façon qu'un démon/service applicatif (`Docker start`/`Docker stop`)

Un `Conteneur Docker` peut communiquer avec l'extérieur en ouvrant des `ports` sur la machine locale. C'est ce que l'on appelle un **`port exposé`**.

Un `Conteneur Docker` peut également communiquer avec d'autres `Conteneurs` qui tournent au sein du même `Environnement Docker`. Pour cela, les conteneurs peuvent ouvrir des `ports` sur le réseau local de l'`Environnement Docker` de la machine. Ces `ports` ne sont pas accessibles depuis la machine, c'est ce que l'on appelle des **`ports internes`**. 

## Volume Docker

Un `Volume Docker` est un disque virtuel qui permet de faire persister des données d'un ou plusieurs `conteneurs`.

Il s'agit d'un "pont" entre, un `dossier physique` de la machine dans lequel les données du `volume` sont réellement stockées et, un `dossier interne` du `conteneur`, dans lequel les données du `volume` peuvent être manipulées par le `conteneur`.  

Un même `Volume Docker` peut être partagé entre plusieurs conteneurs.

>**!!! Important !!!**
>
>- L'association d'un `Volume Docker` à un `conteneur` se fait au moment de son instanciation (`Docker create` ou `Docker run`).
>
>- Si l'on souhaite ajouter d'autres `volumes` plus tard, il faudra ré-instancier le `conteneur` à partir de son image.

<br/>

# Principales commandes de Docker

## Commandes de gestion des images Docker

### `docker images` : Monitorer les images présentes sur la machine hôte

>### Syntaxe :
>
>- **Lister toutes les images**
>
>   **`docker images`** OU **`docker image ls`**

<br/>

### `docker pull` : Récupérer des images standard sur [Docker Hub](https://hub.docker.com/)

>### Syntaxe :
>
>- **Récupérer une image sur Docker Hub**
>
>   **`docker pull <image_name>:<version>`**
>
>   - Si `<version>` n'est pas spécifié, c'est la dernière version (`:latest`) qui sera téléchargée

<br/>

## Commandes de gestion des Conteneurs Docker

### `docker ps` : Monitorer les conteneurs  présents sur la machine hôte

>### Syntaxe :
>
>- **Lister tous les conteneurs en cours d'exécution**
>
>   **`docker ps`** OU **`docker container ls`**
>
>- **Lister tous les conteneurs instanciés (même ceux à l'arrêt)**
>
>   **`docker ps -a`**
