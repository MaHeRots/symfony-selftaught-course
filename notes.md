# Cours de Symfony en autodidacte

## Commençons par commencer

**Le protocole http** = langage de com entre client et serveur sur le web.

**PHP** = langage orienté serveur. Écoute la requête du client et leurs retourne une réponse
  Ex :
``$_GET ``
*Récupère les éléments passés à une URL
Permet de construire une page HTML en fonction de la requête de l'utilisateur*


### Symfony

**Le composant HttpFoundation**
- Request
Donne une abstraction de type objet permettant de manipuler tous les elmts reçus de la requête
  Ex :
`` $request->get('name'!);``
*permet de récupérer le paramètre 'name' passé dans l'URL de l'utilisateur*

- Response
Construit une adresse Http valide et la retourne à l'utilisateur
  Ex :
``$reponse->setStatusCode(Response::HTTP_NOT_FOUND);``
*permet de définir qu'une page n'a pas été trouvée*


### Comment ça se passe dans le framework ?

Dans le framework, nous allons créer des contrôleurs (dans App Controller)
Ces contrôleurs "étendent" généralement la classe AbstractController du framework.

Dans le contrôleur AbstractController nous allons trouver des fonctions avec des annotations particulières

**Annotation n°1**
``@Route("/home", name="accueil")`` nous permet d'indiquer au framework que la fonction est mappée à l'URL de la page /home.
Si l'utilisateur accède à la page d'accueil sur cette appli Symfony, c'est cette fonction qui va s'exécuter.

Ces fonctions, dans les contrôleurs n'ont qu'une seule responsabilité : retourner un objet de type Response

**Annotation n°2**
``@Route("/article/{postId}", name="article")``

Exemple de pattern/wild card
``{postId}`` = expression régulière
>bref, c'est une variable qu'on va retrouver dans la BDD grâce à l'ID de l'article souhaité

### Ceci dit, en Symfony, où est donc le Front Controller ?
Ce n'est assurément pas dans nos contrôleurs Symfony que nous allons mapper chaque URL,aller récupérer chaque information de la requête, et retourner une réponse. D'zilleurs on ne manipule pas la requête directement puisque ces "commandes" sont sous forme d'annotations, de commentaires.

En fait, cette responsabilité est portée par un fichier qui est disponible dans le dossier public, au sein du fichier index.php.

La responsabilité d'être un Front Controller dans Symfony est portée par l'objet Kernel du composant HttpKernel.
>Et là... Je suis perdue ! #PERII C'est quoi ce Kernel ? Mystère et boule de chewin'gum

Cet objet a donc la responsabilité de gérer la requête et de retourner une réponse aux utilisateurs.

## Comment Symphony retourne une réponse à l'utilisateur ?
*Symfony est un framework basé sur le processus HTTP requête/réponse*

### Requêtes et réponses (HTTP)

#### Un utilisateur envoie une requête au serveur...
 le rôle d'un serveur web est toujours de retourner une réponse à l'utilisateur

 En "langage" HTTP, voici l'équivalent de la requête d'un utilisateur qui accéderait au site d'OpenClassrooms :

- ``GET / HTTP/1.1`` : contient la méthode HTTP (ici "GET") ainsi que l’URL (ici "/")
- ``Host: marie-helenerots.com`` : hôte est marie-helenerots.com
- ``Accept: text/html`` : contient le type de contenu attendu, ici du HTML
- ``User-Agent: Mozilla/5.0 (Macintosh)`` : informe le serveur du navigateur utilisé (ici, Mozilla Firefox sur Mac OSX).

Il existe plusieurs méthodes HTTP pour accéder à une ressource, voici les plus importantes : **GET**, **POST**, **PUT** et **DELETE**.

#### ... et le serveur retourne une réponses
Une fois que le serveur sait exactement quelle ressource l'utilisateur/client souhaite et sous quelle forme, il peut retourner ce résultat sous forme de réponse HTTP.


- ``HTTP/1.1 200 OK`` : indique le code de statut HTTP
- ``Date: Sat, 28 Jul 2018 21:05:05 GMT``
- ``Server: cloudfare``
- ``Content-Type: text/html``
- ``<html>
 <!-- ... HTML de la page d'accueil -->
</html>``

 De très nombreux statuts existent, parmi les plus connus :
- **200**, la page a été retournée sans erreur du serveur ;
- **404**, le code HTTP pour une ressource qui n'a pas été retrouvée sur le serveur ;
- **les codes 3XX**, qui signalent les redirections de ressources ;
- **les codes 4XX**, qui signalent une erreur côté utilisateur/client ;
- **les codes 5XX**, qui signalent une erreur côté serveur.

#### Manipuler la couche HTTP en PHP
PHP = langage de programmation serveur pour le web,
Ex : le code suivant récupère des informations de la requête utilisateur et retourne une réponse au format HTML :

``<?php

// en utilisant l'url localhost?name=Zozor
$name = $_GET['name'];

header('Content-Type: text/html');
echo '<html>';
echo '<body>Bonjour '. $name . '</body>';
echo '</html>'
``

Ce "serveur" PHP retournerait la réponse HTTP suivante :

- ``Date: Sat, 28 Jul 2018 02:14:33 GMT``
- ``Server: Apache/2.2.17 (Unix)``
- ``Content-Type: text/html``
- ``<html><body>Bonjour Zozor</body></html>``

#### Requêtes & Réponses en symfony
> CF ce que j'ai écrit en introooooooo, ne nous répétons paaaas !

### Lier une URL à une abstraction
Nous avons besoin d'un contrôleur "front" ou frontal : il est en charge de récupérer les infos de la requête et d'exécuter l'action correspondante qui retournera une réponse.

Au plus simple, un contrôleur front pourrait être implémenté de cette façon :

``<?php
// index.php
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

$request = Request::createFromGlobals();
$url = $request->getPathInfo();
$response = new Response();

switch($url) {
    case '/':
        $response->setContent('Accueil');
        break;
    case '/admin':
        $response->setContent('Accès Espace Admin');
        break;
    default:
        $response->setStatusCode(Response::HTTP_NOT_FOUND);
}

$response->send();``

> Bref, il paraît que même si ça paraît très limité, Symfony fournit un contrôleur frontal beaucoup plus puissant et extensible

#### La gestion du "routing" dans Symfony
> Arrête de vouloir écrire Symfony avec "ph" au lieu de "f" ! Tu vas finir par t'en taper sur les doigts !
