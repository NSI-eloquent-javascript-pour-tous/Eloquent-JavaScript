{{meta {load_files: ["code/chapter/07_robot.js", "code/animatevillage.js"], zip: html}}}

# Projet: Un Robot

{{quote {author: "Edsger Dijkstra", title: "The Threats to Computing Science", chapter: true}

[...] the question of whether Machines Can Think [...] is about as
relevant as the question of whether Submarines Can Swim.

quote}}

{{index "artificial intelligence", "Dijkstra, Edsger"}}

{{figure {url: "img/chapter_picture_7.jpg", alt: "Picture of a package-delivery robot", chapter: framed}}}

{{index "project chapter", "reading code", "writing code"}}

Dans les chapitres à «projet», je cesserai de vous importuner avec
de nouvelles théories pour quelque temps et, à la place, nous travaillerons
ensemble sur un programme. La théorie est nécessaire pour apprendre à
programmer, mais lire et comprendre des programmes plus réalistes est tout
aussi important.

Notre projet dans ce chapitre sera de construire un ((automate)),
un petit programme qui réalise une tâche dans un ((monde virtuel)).
Notre automate sera un ((robot)) livreur qui récupère et livre des colis.

## Meadowfield

{{index "roads array"}}

Le village de ((Meadowfield)) n'est pas bien grand. On y trouve
11 lieux reliés par 14 routes. On peut le décrire avec ce
tableau de routes:

```{includeCode: true}
const routes = [
  "Maison d'Alice-Maison de Bob",   "Maison d'Alice-Cabane",
  "Maison d'Alice-Poste",           "Maison de Bob-Mairie",
  "Maison de Daria-Maison d'Ernie", "Maison de Daria-Mairie",
  "Maison d'Ernie-Maison de Grete", "Maison de Grete-Ferme",
  "Maison de Grete-Magasin",        "Marché-Ferme",
  "Marché-Poste",                   "Marché-Magasin",
  "Marché-Mairie",                  "Magasin-Mairie"
];
```

{{figure {url: "img/village2x.png", alt: "The village of Meadowfield"}}}

Le réseau de routes du village forme un _((graphe))_. Un graphe est
une collection de sommets (lieux dans le village) connectés (ou non)
par des arêtes (routes). Ce graphe sera l'univers dans lequel notre
robot va évoluer.

{{index "roadGraph object"}}

Travailler avec notre tableau de chaînes n'est pas pratique. En effet,
ce qui nous intéresse ce sont les destinations accessibles depuis un
lieu donné. Aussi, nous allons convertir notre liste de routes en une
structure de données qui, pour chaque lieu, va nous dire ceux
que nous pouvons directement atteindre depuis lui.

```{includeCode: true}
function construireGraphe(aretes) {
  let graphe = Object.create(null);
  function ajouterArete(debut, fin) {
    if (graphe[debut] == null) {
      graphe[debut] = [fin];
    } else {
      graphe[debut].push(fin);
    }
  }
  for (let [debut, fin] of aretes.map(r => r.split("-"))) {
    ajouterArete(debut, fin);
    ajouterArete(fin, debut);
  }
  return graphe;
}

const grapheRoutes = construireGraphe(routes);
```

Étant donné un tableau d'arêtes (les routes), `construireGraphe` crée un tableau
associatif qui, à chaque sommet (lieu), associe le tableau des sommets directement
accessibles depuis lui.

{{index "split method"}}

Elle utilise la méthode `split` pour transformer des chaînes de routes de
la forme `"Début-Fin"` en un tableau à deux éléments contenant
début et fin comme chaînes distinctes.

## La mission

Notre ((robot)) devra se déplacer dans le village. Il y a des colis situés
à différents endroits et chacun d'eux possède l'adresse d'un autre lieu.
Le robot doit ramasser les colis lorsqu'il arrive à un endroit où il
y en a, et doit les livrer à l'adresse adéquate lorsqu'il arrive à destination.

L'automate doit décider, à chaque fois qu'il se trouve à un endroit, où
aller ensuite. Sa mission s'achève lorsque tous les colis ont été livrés.

{{index simulation, "virtual world"}}

