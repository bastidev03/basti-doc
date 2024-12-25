# Principales commandes de Git

## Raccourcis de la CLI

### `q` : Permet de sortir de l'interface d'une commande Git (Après un `git log` par ex)

## Commandes globales  

### `git config` : Gérer les configurations par défaut de git (globalement ou localement)

>### Principales syntaxes : 
>
> - **Lecture des configurations**
>
>   **`git config --list --show-scope (--show-origin)`** :
>
>   - Récupération de la liste des configs existantes avec le scope sur lesquelles elles sont configurées
>
>   **`git config --get --show-scope (--show-origin) <config>.<name>`** :
>
>   - Récupération d'une config spécifique avec le scope sur lequel elle est configurée (Ramène la valeur utilisée par la suite dans le projet)
>
>   **`git config --get-all --show-scope (--show-origin) <config>.<name>`** :
>
>   - Récupération de toutes les valeurs d'une config sur tous les scopes sur lesquels elle est définie
>
>- **Modification des configurations**  
>
>   **`git config <config>.<name> <value>`** : 
>
>   - Affectation d'une config dans le scope local 
>
>   **`git config --global <config>.<name> <value>`** : 
>
>   - Affectation d'une config dans le scope global
>
>- **Suppression des configurations**
> 
>   **`git config --unset <config>.<name>`** : 
>
>   - Suppression d'une config dans le scope local
>
>   **`git config --global --unset <config>.<name>`** : 
>
>   - Suppression d'une config dans le scope global

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
>   **`git init (<path_to_directory>)`** : 
>
>   - Si le path n'est pas spécifié, le repo est créé à l'endroit où est ouvert le terminal 

<br/>

### `git clone` : Récupération en local d'un repository existant (local ou distant)

>### Syntaxes : 
>
>- **Récupération d'un repository local**
>
>    **`git clone <path_to_repo_to_clone>`**
>
>- **Récupération d'un repository distant**
>
>    **`git clone <url_to_repo_to_clone>`**

<br>

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
>    **`git status (--short)`** : 
>
>   - Le --short est fait pour avoir un affichage syntétique

<br/>

### `git add` : Ajouter des fichiers/modifications à l'index de commit (stage)

**Toujours exécuter cette commande avant de faire un commit, sinon les modifications ne seront pas prises en compte et seront perdues...** 

>### Syntaxe : 
>
>- **Ajout d'un nouveau fichier ou d'une modification d'un fichier existant**
>
>    **`git add <path_to_file>`** : 
>
>   - Quand on ajoute un dossier, tous les fichiers et sous-dossiers sont ajoutés récursivement
>
>- **Ajout de tous les éléments possibles**
> 
>    **`git add *`** : 
>
>   - Les fichiers qui se trouve dans le `.gitignore` ne sont jamais ajoutés
>
>- **Ajout de tous les éléments possibles qui correspondent à un pattern donné**
>
>   **`git add *<pattern>`** : 
>
>   - Exemple : `git add *.txt` => Ajoute toutes les extensions en ".txt"

<br/>

### `git mv` : Déplacer/Renommer un fichier

Cette commande simule les actions simultanées de suppression du fichier source et d'ajout d'une copie de ce fichier sous un autre nom ou autre path. Le résultat de la commande est directement ajouté à l'index de commit.

