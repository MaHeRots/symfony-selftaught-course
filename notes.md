#Cours de Symfony en autodidacte

##Commençons par commencer

**Le protocole http** = langage de com entre client et serveur sur le web.

**PHP** = langage orienté serveur. Écoute la requête du client et leurs retourne une réponse
  Ex :
``$_GET ``
>Récupère les éléments passés à une URL
Permet de construire une page HTML en fonction de la requête de l'utilisateur


###Symfony

**Le composant HttpFoundation**
- Request
Donne une abstraction de type objet permettant de manipuler tous les elmts reçus de la requête
  Ex :
`` $request->get('name'!);``
> permet de récupérer le paramètre 'name' passé dans l'URL de l'utilisateur

- Response
Construit une adresse Http valide et la retourne à l'utilisateur
  Ex :
``$reponse->setStatusCode(Response::HTTP_NOT_FOUND);``
>permet de définir qu'une page n'a pas été trouvée


###Comment ça se passe dans le framework ?

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

###Ceci dit, en Symfony, où est donc le Front Controller ?
Ce n'est assurément pas dans nos contrôleurs Symfony que nous allons mapper chaque URL,aller récupérer chaque information de la requête, et retourner une réponse. D'zilleurs on ne manipule pas la requête directement puisque ces "commandes" sont sous forme d'annotations, de commentaires.

En fait, cette responsabilité est portée par un fichier qui est disponible dans le dossier public, au sein du fichier index.php.

La responsabilité d'être un Front Controller dans Symfony est portée par l'objet Kernel du composant HttpKernel.
>Et là... Je suis perdue ! #PERII C'est quoi ce Kernel ? Mystère et boule de chewin'gum

Cet objet a donc la responsabilité de gérer la requête et de retourner une réponse aux utilisateurs.

##Comment Symphony retourne une réponse à l'utilisateur ?
*Symfony est un framework basé sur le processus HTTP requête/réponse*

###Requêtes et réponses (HTTP)
