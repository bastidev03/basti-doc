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
# Création d'un contrôleur http dans 'src/Controller/<ControllerName>.php'  avec une route par défaut '/'
symfony console make:controller <ControllerName>
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
