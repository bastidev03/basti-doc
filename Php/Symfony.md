# Principales fonctionnalités de Symfony

## Installation

### Prérequis

> - Installer `Symfony-CLI`
>
> - Installer `PHP` 8.3 ou plus
>   - `Symfony` utilise les dernières syntaxes PHP 8 dans son framework
>
> - Installer `Composer`
>   - `Symfony-CLI` installe des packages PHP par défaut via `Composer` 
>

### Commandes d'installation
Dans un terminal, saisir les commandes suivantes :
```powershell
# Vérification que tout est bien installé sur la machine pour générer un projet symfony
symfony check:requirements

# Commande de création du projet
symfony new <project_name> --version="7.1.*" --php=8.3 --webapp --docker --cloud
```

### Lancement du projet sur un serveur local et visualisation des logs d'accès
Dans un terminal, saisir les commandes suivantes :
```powershell
# Lancement du projet
symfony server:start -d

# Visualisation du projet sur un navigateur
symfony open:local

# Visualisation des logs d'accès générés par le serveur
symfony server:log 

# Arrêt du projet
symfony server:stop
```
>**Remarque :** L'utilisation la commande `symfony server` peut se faire également sur un projet `PHP` sans le framework `symfony` installé dessus.

## Commandes symfony-CLI utiles 

```powershell
# Création d'un Contrôleur http dans 'src/Controller/<ControllerName>.php'  avec une route par défaut '/'
symfony console make:controller <ControllerName>

# Visualisation de toutes les routes définies dans l'App
symfony console debug:router

# Visualisation de la configuration d'une route en particulier
symfony console debug:router <RouteName>

# Trouver une route associée à un path navigateur (URL)
symfony console router:match <PathName>
```

## Fonctions php utilitaires définies par symfony

### Envoi de logs serveurs dans la console de debug symfony
```php
array $var = ['test' => 'value'];

//Fonction permettant de logger des éléments serveurs dans la console frontend symfony
dump($var);
```
## Principaux Concepts et Classes/Namespaces prédéfinis

### HTTP foundations

> Groupement de Classes PHP pré-définies par `Symfony` pour gérer tout ce qui concerne les requêtes HTTP en POO :
> - Récupération des données de requête de $_REQUEST, $_POST, $_FILES, ...
> - Gestion des headers, sessions ...
> - Génération des réponses renvoyées au client  
>

Principales classes HTTP foundations :
```php
//Classe Response => Génère une réponse HTTP
use Symfony\Component\HttpFoundation\Response;

//Classe Request => Récupère une requête HTTP
use Symfony\Component\HttpFoundation\Request;
```

### Entities de l'ORM Doctrine

> Une `entity` Doctrine est une classe `Php` qui permet de définir les caractéristiques (modèle) d'une table dans la BDD associée au projet :
> - Définition des caractéristiques de tous les champs (type, encodage, ...)
> - Définition de la clé primaire
> - Définition des éventuelles clés étrangères
>
>**Chaque élément de donnée (ligne de donnée) issu de la table en question, est récupéré par Doctrine dans une instance de la classe `entity` en question.**
>
>**C'est donc là que l'on définie l'`API` de lecture des données de la table.**
>
>**C'est également ce fichier de définition qui sert à créer/modifier la table en question dans la BDD gâce à la commande `symfony doctrine orm:schema-tool:<create/update/drop>`**

Exemple de définition d'une classe entity dans Symfony avec les attributs :
```php
<?php
// src/Entity/Product.php

use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
#[ORM\Table(name: 'products')]
class Product
{
    #[ORM\Id]
    #[ORM\Column(type: 'integer')]
    #[ORM\GeneratedValue]
    private int|null $id = null;
    #[ORM\Column(type: 'string')]
    private string $name;

    // .. (other code)
}
```

### Repositories de l'ORM Doctrine