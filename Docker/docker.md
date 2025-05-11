# Introduction Docker ([Documentation officielle](https://docs.docker.com/))

Docker permet d'exécuter des sources/applications dans un environnement isolé (`Environnement docker`) de la machine qui l'exécute que l'on appelle `Conteneur`.

Chaque `conteneur` possède son propre système de fichiers, et toutes les dépendances/logiciels nécessaires pour exécuter le code source y sont installés.  
Ce système de fichiers suit la même arborescence que celui d'un `environnement Linux` sur lequel docker s'appuie pour construire sont écosystème.

**Cela permet d'avoir une base de code qui fonctionne toujours de la même façon quelquesoit la machine physique/OS sur lesquels elle est exécutée.**

**Docker est très bien adapté pour du développement CI/CD**

# Architecture Docker

![Image](./docker_architecture.jpeg)

La commande client **`docker`** (`Docker CLI`) permet d'interagir avec le `démon Docker` (`dockerd`) qui gère en temps réel tous les objets qui composent l'environnement Docker en cours d'exécution (`conteneurs`, `images`, `volumes`, ...).

# Concepts

## Image Docker

Une `Image Docker`, est un `binaire` qui contient l'état et les données d'un système de fichiers spécifique, ainsi que les valeurs de certaines configurations systèmes (linux) spécifiques qui seront instanciées au moment de l'instanciation de l'image dans un `Conteneur Docker` (`docker create` ou `docker run`).  

L'`Image Docker` contient également une liste de commandes/fichiers à exécuter afin de lancer certains `services` (Un serveur web par exemple) au moment où le `Conteneur Docker` sera lancé (`docker start` ou `docker run`).  

Cette `Image` est composée d'un empilement de `couches` qui représentent une suite de modifications à apporter au système de fichiers ou à l'environnement OS (linux) du futur `Conteneur Docker` dans lequel elle sera instanciée (Un peu comme une suite de commits dans un arbre `Git`).

Toutes les `Images Docker` ont pour couche de base, une couche que l'on appelle la **`couche scratch`** qui représente un système de fichiers vide.

Une `Image Docker` peut ainsi être créée, soit à partir de cette `couche scratch` (via un `Dockerfile`), soit à partir d'une autre `Image Docker` (`Image perso` ou `Image officielle` téléchargée depuis le registre [`Docker Hub`](https://hub.docker.com/)), dans laquelle seront ajoutées nos propres `couches` personnelles (Cas le plus commun).  
Il est également possible de créer une `Image Docker` à partir d'une arborescence de dossiers/fichiers stockée dans une `archive TAR`, via la commande `docker import`.

>**!!! Important !!!**
> - Une `Image Docker` est **`immutable`** : Elle ne peut pas être modifiée par le `Conteneur` qui l'exécute
>
> - Si le `Conteneur`, durant son exécution, fait des modifications dans le système de fichier sur lequel il tourne, ces modifications seront supprimées dès que le `Conteneur` sera stoppé.  
>Lorsqu'il sera redémarré, le système de fichier se retrouvera dans le même état que celui qui se trouve dans l'image
>
>- Pour sauvegarder des données, il faut utiliser des `volumes`

>### Formats d'exports des images :
>
>Les `images Docker` peuvent être stockées sous différents formats (Et à différents endroits).
>
>- Format `image` : 
>   - L'image est stockée dans la mémoire du `dockerd`, sous un format prêt à être instancié dans un conteneur.
>   - Toute la meta des `couches` qui la composent est accessible, ainsi que l'historique de commit.
>   - Le **contenu** (arborescence) de l'image n'est pas lisible par un humain
>   - Il s'agit du format par défaut manipulé dans la majorité des cas.
>
>- Format `local` :
>   - Le **contenu** (arborescence) de l'image est stocké dans un dossier sur la `machine hôte`
>   - Le détail des couches qui la composent est perdu
>   - Ce format ne peut pas être importé dans la mémoire du `dockerd`
>
>- Format `tar` :
>   - Le **contenu** (arborescence) de l'image est stocké dans une archive TAR sur la `machine hôte`
>   - Ce format peut être importé dans la mémoire du `dockerd` avec la commande `docker import`
>   - Le détail des couches qui composent l'image d'origine est perdu.
>
>- Format `oci` :
>   - L'image est stockée dans une archive TAR dans un format standard (`OCI`) et importable sur d'autres moteur de conteneurisation que `docker`
>   - Le **contenu** (arborescence) de l'image n'est pas lisible par un humain
>   - Ce format peut être importé dans la mémoire du `dockerd` avec la commande `docker load`
>   - Le détail des couches qui composent l'image d'origine est re-construit en intégralité au moment du chargement dans le `dockerd`  
>
>- Format `docker` : 
>   - Même chose que le format `oci`, à part que l'image est stockée dans une archive TAR dans un format propriétaire (Format étendu à partir d'`OCI`), importable uniquement sur `docker`

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