Pour être en mesure de simuler ce processus, nous devons définir
un monde virtuel susceptible de le décrire. Ce modèle devra indiquer
la position du robot et celles des colis. Lorsque le robot décide
de se déplacer, il faudra mettre à jour le modèle de façon à ce qu'il
continue de refléter la situation.

{{index [state, in objects]}}

Si vous réfléchissez à tout ceci en pensant en terme de ((programmation
orientée objet)), vous pourriez être tenté de commencer à définir des
objets pour chaque élément de ce monde virtuel: une ((classe)) pour
le robot, une autre pour les colis, peut-être encore une autre pour
chaque lieu. Les objets correspondant pourraient alors avoir des
propriétés pour décrire leur ((état)) courant, comme une pile de colis
en un lieu, laquelle pourrait évoluer au moment de la mise à jour du
monde virtuel.

Ce n'est pas la bonne façon de procéder. Tout du moins, la plupart du temps.

Le fait que quelque chose ressemble à un objet ne signifie pas pour autant
qu'il doive être un objet dans votre programme.
Écrire des classes, de proche en proche, pour chaque
concept de votre application finit par vous laisser avec une collection
d'objets interconnectés qui ont chacun leur propre état interne à faire
évoluer correctement. De tels programmes sont souvent difficiles à comprendre
et donc simples à casser. 

{{index [state, in objects]}}

Au lieu de cela, nous allons condenser l'état du village en utilisant le plus
petit ensemble de valeurs qui le définit. Il y a la position courante
du robot et l'ensemble des colis qui n'ont pas encore été livrés, chacun
desquels a une position ainsi qu'une adresse de destination. C'est tout.

{{index "VillageState class", "persistent data structure"}}

Et, pendant que nous y sommes, nous ferons en sorte de ne pas avoir à
_modifier_ cet état lorsque le robot se déplace, mais plutôt d'en
calculer un _nouveau_ au cours de ce déplacement.

```{includeCode: true}
class EtatVillage {
  constructor(lieu, colis) {
    this.lieu = lieu;
    this.colis = colis;
  }

  deplacer(destination) {
    if (!grapheRoutes[this.lieu].includes(destination)) {
      return this;
    } else {
      let colis = this.colis.map(c => {
        if (c.lieu != this.lieu) return c;
        return {lieu: destination, adresse: c.adresse};
      }).filter(c => c.lieu != c.adresse);
      return new EtatVillage(destination, colis);
    }
  }
}
```

La méthode `déplacer` définit l'action principale. Elle commence
par vérifier s'il y a une route qui mène de la position courante
à la destination fournie, et sinon, renvoie l'ancien état puisque
le déplacement n'est pas valide.

{{index "map method", "filter method"}}

Ensuite, elle crée un nouvel état où la destination correspond à la
nouvelle position du robot. Mais elle doit aussi créer un nouveau
tableau pour les colis en cours de livraison—ceux que le robot transporte
(qui étaient situés à la position avant déplacement) doivent être
déplacés à la nouvelle position. Et les colis, dont l'adresse correspond
à cette nouvelle position, doivent être livrés—c'est-à-dire supprimés
des colis qui n'ont pas encore été livrés. L'invocation de `map` s'occupe
du déplacement et celle de `filter` de la livraison.

Les objets colis ne sont pas modifiés lorsqu'on les déplace, mais recréés.
La méthode `deplacer` finit par renvoyer le nouvel état du village après déplacement,
sans pour autant modifier l'ancien qui reste intact.

```
let premier = new EtatVillage(
  "Poste",
  [{lieu: "Poste", adresse: "Maison d'Alice"}]
);
let suivant = premier.deplacer("Maison d'Alice");

console.log(suivant.lieu);
// → Maison d'Alice
console.log(suivant.colis);
// → []
console.log(premier.lieu);
// → Poste
```

Le colis est livré suite au déplacement, ce qui se reflète dans l'état
suivant. Mais l'état initial continue à décrire la situtation où
le robot est situé à la poste et le colis n'est pas encore livré.

## Persistance des données

{{index "persistent data structure", mutability, ["data structure", immutable]}}

Les structures de données qui restent identiques à elles-mêmes sont dites
_((immuables))_ ou _persistantes_. Elles se comportent similairement aux
chaînes et aux nombres en ce sens qu'elles sont ce qu'elles sont et
demeurent ainsi, plutôt que de contenir des choses différentes à des
instants différents.

