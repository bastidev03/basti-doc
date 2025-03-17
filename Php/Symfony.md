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
symfony new <project_name> --version="7.1.*" --php=8.4 (--demo) (--webapp) (--docker) (--cloud)
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

# Création d'une entity Doctrine et de son repository 
symfony console make:entity <EntityName>

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

>### <u>Attention :</u>
>
>Lorsqu'on définie une classe dans symfony dans l'arborescence du dossier de travail, **"src/dirname/filename.php"**, la classe doit obligatoirement être créée sous le namespace **"App\dirname"** et le nom de la classe doit être **"filename"** (Norme PSR-4), sinon le framework sortira une erreur (Non sensible à la casse).

### HTTP foundations

 Groupement de Classes PHP pré-définies par `Symfony` pour gérer tout ce qui concerne les requêtes HTTP en POO :
 - Récupération des données de requête de $_REQUEST, $_POST, $_FILES, ...
 - Gestion des headers, sessions ...
 - Génération des réponses renvoyées au client  

**Principales classes HTTP foundations :**
```php
//Classe Response => Génère une réponse HTTP
use Symfony\Component\HttpFoundation\Response;

//Classe Request => Récupère une requête HTTP
use Symfony\Component\HttpFoundation\Request;
```

### Attributs de Routes

La classe `Route`, permet d'affecter un attribut de route à une méthode d'un `Controller`. Cette méthode, sera alors exécutée lorsqu'un appel HTTP sera fait sur cette route

**Exemple d'implémentation :**
```php
<?php
namespace App\Controller;
use Symfony\Component\Routing\Attribute\Route;
class MyController
{
    #[Route('/')]
    public function sayHello()
    {
        die('Hello World');
    }
}
```

### Entities de l'ORM Doctrine

> Une `entity` Doctrine est une classe `Php` qui permet de définir les caractéristiques (modèle) d'une table dans la BDD associée au projet :
> - Définition des caractéristiques de tous les champs (type, encodage, ...)
> - Définition de la clé primaire
> - Définition des éventuelles clés étrangères
>
>Chaque élément de donnée (ligne de donnée) issu de la table en question, est récupéré par Doctrine dans une instance de la classe `entity` en question.
>
>Si des éléments de données sont liés à cette `entity` par clé étrangère dans une autre table (autre `entity`), Doctrine récupère également automatiquement dans une `collection` (liste/array), la liste des instances de cette autre`entity` qui lui sont associées.
>
>C'est donc là que l'on définie l'`API` de lecture des données de la table.
>
>C'est également ce fichier de définition qui sert à créer/modifier la table en question dans la BDD gâce à la commande `symfony doctrine orm:schema-tool:<create/update/drop>`
>
>Quand une propriété d'une `entity` liée à un champ de table est modifiée, Doctrine modifie automatiquement le champ correspondant dans la table quand la méthode `flush()` de l'`entity manager` sera exécutée