>### Syntaxes : 
>
>- **Renommage d'un fichier**
> 
>   **`git mv <current_name> <new_name>`**
>
>- **Déplacement d'un fichier**
> 
>   **`git mv <current_name> <new_path>/<current_name>`** : 
>
>   - On peut décider de changer le nom du fichier cible au passage
>
>- **Renommage/Déplacement d'un fichier avec écrasement d'un fichier cible ayant le même nom**
>
>   **`git mv --force <current_name> <new_name>`** : 
>
>   - Attention cette action est risquée...
> 
>- **Annulation de la suppression du fichier source après un déplacement/renommage**
>
>   **`git restore --staged --worktree <initial_name>`** : 
>
>   - ça permet également de simuler un copier/coller.

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
>   **`git rm <file_path>`** : 
>
>   - Cela supprime également le fichier de l'espace de travail.
>
>- **Suppression d'un fichier versionné modifié dans la révision courante**
> 
>   **`git rm --force <file_path>`** : 
>
>   - Cela supprime également le fichier de l'espace de travail.
>
>- **Retrait d'un fichier de l'arbre de versionnage**
> 
>   **`git rm --cached <file_path>`** : 
>
>   - Le fichier est toujours présent dans l'espace de travail, mais il se retrouve dans un état `non-suivi`.
>
>- **Suppression d'un dossier**
>
>   **`git rm -r <directory_path>`** : 
>
>   - Il faut spécifier le -r pour supprimer le dossier et son contenu récursivement (ce n'est pas implicite par sécurité) 

>### <u>**Attention :**</u>
>
>Lorsque les fichiers sont supprimés de l'espace de travail par la commande `git rm`, les fichiers sont supprimés du système sans passer par la corbeille (On peut quand même les récupérer depuis les commits précédents).

<br/>

### `git restore` : Annuler des modifications-suppressions / Retirer des fichiers de l'index de commit

Depuis la version 2.23 de git, c'est cette commande qui est préférée pour annuler des modifications dans l'espace de travail, car elle est plus "safe". Avant l'existence de celle-ci, on utilisait `git reset`, mais c'est plus risqué car `git reset` peut aussi modifier l'arbre de versionnage, alors que `git restore` non.

>### Syntaxes : 
>
>- **Annulation des modifications `non-indexées` d'un fichier**
>
>   **`git restore <path_to_file>`** : 
>
>   - Si les modifications sont `indexées`, le fichier sera remis dans l'état dans lequel il se trouve dans l'index (Annulation des modifications ultérieures à l'ajout dans l'index s'il y en a). 
>
>   - Si les modifications sont `non-indexées`, le fichier sera remis dans l'état dans lequel il se trouvait lors du précédent commit.  
>
>- **Retrait d'un fichier `indexé` de l'index**
>
>    **`git restore --staged <path_to_file>`** : 
>
>   - Aucune des modifications faites à ce fichier ne seront annulées.
>
>- **Annulation des modifications d'un fichier `indexé`**
>
>   **`git restore --staged --worktree <path_to_file>`** : 
>
>   - Toutes les modifications faites à ce fichier seront annulées. Cela aura pour effet de retirer également le fichier de l'index.

<br/>

### `git commit` : Valider la révision courante et l'ajouter à l'ajouter comme nouvelle entrée dans la branche courante

>### Syntaxes : 
>
>- **Commit de tous les fichiers indexés**
>
>   **`git commit -m "<commit_message>"`** : 
>
>   - Le message de commit est obligatoire. Les modifications qui n'ont pas été ajoutées à l'index ne sont pas perdues et sont gardées pour le commit suivant (si on les add).
>
>- **Commit des fichiers indexés ET des fichiers suivis modifiés-non-indexés (git add oublié)** 
>
>   **`git commit -a -m "<commit_message>"`** : 
>
>   - Les fichiers non-suivis ne sont pas ajoutés au commit.
>
>- **Commit d'un fichier en particulier seulement**
>
>   **`git commit <file_name> -m "<commit_message>`** : 
>
>   - Les modifications des autres fichiers restent indexées pour le prochain commit.

<br>

## Commandes de gestion de l'arbre de versionnage courant (Branche courante)

### `git log` : Visualiser l'historique des commits du projet dans la console

Cette commande permet également de visualiser dans la console le graphe de toutes les branches du projet

>### Syntaxes : 
>
>- **Visualisation de tous les commits de la branche courante avec leur hash associé**
>
>   **`git log`**
>
>   **`git log --oneline`**
>
>   - Le 1er log affiché est le plus récent
>   - Le paramètre `oneline` permet d'avoir un affichage plus compact des lignes de commit
>
>- **Visualisation de tous les commits concernant un fichier donné seulement**
>
>   **`git log <file_name>`**
>
>- **Visualisation du graphe des branches complet du projet (avec leurs commits associés)**
>
>   **`git log --all --graph --decorate --oneline`**

<br>

### `gitk` : Ouvrir l'interface graphique de visualisation de l'arbre du projet

Cette interface graphique affiche toutes les informations concernant l'historique du projet (arbre des branches, visualisation des modifications apportées à chaque commit, modifications courantes non commitées ...)

> ### Syntaxes :
>
>- **Visualisation du graphe des branches complet du projet (avec leurs commits associés)**
>
>   **`gitk --all`**  

> Il existe des interfaces graphiques locales de gestion des `repositories` Git plus complètes que l'interface `gitk` native (GitKraken, SourceTree, GitHub Desktop, ...). Ces logiciels ne seront pas abordés ici.

<br>

### `git reset` : Remettre la branche courante dans un état donné (annuler certains commits) 

Cette fonction permet de supprimer/annuler des commits jusqu'à un hash de commit précis (ne pas confondre avec `git revert` qui ajoute un commit inverse). 

>### Syntaxes : 
>
>- **Suppression des commits pour revenir à un hash précis**
>
>   **`git reset <commit_hash>`** : 
>
>   - Toutes les modifications faites dans les commits qui viennent d'être supprimés, sont conservées dans la copie de travail courante en état `non-indexé`. 
>
>   - Les éventuelles modifications faites au sein de la révision courante avant le reset, sont également conservées, mais celles qui étaient `indexées` repassent en `non-indexées`.
>
>- **Suppression des commits tout en gardant les modifications des commits supprimés dans l'index**
>
>   **`git reset --soft <commit_hash>`** : 
>
>   - Les modifications qui étaient `non-indexées` au sein de la révision courante avant le reset, resteront `non_indexées` après.
>
>- **Suppression des commits sans garder aucune des modifications**
> 
>   **`git reset --hard <commit_hash>`** : 
>
>   - Les modifications au sein de la révision courante avant le reset sont également supprimées
>
>- **Annulation d'un `git reset`**
>
>   **`git reset "HEAD@{1}"`** : 
>
>   - Fait référence à l'état de l'arbre de versionnage UNE commande avant celle-ci (le précédent `git reset` en l'occurence). 
>
>- **Remise en état de l'arbre de versionnage dans lequel il était, N commandes avant l'état actuel**
>
>   **`git reset "HEAD@{<reference_number>}"`** : 
>
>     - Permet de retrouver un état antérieur de l'arbre même si on a fait plusieurs `git reset / commit / merge` entre temps.
> 
>    - Pour retrouver le bon <reference_number>, utiliser la commande `git reflog`. 

<br/>

### `git revert` : Ajouter un commit qui annule un commit antérieur

Cette fonction permet d'exécuter l'action inverse d'un commit antérieur pour en annuler les modifications.

>### <u>**Attention :**</u>
>
>- Lorsque le commit que l'on `revert` n'est pas le dernier commit, des risques de conflits peuvent survenir.
>
>- **S'assurer que l'`index` de la révision courante est bien vide**, car sinon, le `commit` automatique de la commande `revert` se mettra en erreur. (Si utilisation du param `no-commit`, les modifications `indexées` passeront dans le `commit` manuel effectué après la commande `revert`) 


>### Syntaxes :
>
>- **Annulation d'un commit unique**
>
>   **`git revert <commit_hash> --no-edit`**
>
>      - Le paramètre `no-edit` permet à git de générer un message automatique pour le commit. (C'est mieux, car ça évite l'ouverture d'un éditeur de message de commit qui ne fonctionne pas toujours très bien).
>
>      - Les modifications courantes `non-indexées` sont conservées après le commit du `revert`
>
>-  **Annulation d'un commit dont on veut personnaliser le message**
>
>       **`git revert <commit_hash> --no-commit`**
>
>       **`git commit -m "<message>"`**
>
>       - Les modifications se retrouvent `indexées` dans la révision courante
>       - ça évite l'utilisation de l'éditeur de message défectueux
>
>-   **Annulation de plusieurs commits dans un seul commit de revert**
>
>       **`git revert <commit_hash_1> --no-commit`**
>
>       **`...`**
>
>       **`git revert <commit_hash_n> --no-commit`**
>
>       **`git commit -m "<message>"`**
>
>       - On ajoute ainsi successivement les annulations dans des commits successifs dans l'index courant avant de commit le revert global.
>
>-   **Annulation d'un commit de merge**
>
>       **`git revert <commit_hash> -m <parent_number>`**
>
>       - TODO : Commande a tester pour RETEX (Définition de parent_number, modifications ultérieures non prises en compte)

<br/>

## Commandes de gestion des branches

### `git branch` : Visualiser / Créer / Supprimer des branches dans l'arbre de versionnage

>### Syntaxes : 
>
>- **Visualisation des branches du projet en local**
>
>   **`git branch --list`**
>
>- **Visualisation des branches du/des projet(s) distant(s)**
>
>   **`git branch --remotes`**
>
>- **Création d'une nouvelle branche à partir de l'endroit où l'on se trouve (branche et commit commit courant)**
>
>   **`git branch <branch_name>`**
>
>- **Renommage d'une branche**
>
>   **`git branch -m <source_branch_name> <target_branch_name>`**
>
>    - Si **un seul** paramètre est spécifié, c'est la branche sur laquelle on se trouve qui sera renommée avec la valeur du paramètre
>
>- **Copie d'une branche**
>
>   **`git branch -c <source_branch_name> <target_branch_name>`**
>
>    - Si un seul paramètre est spécifié, c'est la branche sur laquelle on se trouve qui sera copiée avec la valeur du paramètre
>    - **Attention :** Ne pas confondre la copie d'une branche, avec la création d'une nouvelle branche à partir d'une autre (Le point de `fork` ne sera pas créé au même endroit) 
>
>- **Suppression d'une branche**
>
>   **`git branch --delete <branch_name>`**
