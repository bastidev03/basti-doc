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