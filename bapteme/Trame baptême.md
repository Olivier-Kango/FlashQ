# Découverte de PostgreSQL

Bonjour et bienvenue les amis ! Je me présente... (se présenter en 1 minutes max) 

On va découvrir aujourd'hui le monde fabuleux des bases de données ! Alors accrochez-vous bien, on vous a préparé de supers petits exercices pour ce premier cours !

**J'espère qu'on va faire du bon travail ensemble !** (très important de dire ça !)


Avant de commencer, je vais vous présenter le plan de notre cours :

(plan de cours dispo sur la slide n°2)

A l'issue de ce cours, vous serez capable de :

(voir slide n°3)


## Contexte

Commençons par rappeler le contexte.

Vous avez déjà pu découvrir les bases du frontend, avec notamment :

- HTML => pour créer la structure de notre site
- CSS => pour ajouter du style et des couleurs à notre page
- JS => pour ajouter du comportement

Et un peu de backend avec :

- Node.js => exécuter du code js sur votre machine
- Express => Pour créer un server http et communiquer avec le frontend 

Ok, c'est génial, mais, il manque quelque chose vous ne trouvez pas ? ...

Imaginons un peu... Vous avez créé le nouveau réaseau social qui fait fureur chez les amateurs de Marvel...

Problème : Comment enregistrer les identifiants d'un utilisateur ? Comment enregistrer les posts de tous les fans ? A votre avis ? (laisser les étudiants répondre dans le chat)

Il va bien falloir les enregistrer quelque part...

Solution : On va les stocker dans une base de données ! Et oui !


## Notions sur les bases de données

Mais alors c'est quoi une base de données ?

Une base de données, c'est un moyen de stocker des données de façon permanente (C'est ce qu'on appelle la **persistance des données**).

C'est un peu comme une bibliothèque. On peut ranger des livres dedans. Chaque livre possède un titre, un auteur, un sujet, etc... Ce sont les attributs de ce livre. On peut trier notre bibliothèque par auteur, par ordre alphabétique, par sujet, etc...

Et bien comme cette bilbiothèque, dans une base de données, on peut ranger des **enregistrements**. Chaque enregistrement peut posséder des attributs, et on peut trier ces enregistrements de la manière qui nous arrange !

On va prendre un exemple juste après pour bien illustrer.

Mais avant, il faut savoir qu'il existe plusieurs Systèmes de Gestion de Base de Données (SGBD) : MySQL, Microsoft SQL Server, Oracle Database... Il y en a pleins !

Il y a différents types de BDD mais nous on va surtout s'intéresser aux bases de données dites **relationnelles** pour l'instant !

Ces BDD relationnelles utilisent toutes un langage commun : le SQL (Structured Query Language)

Pas besoin d'apprendre ce langage par coeur, on va s'y habituer petit à petit a force de l'utiliser, et puis on pourra toujours retrouver sur les documentations (ou Google) quand on a un doute ;)

Ces BDD utilisent une structure dite "en tables". C'est un peu comme des tableaux Excel. Voici un exemple :

(exemple sur la slide, la table utilisateurs)

On voit qu'il y a des colonnes, et des lignes. Ici, il s'agit d'une liste d'utilisateurs, avec leur nom, prénom, age et boisson favorite. On s'intéressera à la colonne id plus tard ;)


Allez, regardons comment on peut récupérer des données dans cette table avec le langage SQL !

(on passe a la diapo suivante)

Si on veut pouvoir récupérer tous les enregistrements (donc toutes les lignes) de notre tableau, on va faire :

`SELECT * FROM "utilisateurs";`

L'étoile sert à dire, je veux récupérer toutes les colonnes pour chaque ligne. On obtiendra donc une liste de tous les utilisateurs présents dans cette table

On pourrait filtrer les utilisateurs qu'on veut trouver, par exemple en fonction de leur id (qui pourrait être un identifiant utilisateur par exemple, comme un numéro étudiant ou autre...)

`SELECT * FROM "utilisateurs" WHERE "id" = 1;`

Ici, on récupèrera uniquement les utilisateurs qui possèdent un id égal à 1, en l'occurence, Monsieur Jean DUPONT.

Allez une dernière pour la route !

`SELECT * FROM "utilisateurs" WHERE "age" < 35;`  

A votre avis, ici on récupèrerait quoi ? (laisser les étudiants répondre)

Et oui ! Tous les utilisateurs qui ont un age strictement inférieur à 35 ! 


## Notre première requête SQL

Okay excellent ! On va jouer à un petit jeu !

(Présenter le jeu dans la slide 7 puis passer à la slide 8)

Let's code together !


(utiliser cette requête pour insérer les blagues dans une bdd)
(Le but du jeu sera de retrouver ces blagues et les découvrir en live devant les étudiants)
```
INSERT INTO blagues (categorie, question, reponse) 
VALUES 
('Humour noir', 'Pourquoi les plongeurs plongent-ils toujours en arrière et jamais en avant ?', 'Parce que sinon ils tombent dans le bateau !'),
('Mathématiques', 'Pourquoi les mathématiques sont-elles stressantes ?', 'Parce qu\'elles ont trop de problèmes !'),
('Animaux', 'Pourquoi les crocodiles sont-ils de mauvais arbitres ?', 'Parce qu\'ils ont toujours la bouche pleine !'),
('Nourriture', 'Pourquoi les tomates rougissent-elles ?', 'Parce qu\'elles voient la salade qui se déshabille !'),
('Sport', 'Pourquoi les footballeurs ne mangent-ils pas avant les matchs ?', 'Parce qu\'ils préfèrent jouer à jeun !'),
('Informatique', 'Pourquoi les programmeurs préfèrent-ils le noir ?', 'Parce que la lumière attire les bugs !');
```