## Registre Docker

TODO

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

### `docker pull` : Charger des images standard sur le registre [Docker Hub](https://hub.docker.com/)

>### Syntaxe :
>
>- **Récupérer une image sur Docker Hub**
>
>   **`docker pull <image_name>:<version>`**
>
>   - Si `<version>` n'est pas spécifié, c'est la dernière version (`:latest`) qui sera téléchargée
>
>   - L'`image` sera ensuite chargée dans la mémoire interne du `dockerd` (Le démon/serveur docker)

<br/>

### `docker import` : Créer une image à partir d'un dossier stocké dans une archive TAR

**Commande utilisée pour créer une `image docker` directement depuis une arborescence dossier de la machine comme couche de base, sans avoir à utiliser une image pré-existante de dockerHub.**

>### Pré-requis :
>
> - Mettre le contenu (fichiers/sous-dossiers) que l'on souhaite ajouter à l'image docker dans une archive TAR.
>
>### Important :
>
>- **L'image ainsi chargée dans la mémoire du `dockerd` à la fin du process, ne possèdera ni `nom`, ni `tag`, ni historique de commit**
>
>- **Si l'on souhaite charger une image TAR dont le contenu est au format OCI, il faut utiliser la commande `docker load`, ce qui permettra de récupérer tout l'historique de meta de l'image** 


>### Syntaxes :
>
>- **Créer une image simple à partir d'une archive TAR**
>
>   **`docker import "<source_path>.tar" -m "<commit_message>"`**
>
>   - Le <commit_message> est facultatif mais recommandé pour plus de clarté dans l'historique de l'image
>
>   - L'`image` sera ensuite chargée dans la mémoire interne du `dockerd` (Le démon/serveur docker)
>
><br/>
>
>- **Créer une image en y ajoutant des instructions `Dockerfile`**
>
>   **`docker import "<source_path>.tar" -c "<Dockerfile_instructions>"`**
>
>   - Exemple de <Dockerfile_instruction> : `WORKDIR /` 

<br/>

### `docker load` : Charger une image à partir d'une archive TAR au format OCI

Ce moyen de chargement (contrairement à `docker import`), permet de charger une image, ainsi que toute sa meta et son historique dans la mémoire du `dockerd`.

