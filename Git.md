# Principales commandes de Git

## Commandes globales  

### `git config` : Gérer les configurations par défaut de git (globalement ou localement)

>### Principales syntaxes : 
>
> - **Lecture des configurations**
>
>   `git config --list --show-scope (--show-origin)` : Récupération de la liste des configs existantes avec le scope sur lesquelles elles sont configurées
>
>   `git config --get --show-scope (--show-origin) <config>.<name>` : Récupération d'une config spécifique avec le scope sur lequel elle est configurée (Ramène la valeur utilisée par la suite dans le projet)
>
>   `git config --get-all --show-scope (--show-origin) <config>.<name>` : Récupération de toutes les valeurs d'une config sur tous les scopes sur lesquels elle est définie
>
>- **Modification des configurations**  
>
>   `git config <config>.<name> <value>` : Affectation d'une config dans le scope local 
>
>   `git config --global <config>.<name> <value>` : Affectation d'une config dans le scope global
>
>- **Suppression des configurations**
> 
>   `git config --unset <config>.<name>` : Suppression d'une config dans le scope local
>
>   `git config --global --unset <config>.<name>` : Suppression d'une config dans le scope global
>
> ### Exemples courants :
>
>```git
>::Pour identifier la personne qui a effectué les commits sur le repository (Utiliser les mêmes données que votre compte github/gitlab)
>
> git config --global user.name bastidev03
> git config --global user.email sebastien.molaire@laposte.net
>```

### `git init` : Création d'un repository local vierge

>### Syntaxe : 
>
>- **Création d'un nouveau repository dans un dossier spécifique**
>
>   `git init (<path_to_directory>)` : Si le path n'est pas spécifié, le repo est créé à l'endroit où est ouvert le terminal 

### `git clone` : Récupération en local d'un repository existant (local ou distant)

>### Syntaxes : 
>
>- **Récupération d'un repository local**
>
>    `git clone <path_to_repo_to_clone>`
>
>- **Récupération d'un repository distant**
>
>    `git clone <url_to_repo_to_clone>`