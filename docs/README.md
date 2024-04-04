# Marmiwild, épisode 4


Tous tes efforts ont fait de toi un véritable chef 👨‍🍳!  
A toi la consécration, la gloire et surtout : ton propre restaurant.

Mais attention, si tu essaies de tout faire tout seul, tu risques très vite d'être victime de ton succès 😰.
Simplifie-toi la vie ! 😄 Embauche de l'aide.

![](images/giphy.gif)
{: .text-center }

## Objectifs
* Initialiser composer
* Installer et configurer Twig

Clone ce dépôt grâce au lien donné au début de cette page ⬆ à&nbsp;la&nbsp;section&nbsp;<a href="#input-clone"><i class="bi bi-code-slash"></i>&nbsp;Code</a>.  
(pense à recréer un fichier *config.php* avec tes données de connexion)
{: .alert-info } 

## 1. Embaucher un second de cuisine : Composer

Comme en cuisine, tes projets web intégreront des ingrédients et préparations régulièrement utilisés. Tu viens de l'apprendre, en PHP, tu vas pouvoir utiliser le gestionnaire de dépendances __Composer__ pour gérer, entre autres, les différentes pièces détachées réutilisables dans ton application.  
Initialise la configuration avec la commande suivante :
```bash
composer init
```
La CLI t'invite à répondre à une série de questions.  
Fais en sorte d'obtenir la configuration suivante dans ton fichier `composer.json` (appuie-toi sur ce que tu viens de voir en cours) :
```json
{
    "name": "app/marmiwild",
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    },
    "require": {}
}
```
N'oublie pas de relancer la commande `composer dump-autoload` car tu as certainement dû modifier manuellement le mapping PSR-4 😉.

## 2. Lui demander de préparer les fourneaux
L'autoloading de __Composer__ est maintenant configuré, tu vas pouvoir y faire référence et utiliser les espaces de nom (namespaces).
- Ajoute, au début du fichier `index.php` du dossier public, un *require* du fichier `autoload.php` généré par __Composer__.  
__💡 le fichier en question est situé dans le dossier */vendor*__.
- Ajoute au début des fichiers `RecipeModel.php` et `RecipeController.php` les namespaces appropriés.
- Dans le fichier `RecipeController.php`, sous la ligne du namespace que tu viens d'écrire, ajoute le *use* de la classe `RecipeModel`. Ton IDE ne devrait plus souligner en rouge les références à cette classe maintenant 😎. Tu peux en profiter pour supprimer le *require* qui n'est plus nécessaire.
- Enfin, dans le fichier `routing.php`, modifie l'instance de la classe `RecipeController` par son FQCN, comme ceci (le *require* pourra aussi être retiré) :
```php
$recipeController = new App\Controllers\RecipeController();
```

Si tu ne vois aucune différence dans ton navigateur et que PHP ne renvoie aucune erreur, c'est que tu as bien réussi, bravo ! 🥳.  
Si ce n'est pas le cas, pas d'inquiétudes, un instant debug est prévu dans la brigade 👨🏼‍🍳.

## 3. Soigner le dressage !
Les fourneaux sont préchauffés. Casserolles et autres marmites n'attendent plus que le coup de feu. Installons notre première dépendance 🚀.

C'est maintenant le grand jour. On te l'avait promis, terminé les salades de PHP/HTML & HTML/PHP... ! Tu vas utiliser le système de templating Twig pour gérer les vues de ton application.  
Pour l'installer, c'est très facile avec __Composer__. Lance la commande suivante :

```bash
composer require twig/twig
```
Tu vas maintenant t'en servir dans ton controller `RecipeController`.  
Tout d'abord, ajoute ces quelques lignes à ta classe sans oublier les *use* :

```php
<?php

namespace App\Controllers;

use Twig\Environment;
use Twig\Loader\FilesystemLoader;

class RecipeController
{
    private Environment $twig;

    public function __construct()
    {
        $loader = new FilesystemLoader(__DIR__ . '/../Views/');
        $this->twig = new Environment($loader);
        //
    }
    //...
}
```    

## 4. Assaisonner... avec des brindilles !
À présent, modifie les extensions `.php` des fichiers du dossier `Views` pour `.html.twig`.  
Voici ensuite la marche à suivre pour remplacer les *require*.  
Prenons l'exemple de la méthode `browse()` de la classe `RecipeController`. Remplace la ligne du *require* pour obtenir le code ci-dessous :

```php
public function browse(): string
{
    $recipes = $this->model->getAll();
    return $this->twig->render('indexRecipe.html.twig', [
        'recipes' => $recipes
    ]);
}
```
La méthode retourne à présent le template html traité par Twig qui reçoit au passage le tableau `$recipes`.
Si tu ne vois rien à l'écran, c'est normal, la méthode n'affiche plus rien directement. Elle retourne quelque chose à afficher 🤓.  
Rends-toi dans le fichier `routing.php`, et ajoutes-y le `echo` nécessaire :

```php
if ('/' === $urlPath) {
    echo $recipeController->browse();
}
```
Ça marche mieux, non ?
Je te laisse modifier les autres méthodes de la classe `RecipeController` sur le même principe.

## 5. Servir à point
Dernière étape : afficher les valeurs.  
Les templates Twig sont prêts et connectés aux méthodes de la classe `RecipeController`. Ils reçoivent des tableaux de variables contenant une ou des recettes, voire un tableau d'erreur pour le template `form.html.twig`.
Fais en sorte de les afficher en utilisant la syntax Twig qui remplacera ce que tu avais fait en PHP.

📣 Et une victoire pour la 12 ! 🥳
{: .text-center }
