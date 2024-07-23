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

<br/>

### `git init` : Création d'un repository local vierge

>### Syntaxe : 
>
>- **Création d'un nouveau repository dans un dossier spécifique**
>
>   `git init (<path_to_directory>)` : Si le path n'est pas spécifié, le repo est créé à l'endroit où est ouvert le terminal 

<br/>

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

## Commandes de gestion instantanée de la révision courante de l'espace de travail (entre 2 commits)

### `git status` : Monitorer l'état des fichiers de l'espace de travail

Les fichiers de l'espace de travail peuvent se trouver dans 4 états différents : 

1. `Non-suivis` : Ces fichiers ne sont pas stockés dans l'arbre de versionnage, leurs modifications ne sont jamais suivies et il ne seront pas récupérés lors d'un `git clone` du projet par un tiers.

2. `Suivis-non-modifiés` : Ces fichers sont stockés dans l'arbre de versionnage (suite à un précédent commit), et n'ont pas été modifiés au sein de l'édition courante

3. `Suivis-modifiés-non-indexés` : Ces fichiers se trouvent dans l'arbre de versionnage, leur contenu a été modifié depuis le précédent commit, mais ces modifications n'ont pas encore été indexées et ne seront pas prises en compte lors d'un futur commit.  

4. `Suivis-modifiés-indexés` : Ces fichiers se trouvent dans l'arbre de versionnage, leur contenu a été modifié depuis le précédent commit, ces modifications ont été indexées et seront prises en compte lors du futur commit.

>### Syntaxe : 
>
>- **Affichage du statut courant de l'arbre de travail**
>
>    `git status (--short)` : Le --short est fait pour avoir un affichage syntétique

<br/>

### `git add` : Ajouter des fichiers/modifications à l'index de commit (stage)

**Toujours exécuter cette commande avant de faire un commit, sinon les modifications ne seront pas prises en compte et seront perdues...** 

>### Syntaxe : 
>
>- **Ajout d'un nouveau fichier ou d'une modification d'un fichier existant**
>
>    `git add <path_to_file>` : Quand on ajoute un dossier, tous les fichiers et sous-dossiers sont ajoutés récursivement
>
>- **Ajout de tous les éléments possibles**
> 
>    `git add *` : Les fichiers qui se trouve dans le `.gitignore` ne sont jamais ajoutés
>
>- **Ajout de tous les éléments possibles qui correspondent à un pattern donné**
>
>   `git add *<pattern>` : Exemple : `git add *.txt`

<br/>

### `git mv` : Déplacer/Renommer un fichier

Cette commande simule les actions simultanées de suppression du fichier source et d'ajout d'une copie de ce fichier sous un autre nom ou autre path. Le résultat de la commande est directement ajouté à l'index de commit.

>### Syntaxes : 
>
>- **Renommage d'un fichier**
> 
>   `git mv <current_name> <new_name>`
>
>- **Déplacement d'un fichier**
> 
>   `git mv <current_name> <new_path>/<current_name>` : On peut décider de changer le nom du fichier cible au passage
>
>- **Renommage/Déplacement d'un fichier avec écrasement d'un fichier cible ayant le même nom**
>
>   `git mv --force <current_name> <new_name>` : Attention cette action est risquée...
> 
>- **Annulation de la suppression du fichier source après un déplacement/renommage**
>
>   `git restore --staged --worktree <initial_name>` : ça permet également de simuler un copier/coller.

>### <u>**Attention :**</u>
>
>- Ne pas renommer un fichier directement avec l'explorateur de fichier car git ne va pas le répertorier comme une action de renommage. Il va considérer que c'est une suppression de l'ancien fichier et de l'ajout d'un nouveau.
>
>- Bien penser à commit le renommage avant de modifier le fichier, sinon git va considérer que c'est une suppression de l'ancien fichier et de l'ajout d'un nouveau.

<br/>

### `git rm` : Supprimer un fichier de l'arbre de versionnage/espace de travail

