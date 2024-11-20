# Principales fonctionnalités de Composer

## Autoloader

### Pré-chargement de Classes non incluses dans les packages Composer installés dans le projet (dossier vendor et déclarées dans composer.json)

>- L'`Autoloading` est un prérequis obligatoire pour pouvoir leur affecter un namespace et les utiliser dans un projet avec la syntaxe **`use`** issue de la norme `PSR-4`. 
>
> - Le système d'`Autoloading` est natif à `PHP`, (Il est donc possible de coder son propre `Autoloader`, mais autant utiliser l'`Autoloader` éprouvé de `composer`) 
> - **Attention ! L'`Autoloader` et le système de `namespace` ne fonctionnent QUE pour pré-charger des `Classes`. Pour charger et exécuter des scripts à part, il faut utiliser les `require`/`include` historiques** 

**Exemple :**
```php
require_once "<PATH_TO_COMPOSER_DIRECTORY>/vendor/autoload.php";

$loader = new \Composer\Autoload\ClassLoader();

//Ajouter une Classe spécifique se trouvant dans un dossier donné
$loader->add('<Class_name>', '<Directory_path>');

//Ajouter toutes les Classes définies sous un même namespace, se trouvant dans un dossier donné
$loader->addPsr4('<Namespace>', '<Directory_path>');

//Enregistrement des pré-chargements effectués
$loader->register();

//L'Autoloader se charge de mapper récursivement dans les dossiers spécifiés pour pré-charger les classes demandées
``` 