Exemple de définition de 2 classes `entity` dans Symfony liées par une clé étrangère dans une relation (1,n) en utilisant les attributs :
```php
<?php
//Use case : 
//Imagineons 2 tables (persons et products)
// 1 Personne peut posséder plusieurs produits
// 1 Produit ne peut être associé qu'à 1 seule Personne
//Dans le modèle cible de la BDD, on veut avoir une clé étrangère dans la table products qui référence l'id de son possesseur

// src/Entity/Person.php
use Doctrine\ORM\Mapping as ORM;
use Doctrine\Common\Collections\ArrayCollection;

#[ORM\Entity]
#[ORM\Table(name: 'persons')]
class Person
{
    //Spécifie qu'il s'agit de la clé primaire
    #[ORM\Id]
    #[ORM\Column(type: 'integer')]
    //Spécifie qu'à l'insertion d'un nouveau Product, ce champ est calculé automatiquement
    #[ORM\GeneratedValue]
    private int|null $id = null;
    
    //Les propriétés utilisées pour définir des champs de table doivent TOUJOURS être privées
    #[ORM\Column(type: 'string')]
    private $first_name = '';

    //Mise en place des assesseurs / mutateurs sur cette propriété
    public function getFirstName(): string
    {
        return $this->first_name;
    }
    public function setFirstName(string $first_name): void
    {
        $this->first_name = $first_name;
    }

    #[ORM\Column(type: 'string')]
    private $last_name = '';

    public function getLastName(): string
    {
        return $this->last_name;
    }
    public function setLastName(string $last_name): void
    {
        $this->last_name = $last_name;
    }

    //La clé étrangère est dans la table 'products', donc l'entity 'Person' est du côté 'inverse' de la relation ('Inverse side')
    //Une Collection dans la modélisation de l'entity 'inverse' regroupe nativement la liste des éléments liés par la clé étrangère dans l'entity 'propriétaire'
    //Quand on récupèrera l'instance de l'entity Person, on aura automatiquement dans la propriété product_list, la liste des instances de l'entité Product associées et stockées dans une ArrayCollection (La jointure est faite automatiquement pendant le construct)
    //On spécifie que du point de vue de l'entité Person, on est dans une relation 'OneToMany' avec l'entité Product
    //On spécifie que l'entity 'propriétaire' est Product et que la clé étrangère se trouve sur la propriété 'owner'
    #[ORM\OneToMany(targetEntity: Product::class, mappedBy:'owner')]
    private Collection $product_list;

    public function getProductList() : Collection
    {
        return $this->product_list;
    }
    //Pour apporter des changements dans une collection, il faut aller ajouter/modifier l'entity dans l'entity 'propriétaire'
    //Ajouter ou supprimer une entity dans une collection depuis une entity 'inverse' n'enregistrera JAMAIS les changements en BDD
    public function addProduct(Product $product): void 
    {
        //!! USELESS => THE DATA WILL NOT PERSIST INTO BDD BY THIS ACTION ONLY!!
        $this->product_list->add($product);

        //!!THIS WILL WORK!!
        $this->product_list->add($product);
        $product->setOwner($this);
    }

}

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

    public function getName(): string
    {
        return $this->name;
    }
    public function setName(string $name): void
    {
        $this->name = $name;
    }

    #[ORM\Column(type: 'float')]
    private string $price;

    public function getPrice(): float
    {
        return $this->price;
    }
    public function setPrice(float $price): void
    {
        $this->price = $price;
    }

    //La clé étrangère est dans la table 'products', donc l'entité 'Product' est du côté 'propriétaire' de la relation ('Owning side')
    //Quand on récupèrera l'instance de l'entity Product, on aura automatiquement dans la propriété owner, l'instance de l'entity Person associée (La jointure est faite automatiquement pendant le construct).
    //On spécifie que du point de vue de l'entité Product, on est dans une relation 'ManyToOne' avec l'entité Person
    //On spécifie que l'entity 'inverse' est Person et que la propriété dans l'entity Person qui fait référence à cette clé étrangère est 'product_list'
    //On peut rajouter des propriétés de jointure avec l'attribut JoinColumn (opt)
    #[ORM\ManyToOne(targetEntity: Person::class, inversedBy: 'product_list')]
    #[ORM\JoinColumn(nullable: false)]
    private Person $owner
    
    public function getOwner(): Person
    {
        return $this->owner;
    }

    public function setOwner(Person $owner): void
    {
        $this->owner = $owner;
    }
}
```

### Repositories de l'ORM Doctrine
>Un `repository` de l'ORM Doctrine est une classe associée à une classe `entity` d'une table, et qui stocke toutes les actions métiers liées à cette `entity`.
>
>Chaque `repository` est étendu à partir d'une classe Doctrine commune, et possède des méthodes génériques pour effectuer des actions `CRUD` récurrentes.
>
>Ce sont dans ces `repositories` que sont également implémentées les méthodes métier spécifiques à cette `entity`
>
>**Ce sont les repositories qui sont utilisés par les contrôleurs métiers pour interagir avec la BDD**

Exemple de défintion d'une classe `repository` dans symfony : 
```php
<?php
// src/repository/BugRepository.php

use Doctrine\ORM\EntityRepository;

class BugRepository extends EntityRepository
{
    //Exemple de méthodes spécifiques implémentées pour matcher une demande métier sur cette entité 
    public function getRecentBugs($number = 30)
    {
        //Généralement dans doctrine, les requêtes sont écrites en DQL (Doctrine Query Language)
        $dql = "SELECT b, e, r FROM Bug b JOIN b.engineer e JOIN b.reporter r ORDER BY b.created DESC";

        //C'est l'instance partagée entityManager() qui sert systématiquement de pont avec la BDD.
        $query = $this->getEntityManager()->createQuery($dql);
        $query->setMaxResults($number);
        //La méthode getResult() renvoie la liste des instances de la classe entité Bug dans un array
        return $query->getResult();
    }

    public function getRecentBugsArray($number = 30)
    {
        $dql = "SELECT b, e, r, p FROM Bug b JOIN b.engineer e ".
               "JOIN b.reporter r JOIN b.products p ORDER BY b.created DESC";
        $query = $this->getEntityManager()->createQuery($dql);
        $query->setMaxResults($number);
        return $query->getArrayResult();
    }
}
```
