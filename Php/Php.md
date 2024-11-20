# Syntaxes et concepts natifs de PHP 8

## Typage

## Atributs
```php

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

    //Dans le cas de l'Ane, pour déclencher son bruit complet, il faut appeler les 2 méthodes suivantes
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

//Cette fonction prend en entrée une instance d'un animal et lui fait déclencher son bruit 'noise'
function getNoise(Animal $animal)
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

$animal = new Dog();
getNoise($animal);
```