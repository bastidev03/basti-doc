# Syntaxes et concepts natifs de PHP 8

## Typage

## Attributs
> Un `Attribut` PHP est un moyen d'affecter des métadonnées à un élément de code (Fonction, Classe, Méthode ...).
> Il s'identifie par son nom, et par les paramètres (valeurs scalaires) que l'on peut lui passer.
>
> Dans son implémentation, un `Attribut` est en fait une `Classe` dans laquelle on passe les paramètres à son constructeur 

**Exemple simple de l'implémentation d'une fonctionnalité en utilisant les attributs :**
```php
/** 
 * @uses Imaginons une série de Classes d'animaux (qui étendent toutes une classe mère Animal) 
 * 
 *  Chaque animal a la capacité de déclencher ce qu'on peut appeler un cri (Meuh, beh,...).
 * 
 *  Problématique : Chaque Animal implémente  une / des méthode(s) propre à sa classe pour déclencher ce cri
 *  Un chien implémente la méthode bark(), un chat la méthode meow(), et un âne le couple de méthodes hi() et han()
 * 
 *  Objectif : Créer une fonction générique 'get_noise()' qui prend en paramètre d'entrée, l'instance d'une Classe issue de 'Animal', et qui permet de déclencher son cri, indépendamment des méthodes spécifiques implémentées dans chaque classe pour y parvenir.
 * 
 * Pour cela, un bon moyen est d'utiliser des Attributs que l'on va affecter à ces fonctions.
*/


//Définition de l'attribut
#[Attribute]
class Noise{

}

class Animal {

}

class Dog extends Animal
{
    //Affectation de l'attribut à cette méthode
    //Dans notre exemple, ça veut dire que c'est cette méthode qui définit le 'noise' de l'animal en question
    #[Noise]
    public static function bark(){
        echo "Ouaf !";
    }
}

class Cat extends Animal{
    //Affectation de l'attribut à cette méthode
    //Cette méthode renvoi le bruit (noise) du chat
    #[Noise]
    public static function meow(){
        echo "Miaou !";
    }
}

class Donkey extends Animal{

    //Dans le cas de l'Ane, pour déclencher son cri complet, il faut appeler les 2 méthodes suivantes
    //On affecte donc l'attribut "Noise" à ces 2 méthodes
    //L'ordre d'exécution correspond à l'ordre de définition des méthodes qui portent l'attribut en question
    #[Noise]
    public static function hi(){
        echo "hi";
    }

    //TODO: Mettre un paramètre #[Noise($exec_order)]
    #[Noise]
    public static function han(){
        echo "han";
    }
}

//Cette fonction prend en entrée une instance d'un animal et lui fait déclencher son cri 'noise'
function get_noise(Animal $animal)
{
    $reflection = new ReflectionObject($animal);

    foreach($reflection->getMethods() as $method){
        $attributes = $method->getAttributes(Noise::class);
        if (count($attributes) > 0) {

            $methodName = $method->getName();
            $animal::$methodName();
        }
    }
}

//Déclenchement du cri d'une instance de chien
$animal = new Dog();
get_noise($animal);
```

## Accesseurs (Getters) et mutateurs (Setters)