Les suppressions sont directement ajoutées à l'index de commit.

>### Syntaxes : 
>
>- **Suppression d'un fichier versionné non modifié dans la révision courante**
> 
>   `git rm <file_path>` : Cela supprime également le fichier de l'espace de travail.
>
>- **Suppression d'un fichier versionné modifié dans la révision courante**
> 
>   `git rm --force <file_path>` : Cela supprime également le fichier de l'espace de travail.
>
>- **Retrait d'un fichier de l'arbre de versionnage**
> 
>   `git rm --cached <file_path>` : Le fichier est toujours présent dans l'espace de travail, mais il se retrouve dans un état `non-suivi`.
>
>- **Suppression d'un dossier**
>
>  `git rm -r <directory_path> ` : Il faut spécifier le -r pour supprimer le dossier et son contenu récursivement (ce n'est pas implicite par sécurité) 

>### <u>**Attention :**</u>
>
>Lorsque les fichiers sont supprimés de l'espace de travail par la commande `git rm`, les fichiers sont supprimés du système sans passer par la corbeille (On peut quand même les récupérer depuis les commits précédents).

<br/>

### `git restore` : Annuler des modifications-suppressions / Retirer des fichiers de l'index de commit

Depuis la version 2.23 de git, c'est cette commande qui est préférée pour annuler des modifications dans l'espace de travail, car elle est plus "safe". Avant l'existence de celle-ci, on utilisait `git reset`, mais c'est plus risqué car `git reset` peut aussi modifier l'historique de commit, alors que `git restore` non.

>### Syntaxes : 
>
>- **Annulation des modifications `non-indexées` d'un fichier**
>
>   `git restore <path_to_file>` : Si le fichier est `indexé`, cela aura pour effet de remettre ce fichier dans l'état dans lequel il se trouve dans l'index (Annulation des modifications ultérieures à l'ajout dans l'index). Sinon, il sera remis dans l'état dans lequel il se trouvait lors du précédent commit.  
>
>- **Retrait d'un fichier `indexé` de l'index**
>
>    `git restore --staged <path_to_file>` : Aucune des modifications faites à ce fichier ne seront annulées.
>
>- **Annulation des modifications d'un fichier `indexé`**
>
>   `git restore --staged --worktree <path_to_file>` : Toutes les modifications faites à ce fichier seront annulées. Cela aura pour effet de retirer également le fichier de l'index.


git reset pour annuler des modifications déjà commitées (reset peut modifier l'historique de commit)

<br/>

### `git commit` : Valider la révision courante et l'ajouter à l'ajouter comme nouvelle entrée dans la branche courante

>### Syntaxes : 
>
>- **Commit de tous les fichiers indexés**
>
>   `git commit -m "<commit_message>"` : Le message de commit est obligatoire. Les modifications qui n'ont pas été ajoutées à l'index ne sont pas perdues et sont gardées pour le commit suivant (si on les add).
>
>- **Commit des fichiers indexés ET des fichiers suivis modifiés-non-indexés (git add oublié)** 
>
>   `git commit -a -m "<commit_message>"` : Les fichiers non-suivis ne sont pas ajoutés au commit.
>
>- **Commit d'un fichier en particulier seulement**
>
>   `git commit <file_name> -m "<commit_message>` : Les autres modifications restent indexées pour le prochain commit.

## Commandes de gestion des branches et de l'arbre de versionnage

### `git log` : Visualiser l'historique des commits sur une branche donnée

>### Syntaxes : 
>
>- **Visualisation de tous les commits**
>
>   `git log` : Le 1er log affiché est le plus récent
>
>- **Visualisation de tous les commits concernant un fichier donné seulement**
>
>   `git log <file_name>`

<br>

### `git reset` : Remettre la branche courante dans un état donné (annuler certains commits) 

<br/>

### `git branch` : Visualiser / Créer / Supprimer des branches dans l'arbre de versionnage