### Première étape : Se connecter à notre BDD 

Allez, on initialise notre nouveau projet ! un petit coup de npm init ! On donne des caractéristiques à notre projet (nom, version, ...)

On a déjà installé PostgreSQL sur vos machines. Mais Node.js ne sait pas parler avec PostgreSQL tout seul. Il va falloir lui apprendre !

Pour ça, on va utiliser un module qui s'appelle 'pg'. Il va être comme la glue entre notre application et notre BDD. C'est ce qui va permettre à notre application de communiquer avec notre SGBD !

Qui dit module dit installer ce module. Comment on fait déjà ? Vous vous en souvenez ? (laisser les étudiants répondre)

Eh oui ! `npm install pg`

Okay, on a maintenant tout ce qu'il faut ! Ce module c'est un peu comme une télécommande. En appuyant sur un bouton, on lance une fonctionnalité sur notre télévision.

Mais avant, il faut connecter la télécommande à la télé !

Il va donc falloir lire le manuel (RTFM !)

(https://node-postgres.com/)[https://node-postgres.com/]

Allez, on met en place :D

```
const { Client } = require('pg'); // On importe l'outil Client à partir du module pg 
const client = new Client({ // Ici, le mot clé new est nouveau pour les étudiants, on leur dit qu'ils découvriront sa signification dans le cours sur la POO
    host: "localhost",
    user: "postgres",
    port: "5432",
    password: "password123",
    database: "blagues"
});

client.connect(); // Pas d'async pour l'instant, on ne complique pas l'exercice

// Okay donc ici on est connecté à notre BDD. On va maintenant essayer de faire notre première requête SQL !

const result = client.query('SELECT * FROM "blagues";'); // On fait exprès de ne pas utiliser de callback ou d'async ici !
console.log(result); // Promise <pending>  <= Qu'est-ce que c'est que ce truc ?!

// C'est bizarre ça, pourtant on devrait récupérer les données dans la table blagues... 
// C'est là qu'on va devoir parler un peu du concept d'asynchronicité !

```


#### Concept d'asynchronicité 

Javascript est un language un peu spécial ! Il nous permet de mettre en attente des choses, le temps qu'elles soient terminées.

Pour illustrer, imaginez que vous alliez à la laverie. Vous déposez vos habits, et vous vous installez sur un chaise le temps que vos habits soient nettoyés. C'est ce qu'on appelle une fonction synchrone ! C'est à dire que vous êtes restés à attendre que l'action soit terminée pour passer à autre chose !

Maintenant imaginez que vous demandiez à quelqu'un de déposer vos habits à la laverie, et d'attendre que les habits soient propre. Puis une fois qu'ils sont lavés, cette personne vous les apporte chez vous. C'est ce qu'on appelle une fonction asynchrone. C'est à dire que pendant que cette tâche s'execute, vous pouvez faire autre chose, par exemple, regarder la nouvelle série sur Netflims ou jouer à la Playbox.

En JS, c'est pareil :

- Il y a des fonctions synchrones, qui bloquent le code le temps qu'elles soient terminées
- Il y a des fonctions asynchrones, qui ne bloquent pas le code pendant qu'elles s'exécutent !

Un petit exemple pour mieux comprendre :

```
console.log("1- Je me réveille le matin");

console.log("2- Je prend mon petit déjeuner");

setTimeout(() => {
  console.log("3- J'écoute un podcast");
}, "1000")

console.log("4- Je travaille");


/* Output : à votre avis ? dans quel ordre vont sortir ces logs ?

1- Je me réveille le matin 
2- Je prend mon petit déjeuner
4- Je travaille
3- J'écoute un podcast

*/

``` 

On voie bien que les instructions n'arrivent pas dans l'ordre d'écriture du code, mais bien dans l'ordre d'exécution !

#### Mais quel rapport avec la fonction client.query ? commment récupérer son résultat ?

Bonne question !

On va devoir indiquer à Node qu'il faut attendre de récupérer le résultat avant de l'afficher !

Pour faire ça, on va utiliser une méthode spéciale qui s'appelle `.then()`

```
client.query('SELECT * FROM "blagues";')
	.then((result) => {
		console.log(result);
		})
```

On voit bien qu'on a maintenant récupéré quelque chose. Mais on a beaucoup d'informations qui ne sont pas des blagues... 

Si on scroll vers le haut, on peut voir qu'on recoit un objet qui possède une propriété "rows" : ce sont les lignes de notre tableau. Ah et tiens... Ca ressemble a des blagues ça...

```
client.query('SELECT * FROM "blagues";')
	.then((result) => {
		console.log(result.rows);
		})
```

On a bien récupéré nos blagues ! Génial, on va pouvoir les lire maintenant !

Bon, et bien maintenant vous êtes capables de vous connecter à une BDD et à récupérer des informations stockées dans une table. Génial !

Je vous propose qu'on fasse une petite pause pour se dégourdir les jambes, et on va attaquer avec d'autres types de requêtes. Bah oui, on aimerait bien ajouter nos propres blagues maintenant !


















