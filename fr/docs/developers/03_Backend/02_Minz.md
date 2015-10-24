FreshRSS repose sur le framework PHP Minz. Celui-ci accuse quelques défauts de jeunesse et un manque de documentation flagrant. Nous essayons ici de documenter les grandes lignes de Minz.

Comme bon nombre de framework PHP, Minz repose sur le patron de conception MVC (Modèle Vue Contrôleur). Ce "design pattern" assure l'architecture globale du logiciel en séparant clairement la couche métier de la logique et de l'interface utilisateur.

# Modèles

## Concept

Les modèles représentent la couche métier d'une application. C'est la couche la plus basse qu'on puisse trouver dans un logiciel basé sur l'architecture MVC. Elle est chargée de traiter les données.

Les modèles se trouvent dans le répertoire `./app/Models` et héritent de `Minz_Model`.

Dans FreshRSS, nous allons trouver trois principaux modèles : `Entry` (qui correspond à un article), `Feed` (représentant les flux RSS) et `Category` (les catégories dans lesquelles sont classés les flux RSS). Ces modèles seront étudiés en détails plus loin dans la documentation.

Minz met à disposition deux DAO différents : `Minz_ModelPdo` (pour les accès à la base de données) et `Minz_ModelArray` (permettant de stocker des tableaux PHP dans des fichiers).

## Modèle DAO

Les modèles « Data Access Object » (DAO) sont des modèles spécifiques dans le sens où ce sont eux qui vont gérer les accès au(x) système(s) de stockage (base de données, système de fichiers, etc.). Chaque modèle devant persister dans le temps va se voir attribuer un équivalent DAO qui se charge de traduire, par exemple, les objets `Entry` (`EntryDAO`) en objets pouvant être traités par la base de données.

# Contrôleurs et actions

Afin de lier les modèles entre eux et traiter la logique de l'application, on va faire appel aux contrôleurs. Typiquement, un contrôleur spécifique va se charger de gérer (lister, créer, modifier, supprimer) les modèles. Ils sont chargés de recevoir les données entrées par l'utilisateur (dans une vue), les nettoyer et les traiter pour exécuter l'action demandée.

Un contrôleur doit hériter de `Minz_ActionController` et est stocké dans `./app/Controllers`. Son nom doit commencer par `FreshRSS_` et se terminer par `_Controller` (par exemple, `FreshRSS_category_Controller`).

Prenons la création d'une catégorie en exemple : le but est de demander à l'utilisateur le nom d'une catégorie et de l'ajouter en base de données. Cela va se passer ainsi :

1. Réception du nom de la catégorie par la vue ;
2. Traitement pour nettoyer le nom : caractères invalides, nom trop long, etc. (`Category`) ;
3. Vérification en base de données que le nom n'existe pas déjà (`CategoryDAO`) ;
4. Insertion en base de données (grâce à `CategoryDAO`) ;
5. Renvoie vers la page de création de catégories avec un message indiquant le résultat de l'insertion (en fonction du résultat retourné par la méthode d'insertion).

Un contrôleur est thématique, c'est-à-dire qu'il va se charger de ne traiter qu'un certain type de données. Ainsi, souvent on va trouver un contrôleur pour chaque modèle (mais ce n'est pas une règle absolue). Afin de séparer les différents traitements, on va les séparer dans des actions (par exemple, `createAction`, `updateAction` ou `deleteAction`). Chaque action (méthode PHP dans le contrôleur) doit se terminer par `Action`.

# Vues

Une vue correspond à ce qui est affiché à l'utilisateur final. Typiquement, l'affichage de la liste des articles ou le formulaire de création d'une catégorie.

Les vues sont des fichiers `.phtml` (mélange de PHP et HTML) situés dans `./app/views`.

Les vues sont intrinséquement liées aux contrôleurs dans le sens où chaque action va disposer d'une vue associée. La vue associée à l'action `createAction` du contrôleur `FreshRSS_category_Controller` se trouvera dans `./app/views/category/create.phtml`.

# Routage

**TODO**

# Écriture des URL

**TODO**

# Internationalisation

**TODO**

# Comprendres les mécanismes internes

**TODO**