En JavaScript, à peu près tout peut être modifié, et donc travailler
avec des valeurs qui sont supposées être persistantes nécessite d'être
vigilant. On dispose d'une fonction nommée `Object.freeze` qui modifie
un objet de façon que la modification de ses propriétés soit ignorée.
On peut utiliser cela, si on veut être prudent, pour s'assurer que nos
objets ne puissent être modifiés. Cependant, «geler» un objet demande
un travail supplémentaire à l'ordinateur et voir des modifications
ignorées risque tout autant d'embrouiller une personne que de la pousser
à faire des choses incorrectes. Pour cela, je préfère la plupart du temps
signaler aux autres qu'il vaut mieux ne pas faire de bêtises avec un certain
objet tout en espérant qu'ils s'en souviennent.

```
let objet = Object.freeze({valeur: 5});
objet.valeur = 10;
console.log(objet.valeur);
// → 5
```

Pourquoi est-ce que je m'évertue à ne pas modifier certains
objets alors que le langage s'attend à ce que je le fasse?

Parce que ça m'aide à mieux comprendre mes propres programmes. C'est
encore en rapport avec la gestion de la complexité. Lorsque les objets de mon
système sont des choses stables et fixées, je peux réfléchir aux opérations
qui portent dessus de façon isolée—se déplacer jusqu'à la maison d'Alice
en partant d'un état inital produit toujours le même nouvel état. Lorsque
les objets sont modifiés au cours du temps, cela ajoute une dimension supplémentaire
de complexité à ce genre de raisonnement.

Pour un petit système, comme celui que nous construisons
dans ce chapitre, nous pourrions gérer cette complexité supplémentaire.
Mais la limite la plus importante sur ce que nous sommes capables de construire
tient à notre capacité à comprendre cette construction. Tout ce qui
rend votre code plus facile à comprendre contribue à la possibilité
de réaliser un système encore plus ambitieux.

Malheureusement, bien qu'un système construit sur la base de données
persistantes soit plus facile à comprendre, en _concevoir_ un, particulièremnt
lorsque votre langage de programmation n'aide pas, peut être un peu plus
difficile. Nous serons attentifs aux opportunités d'utilisation
de données persistantes dans ce livre, mais il nous arrivera aussi d'utiliser
des données muables.

## Simulation

{{index simulation, "virtual world"}}

Un robot livreur observe son environnent et décide de la direction
à prendre pour se déplacer. Ainsi, nous pourrions dire que notre
robot peut être assimilé à une fonction qui prend un objet `EtatVillage` et renvoie
le nom d'une destination accessible.

{{index "runRobot function"}}

Comme nous souhaitons aussi que les robots puissent se souvenir
de certaines choses pour concevoir et exécuter des plans, nous leur
transmettrons leur mémoire et leur permettrons de la renvoyer après qu'elle
aura été rafraîchie. Ainsi, un robot renvoie un objet qui contient à la fois
la direction qu'il a décidé de prendre et la nouvelle valeur de sa mémoire
que nous lui transmettrons à nouveau la prochaine fois que nous l'invoquerons.

```{includeCode: true}
function lancerRobot(etat, robot, memoire) {
  for (let etapes = 0;; etapes++) {
    if (etat.colis.length == 0) {
      console.log(`Mission accomplie en ${etapes} étapes`);
      break;
    }
    let action = robot(etat, memoire);
    etat = etat.deplacer(action.direction);
    memoire = action.memoire;
    console.log(`Déplacement vers ${action.direction}`);
  }
}
```

Réflechissez à ce qu'un robot doit faire pour prendre une décision
pour un état donné. Il va devoir ramasser tous les colis en visitant
tous les lieux où il y en a, puis il va devoir les livrer en se rendant
à tous les endroits auxquels les colis sont adressés, mais seulement
après avoir récupéré un tel colis.

Quelle est la stratégie la plus bête qui pourrait fonctionner? Le robot
pourrait simplement se déplacer au hasard à chaque étape. Cela signifie,
avec une grande probabilité, qu'il va éventuellement découvrir tous
les colis et aussi, qu'à un certain point, il va atteindre toutes
les destinations de livraison.

{{index "randomPick function", "randomRobot function"}}

Voilà à quoi cela pourrait ressembler:

```{includeCode: true}
function elementAuHasard(tableau) {
  let choix = Math.floor(Math.random() * tableau.length);
  return tableau[choix];
}

function robotAleatoire(etat) {
  return {direction: elementAuHasard(grapheRoutes[etat.lieu])};
}
```

{{index "Math.random function", "Math.floor function", [array, "random element"]}}

Souvenez-vous que `Math.random()` renvoie un nombre entre 0 et 1—mais toujours
strictement inférieur à 1. Multiplier un tel nombre par la longueur
d'un tableau puis, appliquer `Math.floor` au résultat, nous donne un index aléatoire
pour le tableau.

Puisque ce robot n'a pas besoin de se souvenir de quoi que ce soit,
il ignore son deuxième argument (rappelez-vous que les fonctions
JavaScript peuvent être appelées avec des arguments supplémentaires
sans que cela n'ait d'effet) et omet la propriété `memoire` de l'objet
qu'il renvoie.

Pour mettre ce robot sophistiqué au travail, nous aurons tout d'abord
besoin d'un moyen pour créer un nouvel état avec quelques colis.
Une méthode statique de la classe `EtatVillage` (qu'on ajoute ici
directement comme propriété du constructeur) est une bonne façon
de faire cela.

```{includeCode: true}
EtatVillage.aleatoire = function(nbColis = 5) {
  let colis = [];
  for (let i = 0; i < nbColis; i++) {
    let adresse = elementAuHasard(Object.keys(grapheRoutes));
    let lieu;
    do {
      lieu = elementAuHasard(Object.keys(grapheRoutes));
    } while (lieu == adresse);
    colis.push({lieu, adresse});
  }
  return new EtatVillage("Poste", colis);
};
```

{{index "do loop"}}

Nous ne voulons pas qu'un colis ait pour adresse sa position initiale.
Pour cette raison, la boucle `do` recommence à tirer des positions
au hasard lorsque la position et l'adresse de destination du colis
coïncident.

En avant, démarrons ce monde virtuel.

```{test: no}
lancerRobot(EtatVillage.aleatoire(), robotAleatoire);
// → Déplacement vers Magasin
// → Déplacement vers Maison de Grete
// → …
// → Mission accomplie en 80 étapes
```

Ce robot effectue beaucoup de déplacements pour livrer tous les colis
car il ne plannifie pas son action.  Nous améliorerons cela bientôt.