>### Prérequis :
>
>Le contenu du fichier TAR à charger doit être au format `OCI` (Cf. [`Formats des images`](#formats-dexports-des-images))

>### Syntaxes :
>
>- **Charger une image à partir d'un export OCI**
>
>   **`docker load -i <source_path>`**


<br/>

### `docker build` : Créer une image à partir d'un Dockerfile

>### Prérequis :
>
>**Avoir configuré un `Dockerfile` qui définie le process de construction de l'image à builder (cf. [`Doc Dockerfile`](./dockerfile.md))**

>### Syntaxes :
>
>- **Créer une image à partir d'un Dockerfile**
>
>   **`docker build <root_dir_path> -t <image_name>:<image_tag>`**
>
>   - **<root_dir_path> : Chemin relatif, depuis l'endroit où l'on exécute la commande, du dossier racine (référence) pour builder l'image**
>
>      - Par défaut, le builder va chercher à lire le `Dockerfile` présent à cet endroit (`<root_dir_path>/Dockerfile`)
>
>      - **Attention :** Tous les paths présents dans le `Dockerfile` qui référencent des `fichiers sources` à intégrer dans l'image (Instructions `ADD` ou `COPY`), doivent être des **`chemins absolus`** depuis ce `<root_dir_path>` (Raison pour laquelle il est recommandé de mettre les `Dockerfile` au niveau du `<root_dir_path>`que l'on a choisi).  
>     
>         Ces fichiers sources doivent **obligatoirement** se trouver **à l'intérieur** de l'arborescence qui a pour origine le `<root_dir_path>`, sinon le builder ne les trouvera pas. 
>
>   - <image_name> et <image_tag> sont tous les 2 optionnels
>
>   - Une fois la commande exécutée, l'image est chargée dans la mémoire interne du `dockerd`, prête à être instanciée dans un conteneur
> 
><br/>
>
>- **Créer une image à partir d'un Dockerfile se trouvant à un endroit différent du <root_dir_path>**
>
>   **`docker build <root_dir_path> -f <dockerfile_path> -t <image_name>:<image_tag>`**
>
>      - <dockerfile_path> : Chemin relatif, depuis l'endroit où l'on exécute la commande, du `dockerfile` à utiliser.
>
><br/>
>
>- **Créer une image et exporter le résultat dans un format spécifique**
>
>   **`docker build <root_dir_path> -o type=<export_format>,dest=<target_path>`**
>
>      - <export_format> : Le format d'export de l'image (`local`, `tar`, `oci`, `docker`, `image` ou `registry` => Cf. [`Formats des images`](#formats-dexports-des-images))
>
>         - Quand l'option "-o" n'est pas spécifiée, c'est le format `image` qui est utilisé (Comportement par défaut de la commande)
>
>         - Le format `registry` correspond à un `Docker push` de l'image générée au format `image`

<br/>

### `docker save` : Exporter une image au `format docker`

Permet d'exporter toutes les données d'une `image docker` (contenu, meta, historique), dans une archive TAR au `format docker` (cf. [`Formats des images`](#formats-dexports-des-images)).  

>### Syntaxes :
>
>- **Exporter une image au format DOCKER**
>
>   **`docker save <image_name> -o <target_path>.tar`**
>
>   - <image_name> : Le nom de l'image utilisée ou son hash/id

>### Remarques :
>
>- On peut ensuite utiliser cet `export` pour ré-importer cette image docker via la commande `docker load`.
>
>

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

<br/>

### `docker create` : Créer un conteneur à partir d'une image

Permet de créer un conteneur à partir d'une image, mais sans le démarrer. Pour démarrer automatiquement le conteneur au moment de son instanciation utiliser `docker run`.

>### Syntaxes : 
>
>- **Créer un conteneur à partir d'une `image docker`**
>
>   `docker create <image_name> --name <container_name>`
>
>   - <image_name> : Le nom de l'image utilisée ou son hash/id

<br/>

### `docker start` : Démarrer un conteneur

>### Syntaxes :
>
>- **Démarrer un conteneur**
>
>   `docker start <container_name>`
>
>   - <container_name> : Le nom du conteneur utilisé ou son hash/id

<br/>

### `docker run` : Démarrer directement un conteneur à partir d'une image

>### Syntaxes : Voir les syntaxes de `docker create` et `docker start`

<br/>

### `docker export` : Exporter le contenu d'un conteneur au `format tar`

Permet d'exporter toute l'arborescence d'un conteneur dans une archive TAR au `format tar` (cf. [`Formats des images`](#formats-dexports-des-images)).  

**Commande utilisée principalement pour inspecter le contenu d'un `conteneur` que l'on n'arrive pas à démarrer.**

>### Syntaxe :
>
>- **Exporter l'arborescence d'un conteneur dans une archive TAR**
>
>   **`docker export <container_name> --output="<target_path>.tar"`**
>
>   - <container_name> : Le nom du conteneur utilisé ou son hash/id

>### Remarques :
>
>- On peut ensuite utiliser cet `export` pour créer une nouvelle image docker à partir de celui-ci via la commande `docker import`.
>
>- **Attention :** si on fait ça, la nouvelle image créée repartira avec ce contenu comme nouvelle couche de base. Les potentielles couches intermédiaires présentes dans l'image utilisée pour instancier le conteneur original seront perdues (Tout comme l'historique de commit).
>
>- Si l'on ne veut pas perdre cet historique, utiliser la commande **`docker commit`** sur ce conteneur, ou **`docker save`** sur l'image utilisée par le conteneur.