{{if interactive

Pour disposer d'une perspective plus agréable sur cette simulation,
vous pouvez utiliser la fonction `lancerAnimationRobot` qui est disponible
dans [l'environnement de programmation de ce chapitre](https://eloquentjavascript.net/code/#7).
Cela lance la simulation qui, au lieu d'afficher du texte, montre
le robot évoluer sur la carte du village.

```{test: no}
lancerAnimationRobot(EtatVillage.aleatoire(), robotAleatoire);
```

La façon dont la fonction `lancerAnimationRobot` est réalisée restera
un mystère pour le moment. Mais après avoir lu des [chapitres à venir](dom)
du livre qui discutent de l'intégration de JavaScript dans les navigateurs
web, vous serez en mesure de comprendre son fonctionnement.

if}}

## La tournée de livraison

{{index "mailRoute array"}}

Nous devrions être capables d'améliorer significativement ce ((robot)) aléatoire.
Une façon simple de le faire est de s'inspirer du fonctionnement des
services postaux. Si nous trouvons une route qui passe par tous les lieux
du village, le robot pourrait la suivre deux fois de suite, ce qui garantirait
que toutes les livraisons ont eu lieu. Dans notre cas la route suivante
qui démarre de la poste fera bien l'affaire.

```{includeCode: true}
const routeDistribution = [
  "Maison d'Alice", "Cabane", "Maison d'Alice", "Maison de Bob",
  "Mairie", "Maison de Daria", "Maison d'Ernie",
  "Maison de Grete", "Magasin", "Maison de Grete", "Ferme",
  "Marché", "Poste"
];
```

{{index "routeRobot function"}}

Pour implémenter ce robot suiveur de route, nous aurons besoin d'utiliser
sa mémoire. Le robot conserve les emplacements qu'il lui reste à visiter
et se débarasse du premier d'entre eux à chaque étape.

```{includeCode: true}
function robotRoute(etat, memoire) {
  if (memoire.length == 0) {
    memoire = routeDistribution;
  }
  return {direction: memoire[0], memoire: memoire.slice(1)};
}
```

Ce robot est déjà beaucoup plus rapide. Il lui faudra au maximum
26 étapes (le double d'une route en 13 étapes) et souvent moins.

{{if interactive

```{test: no}
lancerAnimationRobot(EtatVillage.aleatoire(), robotRoute, []);
```

if}}

## Recherche de chemin

Mais on ne peut pas qualifier de comportement intelligent le fait
de suivre aveuglément une route fixée. Le robot pourrait travailler
plus efficacement en ajustant son comportement par rapport à ce qu'il
lui reste à faire.

{{index pathfinding}}

Pour y parvenir, il doit être capable de choisir de se déplacer
vers un endroit pour collecter un colis ou pour le livrer. Cela
demande, même si le but nécessite plus d'un déplacement, d'être
capable de faire une sorte de recherche de route.

Le problème qui consiste à trouver une route dans un ((graphe)) est
un exemple typique de _problème de recherche_. Nous pouvons facilement
dire si une solution donnée (une route) est valide ou non, mais nous
ne pouvons pas calculer directement une telle solution comme nous le
ferions pour 2 + 2. Au lieu de cela, nous devons continuer à examiner
des solutions potentielles jusqu'à en trouver une qui fonctionne. 

Le nombre des routes possibles dans un graphe est infini. Mais lorsque
nous cherchons une route de _A_ vers _B_, nous sommes seulement intéressés
par celles qui démarrent en _A_. Nous ne nous intéressons pas non plus
à des routes qui repasseraient par le même endroit—ce ne sont clairement
pas les routes les plus efficaces pour notre problème. Toutes ces considérations
réduisent fortement le nombre de routes à examiner.

En fait, nous ne nous intéressons qu'aux routes les plus _courtes_. Il faut
donc nous assurer de regarder les routes les plus courtes avant d'en considérer
de plus longues. Une bonne approche serait de faire «grandir» les routes à
partir de leur point de départ, en explorant tous les lieux accessibles
qui n'ont pas encore été visités, cela jusqu'à ce qu'une
des routes atteigne le but. De cette manière, nous n'explorerons que
les routes potentiellement intéressantes jusqu'à trouver la plus courte
(ou l'une d'elles s'il y en a plusieurs) qui mène au but.

{{index "findRoute function"}}

{{id findRoute}}

Voici une fonction qui fait cela:

```{includeCode: true}
function trouverRoute(graphe, depart, but) {
  let lieuxAtteints = [{lieu: depart, route: []}];
  for (let i = 0; i < lieuxAtteints.length; i++) {
    let {lieu, route} = lieuxAtteints[i];
    for (let prochain of graphe[lieu]) {
      if (prochain == but) return route.concat(prochain);
      if (!lieuxAtteints.some(w => w.lieu == prochain)) {
        lieuxAtteints.push({lieu: prochain, route: route.concat(prochain)});
      }
    }
  }
}
```

L'exploration doit se faire en bon ordre—les endroits atteints
en premier doivent être explorés en premier. Nous ne pouvons pas
immédiatement explorer un endroit dès que nous l'atteignons car
cela impliquerait d'explorer immédiatement les endroits atteints depuis
lui, et ainsi de suite, même s'il y a des chemins plus courts qui n'ont
pas encore été explorés.

Pour cela, la fonction conserve une liste de lieux atteints. Il s'agit
d'un tableau d'endroits qui devraient être explorés aux prochaines
itérations en même temps que la route qui a mené jusqu'à eux.
Elle commence avec la position de départ et une route vide.

La recherche procède en prenant le prochain item de la liste et en
l'explorant, ce qui signifie qu'on explore toutes les routes depuis
cet endroit. Si l'une d'elles conduit à la destination but, on peut
terminer en renvoyant le chemin qui va jusqu'à ce but. Autrement,
et si cette route conduit à un endroit non encore atteint (qui apparaîtrait
alors dans la liste), on ajoute un nouvel item à la liste.
Si au contraire le lieu courant a déjà été atteint et puisque nous
cherchons les routes les plus courtes, nous trouverons soit une
route plus longue jusqu'à ce lieu, soit une route aussi longue qu'une
autre déjà trouvée. Nous n'avons donc pas besoin de l'explorer.

On peut imaginer cela comme une toile de routes connues rampant depuis
la position de départ et grandissant régulièrement de tous côtés (mais
sans jamais faire de boucle). Dès que l'un des fils de cette toile
atteint la destination voulue, ce fil est pris à rebours jusqu'au
point de départ ce qui nous donne notre route.

{{index "connected graph"}}

Ce code n'a pas à se préoccuper du cas où la liste de lieux atteints ne
progresserait plus car nous savons que notre graphe est _connexe_: chaque lieu peut
être atteint depuis un autre. Nous sommes donc toujours capables de
trouver une route entre deux points et la recherche ne peut donc pas
échouer.

```{includeCode: true}
function robotOrienteBut({lieu, colis}, memoire) {
  if (memoire.length == 0) {
    let unColis = colis[0];
    if (unColis.lieu != lieu) {
      memoire = trouverRoute(grapheRoutes, lieu, unColis.lieu);
    } else {
      memoire = trouverRoute(grapheRoutes, lieu, unColis.adresse);
    }
  }
  return {direction: memoire[0], memoire: memoire.slice(1)};
}
```

{{index "goalOrientedRobot function"}}

Ce robot utilise sa mémoire pour maintenir une liste de directions à prendre,
comme le robot suiveur de route. Mais dès que cette liste est vide, il
doit rechercher quoi faire ensuite. Il prend le premier colis non encore
livré et, s'il ne l'a pas encore ramassé, cherche une route qui y mène.
S'il a déjà ramassé le colis, il faut encore le livrer et donc il
recherche une route qui mène à son adresse de livraison.

{{if interactive

Voyons ce que cela donne.

```{test: no, startCode: true}
lancerAnimationRobot(EtatVillage.aleatoire(),
                  robotOrienteBut, []);
```

if}}

Ce robot termine son travail de livraison pour 5 colis en environ 16 étapes.
C'est un peu mieux que `robotRoute` mais toujours pas optimal.

## Exercices

### Mesure d'un robot

{{index "measuring a robot (exercise)", testing, automation, "compareRobots function"}}

Il est difficile de comparer des ((robot))s objectivement en les laissant
résoudre quelques situations. Peut-être que l'un aura eu une mission
plus facile ou du genre de celles dans lesquelles il fonctionne efficacement,
alors que l'autre non.

Écrire une fonction `comparerRobots` qui prend deux robots comme
arguments (ainsi que leurs mémoires initiales). Elle doit produire
100 situations et laisser les robots résoudre chacune d'elles. Au final,
elle devrait renvoyer le nombre moyen d'étapes que chaque robot a utilisé
pour les résoudre.

Pour être équitable, assurez-vous que chaque situation a été donnée
aux deux robots, plutôt que de produire les situations pour chaque
robot.

{{if interactive

```{test: no}
function comparerRobots(robot1, memoire1, robot2, memoire2) {
  // Votre code ici.
}

comparerRobots(robotRoute, [], robotOrienteBut, []);
```
if}}

{{hint

{{index "measuring a robot (exercise)", "runRobot function"}}

Vous aurez besoin d'écrire une variante de la fonction `lancerRobot`.
Plutôt que d'afficher chaque déplacement dans la console, elle devrait
renvoyer le nombre d'étapes que le robot a utilisées pour accomplir sa
tâche.

Votre fonction de mesure peut alors, dans une boucle, produire de nouveaux états et compter
le nombre d'étapes utilisées par chaque robot. Une fois que
suffisamment de mesures ont été réalisées, elle peut utiliser `console.log`
pour afficher la moyenne, le nombre total d'étapes divisé par le nombre
de mesures, de chaque robot.

hint}}

### Efficacité d'un robot

{{index "robot efficiency (exercise)"}}

Pouvez-vous écrire un robot qui effectue ses livraisons plus rapidement
que `robotOrienteRoute`? En observant le comportement de ce dernier, voyez-vous
ce qu'il fait d'idiot? Et comment il pourrait être amélioré?

Si vous avez résolu l'exercice précédent, vous pourriez utiliser
votre fonction `comparerRobots` pour voir si vous êtes effectivement
parvenu à l'améliorer.

{{if interactive

```{test: no}
// Votre code ici.

lancerAnimationRobot(EtatVillage.aleatoire(), votreRobot, memoire);
```

if}}

{{hint

{{index "robot efficiency (exercise)"}}

Le principal défaut de `robotOrienteBut` est qu'il ne considère qu'un
colis à la fois. Il marche souvent dans un sens puis dans l'autre
à travers le village lorsqu'il se trouve que le colis qu'il cherche
se situe à l'autre bout du village, même s'il y en de plus proches.

Une solution possible serait de calculer les routes menant à chaque
colis et de choisir la plus courte. On peut même faire mieux lorsqu'il
y a plusieurs routes minimales, en donnant la préférence à celles qui permettent
de ramasser un paquet plutôt que d'en livrer un.

hint}}

### Ensemble persistant

{{index "persistent group (exercise)", "persistent data structure", "Set class", "set (data structure)", "Group class", "PGroup class"}}

La plupart des structures de données standard fournies par JavaScript
sont mal adaptées à une utilisation persistante. Les tableaux ont
des méthodes `slice` et `concat` qui nous permettent de créer facilement
de nouveaux tableaux sans toucher aux anciens. Mais `Set`, par exemple,
n'a pas de méthode pour créer un nouvel ensemble obtenu en ajoutant
ou en supprimant un élément.

Écrire une nouvelle classe `EnsImmuable`, similaire à la classe `Ensemble`
du [Chapitre ?](object#groups) qui stocke un ensemble de valeurs. Comme
`Ensemble`, elle dispose des méthodes `ajouter`, `supprimer` et `contient`.

Cependant, sa méthode `ajouter` devrait renvoyer une _nouvelle_
instance de `EnsImmuable` contenant l'élément ajouté sans modifier
l'instance sur laquelle elle a été invoquée. Similairement, `supprimer`
crée une nouvelle instance sans l'élément donné.

La classe devrait fonctionner avec des valeurs de tous types, pas seulement
des chaînes. Elle n'a pas besoin d'être performante lorsqu'on l'utilise
avec de grandes quantités de données.

{{index [interface, object]}}

Le ((constructeur)) ne devrait pas faire partie de l'interface de la classe
(même si vous aurez besoin de l'utiliser en interne). Au lieu de cela,
la classe dispose d'une instance vide, `EnsImmuable.vide`, qui peut être
utilisée comme valeur de départ.

{{index singleton}}

Pourquoi n'avez-vous besoin que d'une seule valeur `EnsImmuable.vide`,
plutôt que d'avoir une fonction qui crée un nouvel ensemble vide à chaque fois?

{{if interactive

```{test: no}
class EnsImmuable {
  // Votre code ici.
}

let a = EnsImmuable.vide.ajouter("a");
let ab = a.ajouter("b");
let b = ab.supprimer("a");

console.log(b.contient("b"));
// → true
console.log(a.contient("b"));
// → false
console.log(b.contient("a"));
// → false
```

if}}

{{hint

{{index "persistent map (exercise)", "Set class", [array, creation], "PGroup class"}}

La façon la plus simple de représenter un ensemble de valeurs
est encore d'utiliser un tableau car ils sont faciles à copier.

{{index "concat method", "filter method"}}

Lorsqu'une valeur est ajoutée à l'ensemble, vous pouvez créer un nouvel
ensemble en copiant le tableau d'origine avec la valeur à ajouter (en utilisant
`concat` par exemple). Lorsqu'une valeur est supprimée, vous pouvez utiliser
`filter` sur le tableau.

Le constructeur de la classe peut prendre un tel tableau comme argument
et le stocker comme unique propriété de l'instance. Ce tableau ne sera
jamais modifié.

{{index "static method"}}

Pour ajouter une propriété (`vide`) au constructeur qui n'est pas une méthode,
il faudra l'ajouter au constructeur après la déclaration de la classe, comme
une propriété ordinaire.

Vous n'avez besoin que d'une instance de `vide` car tous les ensembles
vides sont identiques et que les instances de la classe ne sont pas modifiées.
Vous pouvez créer nombre d'ensembles différents à partir de cet ensemble vide
sans l'affecter pour autant.

hint}}
