{{meta {load_files: ["code/journal.js", "code/chapter/04_data.js"], zip: "node/html"}}}

# Structures de données: Objets et Tableaux

{{quote {author: "Charles Babbage", title: "Passages from the Life of a Philosopher (1864)", chapter: true}

On two occasions I have been asked, 'Pray, Mr. Babbage, if you put
into the machine wrong figures, will the right answers come out?'
[...] I am not able rightly to apprehend the kind of confusion of
ideas that could provoke such a question.

quote}}

{{index "Babbage, Charles"}}

{{figure {url: "img/chapter_picture_4.jpg", alt: "Picture of a weresquirrel", chapter: framed}}}

{{index object, "data structure"}}

Les nombres, les booléens et les chaînes sont les atomes qui composent les
structures de données. Mais la plupart des informations nécessitent
plus qu'un simple atome pour se laisser décrire. Les _objets_ vont nous permettre de regrouper des
valeurs—y compris d'autres objets—de façon à produire des structures plus complexes. 

Les programmes construit jusqu'ici ont été limités parce qu'ils n'agissent que sur
des données de type élémentaire. Dans ce chapitre nous introduisons quelques
structures de données basiques. À la fin de celui-ci, vous en saurez assez
pour commencer à écrire des programmes vraiment utiles.

Pour illustrer cela, nous prendrons comme fil conducteur un exemple de
programme plus ou moins réaliste afin d'introduire progressivement les
notions en lien avec le problème. Le code de l'exemple s'appuiera
fréquemment sur les fonctions et les variables définies plus tôt dans le texte. 

{{if book

Le ((bac à sable)) de la version en ligne du livre
([_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code))
fourni un moyen pour faire fonctionner les exemples dans le contexte d'un chapitre particulier.
Si vous souhaitez travailler les exemples dans un autre environnement,
assurez-vous d'avoir télécharger au préalable le code complet pour ce chapitre
depuis la page du bac à sable.

if}}

## L'Écuromorphe

{{index "weresquirrel example", lycanthropy}}

De temps à autre, souvent entre 20 et 22h, ((Jacques)) se transforme en un vif petit
rongeur à la queue touffue.

D'un côté, Jacques se félicite de n'être pas atteint de lycanthropie classique.
Devenir un écureuil pose moins de problèmes que de devenir un loup. Au lieu
de craindre de manger accidentellement le voisin (ce qui serait pour le moins gênant),
il craint d'être mangé par le chat du voisin. Après s'être réveillé, à deux reprises,
nu et désorienté, sur une fine branche au sommet d'un chêne, il se met à verrouiller portes
et fenêtres, et à disposer quelques noix au sol pour s'occuper la nuit dans sa chambre.

Cela règle les problèmes de chat et d'arbre. Mais Jacques aurait préféré
se débarrasser de ces changements d'état intempestifs. L'irrégularité de leurs
survenues lui fit penser qu'ils étaient causés par quelque événement.
Pendant un temps, il crut qu'ils n'avaient lieu que les jours où il s'était
trouvé à proximité de chênes. Mais éviter les chênes ne mit pas fin
au problème.

{{index journal}}

Il décida donc d'opter pour une approche plus scientifique. Il commença
à tenir un journal en y notant, chaque jour, ce qu'il faisait et si sa
transformation avait eu lieu. Il espérait ainsi pouvoir identifier les
conditions qui annonçaient sa métamorphose.

La première chose dont il eut besoin fut une structure de données afin
d'organiser et conserver les informations qu'il notait.

## Collections de données

{{index ["data structure", collection], [memory, organization]}}

Pour pouvoir exploiter un fragment de données numériques, nous aurons
en premier lieu besoin de trouver un moyen de le représenter dans la 
mémoire de notre machine. Supposons, pour l'exemple, que nous souhaitions
représenter une ((collection)) de nombres 2, 3, 5, 7 et 11.

{{index string}}

Nous pourrions faire preuve de créativité avec les chaînes—après tout,
elles peuvent être de taille arbitraire et il est donc possible d'y mettre
beaucoup de données—et utiliser `"2 3 5 7 11"` comme représentation.
Mais c'est maladroit. Il nous faudrait encore extraire les chiffres et les 
convertir à nouveau en nombres pour pouvoir les exploiter.

{{index [array, creation], "[] (array)"}}

Fort heureusement, Javascript fournit un type de donnée approprié pour
conserver une série de valeurs. Cela s'appelle un _tableau_ \[_array_\] et
on l'écrit comme une liste de valeurs entre crochets, séparées par des virgules.

```
let listeDeNombres = [2, 3, 5, 7, 11];
console.log(listeDeNombres[2]);
// → 5
console.log(listeDeNombres[0]);
// → 2
console.log(listeDeNombres[2 - 1]);
// → 3
```

{{index "[] (subscript)", [array, indexing]}}

La notation pour accéder aux éléments d'un tableau utilise aussi des ((crochets)).
Une expression suivie d'une paire de crochets qui contient une autre expression va chercher
l'élément du tableau (associé à la première expression) dont l'_((index))_ ou position est
donné par l'expression entre crochets.

{{id array_indexing}}
{{index "zero-based counting"}}

Le premier index d'un tableau est zéro, non un. Ainsi, son premier élément
s'obtient avec `listeDeNombres[0]`. Le fait de démarrer les comptes à zéro
a une longue tradition en informatique et a, à certains égards, beaucoup de sens, mais
il faut un peu de temps pour s'y habituer. Pensez à l'index comme le nombre d'éléments
à ignorer, en partant du début du tableau, pour atteindre celui qu'on veut.

{{id properties}}

## Propriétés

{{index "Math object", "Math.max function", ["length property", "for string"], [object, property], "period character", [property, access]}}

Nous avons vu quelques expressions à l'allure curieuse comme `maChaine.length`
(pour obtenir la longueur d'une chaîne) et `Math.max` (la fonction maximum)
dans les chapitres précédents. Ces expressions servent à accéder à une
_propriété_ d'une certaine valeur. Dans le premier cas, on accède à la propriété
`length` de la valeur chaîne `maChaîne` et, dans le second, à la propriété `max` de
l'objet `Math` (qui regroupe des constantes et des fonctions en rapport avec les mathématiques). 

{{index [property, access], null, undefined}}

Presque toutes les valeurs de JavaScript ont des propriétés. Les seules
exceptions sont `null` et `undefined`. Si vous tentez d'accéder à une
propriété de l'un d'eux, vous obtenez une erreur.

```{test: no}
null.length;
// → TypeError: null has no properties
```

{{indexsee "dot character", "period character"}}
{{index "[] (subscript)", "period character", "square brackets", "computed property", [property, access]}}

Il y a principalement deux façons d'accéder à une propriété en JavaScript.
La première utilise un point et la seconde une paire de crochets. Aussi bien
`valeur.x` que `valeur[x]` permettent d'accéder à une propriété de `valeur`-mais
pas nécessairement la même. La différence réside dans la façon dont JavaScript interprète
`x`. Lorsqu'on utilise un point, le nom après lui est littéralement le nom de la propriété.
Lorsqu'on utilise des crochets, l'expression qui s'y trouve est _évaluée_ afin d'obtenir
le nom littéral de la propriété. Alors que `valeur.x` charge la propriété de `valeur`
nommée «`x`», `valeur[x]` commence par évaluer l'expression `x` et utilise le résultat, converti
en chaîne, comme nom de propriété. 

Ainsi, si vous savez que la propriété qui vous intéresse s'appelle _couleur_,
vous utiliserez `valeur.couleur`. Si vous voulez récupérer la propriété dont
le nom est la valeur de la variable `i`, vous utiliserez `valeur[i]`.
Les noms de propriétés sont des chaînes. Cela peut être n'importe quelle chaîne,
mais la notation avec point ne peut s'utiliser qu'avec des chaînes qui correspondent
à des noms de variable valides. Aussi, si vous voulez accéder à une propriété nommée
_2_ ou _John Doe_, vous devez utiliser les crochets: `valeur[2]` ou `valeur["John Doe"]`.

Les éléments d'un ((tableau)) sont ses propriétés et ils utilisent des nombres comme
nom de propriété. Comme il n'est pas possible d'utiliser des nombres dans
la notation pointée, et qu'on souhaite généralement conserver l'index dans une variable
de toute façon, on doit utiliser la notation crochet pour obtenir un élément
d'un tableau.  

{{index ["length property", "for array"], [array, "length of"]}}

La propriété `length` d'un tableau contient le nombre de ses éléments.
Ce nom de propriété est un nom de variable valide qu'on connaît à l'avance,
aussi, pour connaître la longueur d'un tableau, on préfère écrire `tableau.length`
plutôt que `tableau["length"]` car c'est plus simple.

{{id methods}}

## Méthodes

{{index [function, "as property"], method, string}}

En plus de la propriété `length`, les valeurs de type tableau ou chaîne
contiennent nombre de propriétés qui correspondent à des valeurs-fonctions.

```
let oups = "Oups";
console.log(typeof oups.toUpperCase);
// → function
console.log(oups.toUpperCase());
// → OUPS 
``` 

{{index "case conversion", "toUpperCase method", "toLowerCase method"}}

Toute chaîne possède une propriété `toUpperCase`. Lorsqu'on l'invoque,
elle renvoie une copie de la chaîne dans laquelle toutes les lettres ont été mises
en majuscules. Il y a aussi `ToLowerCase` qui fait le contraire.

{{index "this binding"}}

Fait intéressant, même si aucun argument n'est fourni à l'appel de `toUpperCase`,
la fonction parvient tout de même à accéder à la chaîne `"Oups"`, la valeur qui porte
la propriété invoquée. La manière dont cela fonctionne sera détaillée dans
le [Chapitre ?](object#obj_methods)

Les propriétés qui font référence à des fonctions sont appelées _méthodes_
de la valeur à laquelle elles appartiennent, on peut donc dire que
«`toUpperCase` est une méthode d'une chaîne».

{{id array_methods}}

L'exemple qui suit vous montre deux méthodes qu'on peut utiliser
pour manipuler des tableaux.

```
let sequence = [1, 2, 3];
sequence.push(4);
sequence.push(5);
console.log(sequence);
// → [1, 2, 3, 4, 5]
console.log(sequence.pop());
// → 5
console.log(sequence);
// → [1, 2, 3, 4]
```

{{index collection, array, "push method", "pop method"}}

La méthode `push` ajoute une valeur à la fin du tableau, et `pop`
fait le contraire en supprimant la dernière valeur du tableau et en
la renvoyant.

{{index ["data structure", stack]}}

Ces drôles de noms sont ceux des opérations traditionnelles sur une _((pile))_.
Une pile, en programmation, renvoie à une structure de données qui permet
principalement d'_empiler_ une donnée à son sommet (penser à une pile d'assiettes) et
de la _dépiler_ à nouveau dans l'ordre opposé de façon que le dernier élément ajouté soit aussi
le premier supprimé. Celles-ci sont courantes en programmation—peut-être vous souvenez-vous de la
((pile d'appels)) de fonctions mentionnée dans le [chapitre précédent](functions#stack)
qui exploite la même idée. 

## Objets

{{index journal, "weresquirrel example", array, record}}

Revenons à notre écuromorphe. On peut représenter les notes quotidiennes
prises par Jacques à l'aide d'un tableau. Malheureusement, ces notes ne sont
pas seulement formées d'un nombre ou d'une chaîne—chaque note est constituée
d'une liste d'activités et d'une valeur booléenne indiquant si la transformation
de Jacques s'est produite ou non. Le plus agréable serait de les grouper au sein
d'une seule valeur puis de grouper celles-ci dans un tableau de notes quotidiennes. 

{{index [syntax, object], [property, definition], [braces, object], "{} (object)"}}

Une valeur de type _((objet))_ consiste en une collection arbitraire de propriétés.
On peut en créer une en utilisant une expression délimitée par des accolades.

```
let jour1 = {
  ecureuil: false,
  evenements: ["travailler", "caresser un arbre", "pizza", "course à pied"]
};
console.log(jour1.ecureuil);
// → false
console.log(jour1.loup);
// → undefined
jour1.loup = false;
console.log(jour1.loup);
// → false
```

{{index [quoting, "of object properties"], "colon character"}}

À l'intérieur des accolades, on place une liste de propriétés séparées par des virgules.
Chaque propriété consiste en un nom suivi de deux points eux-mêmes suivis d'une valeur.  
Lorsqu'il faut plusieurs lignes, on convient d'indenter chaque propriété comme
dans l'exemple de façon à améliorer la lisibilité.
Lorsque le nom d'une propriété n'est pas celui d'une variable valide,
on doit le placer entre guillemets.

```
let descriptions = {
  travailler: "se rendre au travail",
  "caresser un arbre": "frotter une partie de son corps contre l'écorce d'un arbre"
};
```

{{index [braces, object]}}

Ainsi les accolades ont deux significations en JavaScript.
Au début d'une ((instruction)), elles délimitent un ((bloc)) d'instructions.
Dans toute autre position, elles décrivent un objet.
Fort heureusement, il est rarement utile de démarrer une instruction avec un objet
entre accolades, de sorte que l'ambiguité de leur utilisation n'est pas vraiment
un problème.

{{index undefined}}

Lire une propriété qui n'existe pas produira la valeur `undefined`.

{{index [property, assignment], mutability, "= operator"}}

On peut affecter une valeur à une expression désignant une propriété avec l'opérateur
`=`. Cela a pour effet de remplacer la valeur de cette propriété si elle existait déjà
ou de la créer sur l'objet sinon.

{{index "tentacle (analogy)", [property, "model of"], [binding, "model of"]}}

Revenons à notre image des tentacules pour l'((affectaction))—celle d'une propriété est similaire. 
Chaque tentacule saisit une valeur, mais d'autres variables ou propriétés peuvent aussi se
saisir de cette même valeur. Vous pouvez imaginer un objet comme une sorte de pieuvre avec un
nombre arbitraire de tentacules, chacune portant un nom tatoué dessus.

{{index "delete operator", [property, deletion]}}

L'opérateur `delete` ampute la pieuvre de l'une de ses tentacules. C'est
un opérateur unaire qui, lorsqu'il est appliqué à l'une des propriétés d'un objet,
va supprimer le nom de cette propriété de l'objet. Cela ne s'utilise pas très
souvent, mais reste possible.

```
let unObjet = {gauche: 1, droite: 2};
console.log(unObjet.gauche);
// → 1
delete unObjet.gauche;
console.log(unObjet.gauche);
// → undefined
console.log("gauche" in unObjet);
// → false
console.log("droite" in unObjet);
// → true
```

{{index "in operator", [property, "testing for"], object}}

L'opérateur binaire `in`, lorsqu'il est appliqué à une chaîne et à un objet,
vous indique si l'objet a une propriété avec ce nom. La différence
entre le fait de mettre à une propriété la valeur `undefined` et de la supprimer est que,
dans le premier cas, l'objet _a_ toujours cette propriété (elle n'a tout simplement pas
une valeur très intéressante), alors que dans le second, cette propriété n'en fait plus
partie et l'opérateur `in` renverra alors `false`.

{{index "Object.keys function"}}

Pour découvrir les propriétés d'un objet, on peut utiliser la fonction
`Object.keys`. Vous lui donnez un objet et elle vous renvoie un tableau
de chaînes—celles des noms de ses propriétés.

```
console.log(Object.keys({x: 0, y: 0, z: 2}));
// → ["x", "y", "z"]
```

On dispose aussi de la fonction `Object.assign` qui copie toutes les propriétés
d'un objet dans un autre.

```
let objetA = {a: 1, b: 2};
Object.assign(objetA, {b: 3, c: 4});
console.log(objetA);
// → {a: 1, b: 3, c: 4}
```

{{index array, collection}}

Les tableaux sont en fait juste une sorte d'objets ayant pour rôle
de conserver une séquence de choses. Si vous évaluez `typeof []`, cela
produit `"objects"`. Pour reprendre la métaphore, vous pouvez les voir
comme des pieuvres très allongées avec leurs tentacules les unes à la suite
des autres, bien alignées, et portant chacune un numéro.

{{index journal, "weresquirrel example"}}

Nous représenterons le journal tenu par Jacques comme un tableau d'objets.

```{test: wrap}
let journal = [
  {evenements: ["travailler", "caresser les arbres", "pizza",
            "course à pied", "télévision"],
   ecureuil: false},
  {evenements: ["travailler", "glace", "chou-fleur",
            "lasagne", "caresser les arbres", "se brosser les dents"],
   ecureuil: false},
  {evenements: ["week-end", "sortie à vélo", "faire une pause", "cacahuètes",
            "bière"],
   ecureuil: true},
  /* et ainsi de suite... */
];
```

## Muabilité

Nous allons commencer à _vraiment_ programmer bientôt. Mais
il y a encore un point de théorie à comprendre.

{{index mutability, "side effect", number, string, Boolean, [object, mutability]}}

Nous avons déjà dit que les valeurs d'un objet peuvent être modifiées.
Les valeurs élémentaires (nombres, chaînes et booléens) présentées plus tôt
sont toutes _((immuables))_—il est impossible de modifier des valeurs de ce
type. Vous pouvez les combiner pour produire de nouvelles valeurs, mais
lorsque vous faites référence à une valeur-chaîne particulière (par exemple),
cette valeur restera toujours identique à elle-même. Si une chaîne contient `"cat`",
il n'est pas possible qu'un autre code modifie l'un de ces caractères de façon qu'elle
devienne `"rat"`.

Les objets fonctionnent différemment. On _peut_ changer leurs propriétés de telle
sorte que le contenu d'une valeur-objet puisse varier au cours du temps.

{{index [object, identity], identity, [memory, organization], mutability}}

Lorsqu'on a deux nombres, 120 et 120, on peut estimer qu'ils sont un seul et même
nombre, qu'ils réfèrent ou non à une même zone mémoire.
Avec les objets, il y a une différence entre avoir deux références au même objet
et avoir deux objets différents qui contiennent exactement les mêmes propriétés.
Étudiez le fragment de code qui suit:

```
let objet1 = {valeur: 10};
let objet2 = objet1;
let objet3 = {valeur: 10};

console.log(objet1 == objet2);
// → true
console.log(objet1 == objet3);
// → false

objet1.valeur = 15;
console.log(objet2.valeur);
// → 15
console.log(objet3.valeur);
// → 10
```

{{index "tentacle (analogy)", [binding, "model of"]}}

Les variables `objet1` et `objet2` font référence au _même_ objet, voilà
pourquoi modifier `objet1` change aussi les propriétés d'`objet2`. On dit
qu'ils ont la même _identité_. La variable `objet3` fait référence à (pointe) un
objet différent, qui a initialement le même contenu que `objet1` tout en ayant
une vie propre.

{{index "const keyword", "let keyword", [binding, "as state"]}}

Les variables peuvent aussi être modifiables ou constantes, mais cela
est sans rapport avec le comportement des valeurs qu'elles référencent (ou pointent).
Bien que la valeur d'un nombre ne soit pas modifiable, cela n'empêche
pas une variable déclarée avec `let` de suivre une valeur numérique qui évolue
en modifiant la valeur qu'elle référence, non la valeur elle-même.
Symétriquement, une variable déclarée avec `const` pointera toujours
vers le même objet, mais cela n'empêche pas le contenu de cet objet de changer.

```{test: no}
const score = {visiteurs: 0, domicile: 0};
// Ne pose aucun problème
score.visiteurs = 1;
// Ce n'est pas permis
score = {visiteurs: 1, domicile: 1};
```

{{index "== operator", [comparison, "of objects"], "deep comparison"}}

Lorsque vous comparez des objets avec l'opérateur `==` de JavaScript,
la comparaison se fait par identité: elle ne produit `true` qui si ces
objets sont une seule et même valeur. Comparer deux objets non identiques
produira `false`, même s'ils ont les mêmes propriétés. Il n'y a pas d'opération
prédéfinie de comparaison «profonde» (propriété par propriété) en
JavasCript, mais on peut l'écrire soi-même (ce qui est l'objet d'un
des [exercices](data#exercise_deep_compare) en fin de chapitre).

## Journal de l'écuromorphe

{{index "weresquirrel example", lycanthropy, "addEntry function"}}

C'est ainsi que Jacques démarra son interpréteur JavaScript et
mit en place l'environnement dont il avait besoin pour tenir son ((journal)).

```{includeCode: true}
let journal = [];

function ajouterNote(evenements, ecureuil) {
  journal.push({evenements, ecureuil});
}
```

{{index [braces, object], "{} (object)", [property, definition]}}

Observez que l'objet ajouté au journal a une forme un peu étrange.
Plutôt que de déclarer les propriétés avec la syntaxe
`evenements: evenements`, on donne juste le nom de la propriété.
Il s'agit d'un raccourci d'écriture qui veut dire la même chose—si
le nom d'une propriété apparaissant entre accolades n'est pas suivi
d'une valeur, c'est la valeur pointée par ce nom qui est utilisée. 

Chaque soir, vers 22h—ou parfois le matin qui suit, après être descendu
du sommet de son perchoir—Jacques renseigne son journal du jour.

```
ajouterNote(["travailler", "caresser un arbre", "pizza", "course à pied",
          "télévision"], false);
ajouterNote(["travailler", "glace", "chou-fleur", "lasagne",
          "caresser un arbre", "se brosser les dents"], false);
ajouterNote(["week-end", "vélo", "pause", "cacahuètes",
          "bière"], true);
```

Une fois qu'il aura suffisamment de notes, il a l'intention d'utiliser
les statistiques pour comprendre lequel de ces événements peut être
rapproché de son écuromorphose.

{{index correlation}}

La _corrélation_ est une mesure de ((dépendance)) entre des variables
statistiques. Une variable statistique n'est pas tout à fait la même chose
qu'une variable d'un programme. En statistique, on dispose habituellement d'un
ensemble de _mesures_, et chaque variable prend ses valeurs en fonctions de celles-ci.
La corrélation entre deux variables s'exprime d'ordinaire comme un nombre de l'intervalle
qui va de -1 à 1. Une corrélation égale à 0 veut dire que les variables impliquées
n'ont pas de relations mutuelles. Une corrélation à 1 indique que les deux variables
sont parfaitement reliées—si vous connaissez l'une, vous connaissez l'autre. -1
aussi indique une relation parfaite mais dans une opposition—si l'une est vraie, l'autre est
fausse et vice versa.

{{index "phi coefficient"}}

Pour calculer cette corrélation entre deux variables booléennes,
on peut utiliser le _coefficient phi_ (_ϕ_). On l'obtient en utilisant
une formule dont les paramètres proviennent d'une ((table de fréquences))
qui contient le nombre de fois où la combinaison de deux variables a
été observée. La formule produit alors un nombre compris entre -1 et 1 qui
décrit la corrélation.

On pourrait prendre l'événement manger une ((pizza)) et le mettre
dans une table de fréquences comme celle qui suit. Chaque nombre
indique combien de fois la combinaison indiquée a eu lieu dans nos mesures.

{{figure {url: "img/pizza-squirrel.svg", alt: "Eating pizza versus turning into a squirrel", width: "7cm"}}}

Si nous appelons cette table _n_, on peut calculer _ϕ_ en utilisant la formule qui suit:

{{if html

<div>
<table style="border-collapse: collapse; margin-left: 1em;"><tr>
  <td style="vertical-align: middle"><em>ϕ</em> =</td>
  <td style="padding-left: .5em">
    <div style="border-bottom: 1px solid black; padding: 0 7px;"><em>n</em><sub>11</sub><em>n</em><sub>00</sub> −
      <em>n</em><sub>10</sub><em>n</em><sub>01</sub></div>
    <div style="padding: 0 7px;">√<span style="border-top: 1px solid black; position: relative; top: 2px;">
      <span style="position: relative; top: -4px"><em>n</em><sub>1•</sub><em>n</em><sub>0•</sub><em>n</em><sub>•1</sub><em>n</em><sub>•0</sub></span>
    </span></div>
  </td>
</tr></table>
</div>

if}}

{{if tex

[\begin{equation}\varphi = \frac{n_{11}n_{00}-n_{10}n_{01}}{\sqrt{n_{1\bullet}n_{0\bullet}n_{\bullet1}n_{\bullet0}}}\end{equation}]{latex}

if}}

(Si à ce point vous avez envie de jeter le bouquin par la fenêtre du
fait d'un mauvais souvenir des mathématiques—n'en faites rien! Je n'ai
pas l'intention de vous torturer avec des pages sans fin contenant
d'obscures formules—C'est juste une petite formule pour une fois. Et d'ailleurs,
nous allons juste la traduire en JavaScript.)

La notation [_n_~01~]{if html}[[$n_{01}$]{latex}]{if tex} symbolise
le nombre de mesures où la première variable (être transformé en écureuil) est
fausse (0) et la seconde (pizza) est vraie (1). Dans la table précédente,
[_n_~01~]{if html}[[$n_{01}$]{latex}]{if tex} vaut 9.

La valeur de [_n_~1•~]{if html}[[$n_{1\bullet}$]{latex}]{if tex} fait
référence à la somme de toutes les mesures où la première variable est vraie,
soit 5 dans la table exemple. Similairement, [_n_~•0~]{if
html}[[$n_{\bullet0}$]{latex}]{if tex} fait référence à la somme
des mesures où la seconde variable est fausse. 
 
{{index correlation, "phi coefficient"}}

Pour notre table exemple, la partie au dessus de la division (le dividende)
serait 1×76−4×9 = 40, et la partie en dessous (le diviseur) serait
la racine carrée de 5×85×10×80, soit [√340000]{if
html}[[$\sqrt{340000}$]{latex}]{if tex}. On obtient finalement  _ϕ_ ≈
0.069, ce qui est faible. On peut conclure que le fait de manger
des ((pizza))s ne semble pas avoir d'influence sur les transformations.

## Calculer la corrélation

{{index [array, "as table"], [nesting, "of arrays"]}}

En JavaScript, nous pouvons représenter un ((tableau)) à double entrée par
un tableau à quatre éléments (`[76, 9, 4, 1]`). Nous pourrions encore
utiliser d'autres représentations, comme un tableau contenant deux sous-tableaux
à deux éléments (`[[76, 9], [4, 1]]`) ou comme un objet avec des noms de propriétés
comme `"11"` et `"01"`, mais le tableau du début est simple et réduit
agréablement la taille des expressions servant à accéder aux éléments.
Nous interpréterons les indices du tableau comme des nombres binaires à
deux bits, où le bit le plus à gauche (le plus significatif) fait référence
à la variable d'écureuil et celui de droite (le moins significatif) à la
variable d'événement. Par exemple, le nombre binaire 10 désigne la case
où Jacques s'est transformé en écureuil, mais où l'événement (disons, «pizza») ne
s'est pas produit. Il y a quatre cas possibles. Et puisque le nombre binaire 10
s'écrit 2 en décimal, nous stockerons ce nombre à l'index 2 du tableau.

{{index "phi coefficient", "phi function"}}

{{id phi_function}}

Voici la fonction qui calcule le coefficient _ϕ_ pour un tel tableau:

```{includeCode: strip_log, test: clip}
function phi(tab) {
  return (tab[3] * tab[0] - tab[2] * tab[1]) /
    Math.sqrt((tab[2] + tab[3]) *
              (tab[0] + tab[1]) *
              (tab[1] + tab[3]) *
              (tab[0] + tab[2]));
}

console.log(phi([76, 9, 4, 1]));
// → 0.068599434
```

{{index "square root", "Math.sqrt function"}}

C'est la traduction directe la formule de _ϕ_ en JavaScript.
`Math.sqrt` est la fonction racine carrée telle que fournie par
l'environnement standard de JavaScript à travers l'objet `Math`.
Nous devons encore ajouter deux éléments du tableau de façon à
calculer les valeurs comme [n~1•~]{if html}[[$n_{1\bullet}$]{latex}]{if tex}
car les sommes des lignes ou des colonnes ne sont pas directement accessibles
depuis notre structure de données.

{{index "JOURNAL data set"}}

Jacques a tenu son journal pendant trois mois. L'ensemble de données
correspondant est accessible depuis le [bac à sable](https://eloquentjavascript.net/code#4)
dans ce chapitre[
([_https://eloquentjavascript.net/code#4_](https://eloquentjavascript.net/code#4))]{if
book}, via la variable `JOURNAL` et aussi en téléchargeant ce [fichier](https://eloquentjavascript.net/code/journal.js).

{{index "tableFor function"}}

Pour extraire du journal le tableau à double entrée pour un événement spécifique,
nous devons parcourir chaque note et compter combien de fois l'événement a eu lieu
en relation avec les transformations en écureuil.

```{includeCode: strip_log}
function tableauPour(evenement, journal) {
  let tableau = [0, 0, 0, 0];
  for (let i = 0; i < journal.length; i++) {
    let entree = journal[i], index = 0;
    if (entree.evenements.includes(evenement)) index += 1;
    if (entree.ecureuil) index += 2;
    tableau[index] += 1;
  }
  return tableau;
}

console.log(tableauPour("pizza", JOURNAL));
// → [76, 9, 4, 1]
```

{{index [array, searching], "includes method"}}

Les tableaux disposent d'une méthode `includes` qui vérifie si
une valeur donnée se trouve dans le tableau. Notre fonction exploite
cela pour savoir si le nom de l'événement auquel on s'intéresse fait partie
de la liste des événements qui se sont produits un jour donné.

{{index [array, indexing]}}

Le corps de boucle de `TableauPour` s'occupe de reconnaître dans quelle case du
tableau mettre chaque note du journal en vérifiant si cette note contient
l'événement auquel on s'intéresse et s'il se produit en lien avec une transformation.
La boucle ajoute alors un à la case adéquate du tableau.

Nous avons maintenant les outils adaptés pour calculer les ((corrélation))s
individuelles. La dernière étape consiste à trouver le coefficient de corrélation
pour chaque événement enregistré dans le journal et à voir si l'un d'eux a une 
influence déterminante.

{{id for_of_loop}}

## Boucler sur un tableau

{{index "for loop", loop, [array, iteration]}}

Dans la fonction `tableauPour` on trouve une boucle de la forme:

```
for (let i = 0; i < JOURNAL.length; i++) {
  let note = JOURNAL[i];
  // Faire quelque chose avec note
}
```

Ce genre de boucle est habituelle en programmation JavaScript classique—parcourir
un tableau un élément à la fois est chose courante, et pour ce faire on exploite
un compteur en lien avec la longueur du tableau et on sélectionne chaque élément tour à tour.

Il y a un moyen plus simple pour écrire une telle boucle dans les versions récentes
de JavaScript.

```
for (let note of JOURNAL) {
  console.log(`${note.evenements.length} événements.`);
}
```

{{index "for/of loop"}}

Lorsqu'une boucle `for` a cette allure, avec le mot `of` qui suit la définition
d'une variable, elle va parcourir les éléments de la valeur située après `of` tout en associant
cet élément à la variable située avant.
Cela fonctionne avec les tableaux mais aussi avec les chaînes et d'autres
structures de données. Nous expliquerons _comment_ cela fonctionne dans le
[chapitre ?](object). 

{{id analysis}}

## L'analyse finale

{{index journal, "weresquirrel example", "journalEvents function"}}

Nous devons calculer la corrélation de chaque sorte d'événement qui apparaît
dans le jeu de données. Pour faire cela, nous commençons par _trouver_ ces sortes
d'événements.

{{index "includes method", "push method"}}

```{includeCode: "strip_log"}
function journalEvenements(journal) {
  let evenements = [];
  for (let note of journal) {
    for (let evenement of note.evenements) {
      if (!evenements.includes(evenement)) {
        evenements.push(evenement);
      }
    }
  }
  return evenements;
}

console.log(journalEvenements(JOURNAL));
// → ["carotte", "exercice", "week-end", "pain", …]
```

La fonction collecte chaque événement en parcourant tous les événements
de toutes les notes du journal et en ajoutant ceux qui n'ont pas déjà
été collectés au tableau `evenements`. 

En utilisant cela, nous pouvons voir les différentes ((corrélation))s.

```{test: no}
for (let evenement of journalEvenements(JOURNAL)) {
  console.log(evenement + ":", phi(tableauPour(evenement, JOURNAL)));
}
// → carotte:   0.0140970969
// → exercice: 0.0685994341
// → week-end:  0.1371988681
// → pain:   -0.0757554019
// → gâteau: -0.0648203724
// et ainsi de suite...
```

La plupart des coefficients de corrélation sont proches de zéro. Manger
des carottes, du pain, ou des gâteaux n'a pas d'effet sur la transformation.
Il _semble_ que le phénomène se produise plus souvent les week-ends. Filtrons
le résultat de façon à ne faire apparaître que les événements de corrélation
supérieure à 0,1 ou inférieure à -0,1.

```{test: no, startCode: true}
for (let evenement of journalEvenements(JOURNAL)) {
  let correlation = phi(tableauPour(evenement, JOURNAL));
  if (correlation > 0.1 || correlation < -0.1) {
    console.log(evenement + ":", correlation);
  }
}
// → week-end:              0.1371988681
// → se brosser les dents: -0.3805211953
// → sucrerie:              0.1296407447
// → travail:              -0.1371988681
// → spaghetti:             0.2425356250
// → lire:                  0.1106828054
// → cacahuètes:            0.5902679812
```

Ah ah! On voit deux facteurs ayant un coefficient de corrélation nettement plus fort que les autres.
Manger des ((cacahuètes)) a un fort impact positif sur l'éventualité de se changer
en écureuil, tandis que se laver les dents a un fort effet négatif sur celle-ci.

Intéressant. Essayons quelque chose.

```
for (let note of JOURNAL) {
  if (note.evenements.includes("cacahuètes") &&
     !note.evenements.includes("se brosser les dents")) {
    note.evenements.push("cacahuètes dents");
  }
}
console.log(phi(tableauPour("cacahuètes dents", JOURNAL)));
// → 1
```

C'est un résultat très net. Le phénomène se produit précisément lorsque
Jacques mange des ((cacahuètes)) sans se brosser les dents. S'il
n'avait pas été si négligent sur son hygiène buccale, il n'aurait
jamais pu s'apercevoir qu'il était atteint d'écuromorphose.

Sachant cela, Jacques cessa de manger des cacahuètes et ne se transforma
plus jamais en écureuil.

{{index "weresquirrel example"}}

Pendant quelques années, tout alla bien pour Jacques. Mais il finit par
perdre son emploi. Et comme il vivait dans un mauvais pays où être au
chômage fait perdre tous droits à la santé, il dut se résoudre à travailler
dans un ((cirque)) en tant que l'_incroyable homme écureuil_, et se goinfra
de beurre de cacahuètes avant chaque représentation.

Un jour, lassé par cette existence pitoyable, Jacques ne parvint pas à retrouver
sa forme humaine, sautilla hors du cirque et disparut dans la forêt.
On le revit plus jamais.

## Plus sur les tableaux

{{index [array, methods], [method, array]}}

Avant de finir ce chapitre, je voudrais vous présenter quelques notions
supplémentaires au sujet des objets. Je vais commencer par vous présenter
quelques méthodes bien utiles des tableaux.

{{index "push method", "pop method", "shift method", "unshift method"}}

Nous avons déjà mentionné `push` et `pop`, qui respectivement ajoute
et enlève un élément à la fin d'un tableau, [plus tôt](data#array_methods)
dans ce chapitre. Les méthodes correspondantes
pour ajouter ou supprimer un élément au _début_ d'un tableau sont `unshift` et `shift`.

```
let listeTaches = [];
function seSouvenir(tache) {
  listeTaches.push(tache);
}
function recupererTache() {
  return listeTaches.shift();
}
function seSouvenirEnUrgence(tache) {
  listeTaches.unshift(tache);
}
```

{{index "task management example"}}

Ce programme gère une _file_ de tâches. On ajoute (enfile) une tâche en fin
de file en utilisant `seSouvenir("aller à l'épicerie")`, et lorsqu'on
est prêt à faire quelque chose, on utilise `recupererTache()` pour (défiler)
obtenir tout en supprimant 
une tâche en début de file. La fonction `seSouvenirEnUrgence` ajoute aussi
une tâche mais en début de file plutôt qu'à la fin.

{{index [array, searching], "indexOf method", "lastIndexOf method"}}

Pour chercher une valeur particulière, les tableaux disposent de la méthode
`indexOf`. Elle recherche la valeur en parcourant le tableau du début à la fin
et renvoie l'index associé à la valeur cherchée—ou -1 si elle n'est pas trouvée.
Pour faire une recherche depuis la fin plutôt que du début, ils disposent d'une méthode
similaire nommée `lastIndexOf`.

```
console.log([1, 2, 3, 2, 1].indexOf(2));
// → 1
console.log([1, 2, 3, 2, 1].lastIndexOf(2));
// → 3
```

Ces deux méthodes peuvent recevoir un deuxième argument optionnel qui
précise l'index à partir duquel commencer la recherche.

{{index "slice method", [array, indexing]}}

`slice` est une autre méthode importante des tableaux. Elle prend un index
de début et un index de fin et renvoie un tableau qui contient seulement
les éléments situés entre ces index. L'index de début est inclusif, l'index
de fin est exclusif.

```
console.log([0, 1, 2, 3, 4].slice(2, 4));
// → [2, 3]
console.log([0, 1, 2, 3, 4].slice(2));
// → [2, 3, 4]
```

{{index [string, indexing]}}

Lorsque l'index de fin n'est pas fourni, `slice` prendra tous les éléments après l'index
de début. On peut aussi omettre l'index de début de façon à copier le tableau intégralement.

{{index concatenation, "concat method"}}

La méthode `concat` sert à «coller» des tableaux afin d'en obtenir un autre,
cela de façon analogue à l'effet de l'opérateur `+` sur les chaînes.

L'exemple qui suit vous montre `concat` et `slice` en action. Étant donnés
un tableau et un index, il renvoie un nouveau tableau qui est une copie de l'ancien
privé de l'élément d'index donné.

```
function supprimer(tab, index) {
  return tab.slice(0, index)
    .concat(tab.slice(index + 1));
}
console.log(supprimer(["a", "b", "c", "d", "e"], 2));
// → ["a", "b", "d", "e"]
```

Si vous donnez à `concat` une valeur qui n'est pas un tableau, elle sera
ajoutée au tableau final comme si on l'avait mise dans un tableau à un élément.

## Propriétés des chaînes

{{index [string, properties]}}

On peut lire des propriétés de chaîne comme `length` et `toUpperCase`.
Mais si on essaie de leur ajouter une propriété, elle ne colle pas.

```
let kim = "Kim";
kim.age = 88;
console.log(kim.age);
// → undefined
```

Les valeurs de type chaîne, nombre et booléen ne sont pas des objets,
et, bien que JavaScript n'émette aucune sorte de protestation si on
essaie d'ajouter de nouvelles propriétés dessus, cela n'a aucun effet.
Comme indiqué plus tôt, ces sortes de valeurs sont immuables et ne peuvent
être modifiées.

{{index [string, methods], "slice method", "indexOf method", [string, searching]}}

Cela n'empêche pas ces types d'avoir des propriétés prédéfinies. Chaque valeur-chaîne
dispose de nombreuses méthodes. Certaines sont très utiles comme `slice` et `indexOf`,
lesquelles ressemblent aux méthodes de même nom des tableaux.

```
console.log("noix de coco".slice(8, 12));
// → coco
console.log("noix de coco".indexOf("x"));
// → 3
```

Une différence de la méthode `indexOf` pour les chaînes est qu'elle
peut chercher une (sous-)chaîne contenant plus d'un caractère, alors
que celle des tableaux ne cherche qu'un élément unique.

```
console.log("un deux trois".indexOf("tr"));
// → 8
```

{{index [whitespace, trimming], "trim method"}}

La méthode `trim` supprime les caractères blancs (espaces, sauts de ligne,
tabulations et autres caractères similaires) situés en début et en fin de chaîne.

```
console.log("  d'accord \n ".trim());
// → d'accord
```

La fonction `bourrerAvecZero` du [chapitre précédent](functions) existe aussi
comme méthode de chaîne. Elle se nomme `padStart` et prend la taille
désirée ainsi que le caractère de bourrage en arguments.

```
console.log(String(6).padStart(3, "0"));
// → 006
```

{{id split}}

On peut découper une chaîne à chaque occurrence d'une autre avec `split`
et recoller les bouts à nouveau avec `join`.

```
let phrase = "Secrétaires oiseux spécialistes du tamponnage";
let mots = phrase.split(" ");
console.log(mots);
// → ["Secrétaires ", "oiseux", "spécialistes", "du", "tamponnage"]
console.log(mots.join(". "));
// → Secrétaires.  oiseux.  spécialistes.  du.  tamponnage. 
```

{{index "repeat method"}}

Une chaîne peut être répétée avec la méthode `repeat` qui crée une
nouvelle chaîne qui contient de multiples copies de la chaîne d'origine collées
ensemble.

```
console.log("LA".repeat(3));
// → LALALA
```

{{index ["length property", "for string"], [string, indexing]}}

Nous avons déjà mentionné la propriété `length` des chaînes. On
peut accéder aux caractères individuels d'une chaîne comme aux
éléments d'un tableau (en respectant un schéma que nous verrons au
[chapitre ?](higher_order#code_units)).

```
let chaine = "abc";
console.log(chaine.length);
// → 3
console.log(chaine[1]);
// → b
```

{{id rest_parameters}}

## Paramètres du reste

{{index "Math.max function"}}

Il peut s'avérer utile qu'une fonction accepte un nombre arbitraire d'((arguments)).
Par exemple, `Math.max` trouve le plus grand de _tous_ les arguments qu'on lui fournit.

{{index "period character", "max example", spread}}

Pour définir une telle fonction, on place trois points avant le dernier
((paramètre)) de la fonction, comme ceci:

```{includeCode: strip_log}
function max(...nombres) {
  let resultat = -Infinity;
  for (let nombre of nombres) {
    if (nombre > resultat) resultat = nombre;
  }
  return resultat;
}
console.log(max(4, 1, 9, -2));
// → 9
```

Lorsqu'on invoque une telle fonction, le _((paramètre du reste))_ pointe vers
un tableau qui contient tous les arguments supplémentaires. S'il y a d'autres
paramètres avant lui, leurs valeurs ne sont pas placées dans ce tableau.
Lorsque c'est le seul paramètre, comme dans `max`, le tableau contient tous
les arguments.

{{index [function, application]}}

On peut aussi utiliser la notation préfixe à trois points pour _appeler_ une
fonction avec un tableau contenant les arguments à lui transmettre individuellement. 

```
let nombres = [5, 1, 7];
console.log(max(...nombres));
// → 7
```

Cela a pour effet de «ventiler» les éléments du tableau entre les parenthèses
servant à appeler la fonction, chaque élément devenant un argument à part entière.
Il est possible d'inclure un tableau comme cela à côté d'autres arguments, comme dans
`max(9, ...nombres, 2)`.

{{index [array, "of rest arguments"], "square brackets"}}

De même, la notation entre crochets des tableaux supporte l'opérateur trois points
pour ventiler les éléments d'un autre tableau dans celui qu'on définit.

```
let mots = ["jamais", "comprendre"];
console.log(["ne", ...mots, "complètement"]);
// → ["ne", "jamais", "comprendre", "complètement"]
```

## L'objet Math

{{index "Math object", "Math.min function", "Math.max function", "Math.sqrt function", minimum, maximum, "square root"}}

Comme vu précédement, `Math` est une sorte de sac qui regroupe des
fonctions en rapport avec les maths comme `Math.max` (maximum), `Math.min`
(minimum) ou `math.sqrt` (racine carrée).

{{index namespace, [object, property]}}

{{id namespace_pollution}}

L'objet `Math` sert à envelopper des utilitaires ayant un propos similaire.
Il n'y a qu'un objet `Math`, et il n'est presque jamais utilisé comme une
valeur. Son propos est d'offrir ce qu'il est convenu d'appeler un _espace de nommage_
de façon que les noms de ses fonctions et valeurs n'entrent pas en conflit
avec des noms que nous pourrions avoir envie d'utiliser.

{{index [binding, naming]}}

Avoir trop de noms accessibles globalement «pollue» l'espace de nommage global.
Plus il y a de noms en libre circulation, plus le risque d'en écraser un
par redéfinition est grand. Par exemple, il n'est pas improbable de vouloir
nommer `max` quelque chose dans un de nos programmes. Mais comme la fonction
de même nom de JavaScript a été soigneusement enfouie à l'intérieur de l'objet
`Math`, nous ne risquons pas de l'écraser.

{{index "let keyword", "const keyword"}}

La plupart des langages vous empêcheront, ou du moins vous mettront en garde, de
redéfinir un nom déjà utilisé. JavaScript fera cela pour une association (entre
un nom et une valeur) déclarée avec `let` ou `const` mais non, et c'est ce qui
est pervers, pour des associations prédéfinies ni pour celles déclarées
avec `var` ou `function`. 

{{index "Math.cos function", "Math.sin function", "Math.tan function", "Math.acos function", "Math.asin function", "Math.atan function", "Math.PI constant", cosine, sine, tangent, "PI constant", pi}}

Revenons à l'objet `Math`. Si vous avez besoin de faire de la trigonométrie, `Math`
peut vous aider. Il contient les fonctions `cos` (cosinus), `sin` (sinus) et `tan`
(tangente), et aussi leurs fonctions réciproques respectives `acos`, `asin` et `atan`.
Le nombre π (pi)—ou du moins la valeur la plus proche de ce nombre qui puisse tenir dans
un nombre JavaScript—s'écrit `Math.PI`. C'est une habitude bien ancrée d'écrire les noms
de valeurs constantes tout en majuscules.

```{test: no}
function pointSurUnCercleAuHasard(rayon) {
  let angle = Math.random() * 2 * Math.PI;
  return {x: rayon * Math.cos(angle),
          y: rayon * Math.sin(angle)};
}
console.log(pointSurUnCercleAuHasard(2));
// → {x: 0.3667, y: 1.966}
```

Si vous n'êtes pas très familier avec les fonctions sinus et cosinus, restez zen.
Lorsque nous les utiliserons dans ce livre, au [chapitre ?](dom#sin_cos),
je vous les expliquerai.

{{index "Math.random function", "random number"}}

L'exemple précédent utilise `Math.random`. C'est une fonction qui
renvoie un nombre (pseudo-)aléatoire compris entre zéro (inclus) et un (exclus)
à chaque fois que vous l'invoquez.

```{test: no}
console.log(Math.random());
// → 0.36993729369714856
console.log(Math.random());
// → 0.727367032552138
console.log(Math.random());
// → 0.40180766698904335
```

{{index "pseudorandom number", "random number"}}

Bien que les ordinateurs soient des machines déterministes—ils réagissent
toujours de la même façon pour une entrée donnée—il est possible de leur
faire produire des nombres qui semblent être aléatoires. Pour y parvenir,
la machine conserve et cache une certaine valeur, puis elle effectue des
calculs compliqués sur la base de celle-ci pour en créer une autre.
Elle range alors cette nouvelle valeur secrète et en dérive une autre qu'elle
renvoie. De cette façon, elle peut produire à chaque fois une nouvelle
valeur difficile à prévoir qui donne l'_illusion_ d'un nombre tiré au hasard.

{{index rounding, "Math.floor function"}}

Si on veut obtenir un entier aléatoire plutôt qu'un nombre décimal,
on peut utiliser `Math.floor` (qui supprime la partie décimale) sur le
résultat de `Math.random`.

```{test: no}
console.log(Math.floor(Math.random() * 10));
// → 2
```

En multipliant le nombre aléatoire par 10, on obtient un nombre décimal
compris entre 0 et 10 (exclus). Comme `Math.floor` le tronque, cette expression
fournit un nombre entier entre 0 et 9 (inclus) avec les mêmes chances.

{{index "Math.ceil function", "Math.round function", "Math.abs function", "absolute value"}}

Il y a encore les fonctions `Math.ceil` (pour obtenir l'entier immédiatement supérieur à un décimal donné), `Math.round` (pour arrondir à l'entier le plus proche) et `Math.abs`, qui
donne la valeur absolue d'un nombre, ce qui veut dire qu'elle change le signe d'un nombre négatif
mais qu'elle laisse un nombre positif comme il est.

## Décomposition \[_destructuring_\]

{{index "phi function"}}

Revenons à notre fonction `phi` un moment.

```{test: wrap}
function phi(tab) {
  return (tab[3] * tab[0] - tab[2] * tab[1]) /
    Math.sqrt((tab[2] + tab[3]) *
              (tab[0] + tab[1]) *
              (tab[1] + tab[3]) *
              (tab[0] + tab[2]));
}
```

{{index "destructuring binding", parameter}}

L'une des raisons pour laquelle cette fonction est assez désagréable à lire
tient à ce que le paramètre `tab` pointe vers un tableau alors qu'il serait
préférable d'avoir des variables qui pointent sur chaque _élément_ du tableau,
comme dans `let n00 = tab[0]` et ainsi de suite.
Javascript offre un moyen simple de faire cela.

```
function phi([n00, n01, n10, n11]) {
  return (n11 * n00 - n10 * n01) /
    Math.sqrt((n10 + n11) * (n00 + n01) *
              (n01 + n11) * (n00 + n10));
}
```

{{index "let keyword", "var keyword", "const keyword", [binding, destructuring]}}

Cela fonctionne aussi avec des associations nom-valeur déclarées avec `let`, `var`
ou `const`. Si vous savez que la valeur cible est un tableau, vous
pouvez utiliser des crochets à gauche du `=` pour «regarder à l'intérieur»
de la valeur-tableau, créant ainsi des associations élément par élément.

{{index [object, property], [braces, object]}}

On peut utiliser quelque chose de similaire pour les objets, en utilisant
des accolades à la place des crochets.

```
let {nom} = {nom: "Faraji", age: 23};
console.log(nom);
// → Faraji
```

{{index null, undefined}}

Il faut noter qu'essayer de décomposer `null` ou `undefined` produira une
erreur de la même manière qu'en tentant d'accéder à une propriété pour ces valeurs.

## JSON

{{index [array, representation], [object, representation], "data format", [memory, organization]}}

Pour cette bonne raison que les propriétés ne font que pointer leur valeur,
plutôt que de les contenir, les objets et les tableaux sont stockés dans la
mémoire de l'ordinateur comme une séquence de bits qui traduit les
_((adresse))s_—leur position en mémoire—de leur contenu. Ainsi,
un tableau qui contient un autre tableau se traduit par (au moins) une
zone mémoire pour le tableau interne et une autre pour le tableau externe, laquelle
contient (entre autres) un nombre binaire qui représente la position du tableau interne.

Si on souhaite sauvegarder les données dans un fichier pour plus tard ou
pour les envoyer à un autre ordinateur à travers le réseau, il va falloir
convertir d'une façon ou d'une autre ces entrecroisements d'adresses mémoire
en une description qui peut être sauvegardée ou transmise. On _pourrait_ envoyer
l'intégralité de la mémoire de l'ordinateur avec les adresses des valeurs
qui nous intéressent, du moins je le suppose,
mais ça n'a pas l'air d'être la meilleur approche du problème.

{{indexsee "JavaScript Object Notation", JSON}}

{{index serialization, "World Wide Web"}}

Ce qu'on peut faire c'est _sérialiser_ les données. Cela signifie les convertir
dans une description séquentielle. Un format de sérialisation
populaire se nomme _((JSON))_ (prononcer «Jésonne»), qui est l'acronyme
de JavaScript Objet Notation. Il est largement utilisé comme moyen de sauvegarde
des données et comme format de communication sur le web, même dans d'autres
langages que JavaScript.

{{index [array, notation], [object, creation], [quoting, "in JSON"], comment}}

Le JSON ressemble à la façon d'écrire les objets et les tableaux de JavaScript,
avec quelques restrictions. Tous les noms de propriétés doivent être délimités
par des guillemets doubles, et seules des expressions simples des données sont
permises—pas d'appel de fonctions, de variables, ou tout ce qui implique des calculs
d'une façon ou d'une autre. Les commentaires ne sont pas permis non plus en JSON. 

Une note du journal de Jacques pourrait ressembler à cela dans le format JSON:

```{lang: "application/json"}
{
  "ecureuil": false,
  "evenements": ["travailler", "se frotter contre un arbre", "pizza", "course à pied"]
}
```

{{index "JSON.stringify function", "JSON.parse function", serialization, deserialization, parsing}}

JavaScript dispose des fonctions prédéfinies `JSON.stringify` et `JSON.parse` pour
convertir les données vers et depuis ce format. La première prend une valeur
JavaScript et la renvoie dans une chaîne au format JSON. La seconde prend une telle
chaîne et la convertit en la valeur que cette chaîne encode.

```
let chaine = JSON.stringify({ecureuil: false,
                             evenements: ["week-end"]});
console.log(chaine);
// → {"ecureuil":false,"evenements":["week-end"]}
console.log(JSON.parse(chaine).evenements);
// → ["week-end"]
```

## Résumé

Les objets et les tableaux (qui sont une sorte spéciale d'objet) fournissent
des moyens de grouper plusieurs valeurs en une seule. Conceptuellement,
cela nous permet de placer un assortiment de choses en lien les unes avec
les autres dans un sac et de se balader avec le sac, plutôt que d'essayer
de tenir chacune d'elles à bout de bras.

La plupart des valeurs ont des propriétés en Javascript, `null` et `undefined`
constituant l'exception. L'accession aux propriétés s'écrit `valeur.prop` ou
`valeur["prop"]`. Les objets utilisent plutôt des noms pour leurs propriétés et
ils en ont plus ou moins par défaut. Les tableaux, en revanche, servent ordinairement
à contenir un nombre variable de valeurs semblables et utilisent des nombres
(en démarrant à 0) comme noms pour leurs propriétés.

Les tableaux ont aussi quelques propriétés nommées, comme `length` et
nombre de méthodes. Une méthode est une fonction attachée à une propriété
d'un objet et qui agit (habituellement) sur l'objet auquel elle se rattache.

On peut parcourir un tableau en utilisant une forme spéciale de boucle
`for`—`for (let element of tableau)`.

## Exercices

### La somme d'une série

{{index "summing (exercise)"}}

Dans l'[introduction](intro) de ce livre, nous avons fait allusion à
un moyen agréable de calculer la somme d'une série de nombres:

```{test: no}
console.log(somme(serie(1, 10)));
```

{{index "range function", "sum function"}}

Écrire une fonction `serie` qui prend deux arguments, `debut` et `fin`,
et renvoie un tableau contenant tous les nombres entiers depuis `debut`
jusqu'à `fin` inclus.

Ensuite, écrire une fonction `somme` qui prend un tableau de nombres
et renvoie la somme de ces nombres. Faites alors tourner le programme
exemple pour vérifier si le résultat est bien 55.

{{index "optional argument"}}

En prime, modifier votre fonction `serie` pour prendre un troisième
argument optionnel qui précise un «pas» à utiliser pour produire le tableau.
Si aucun pas n'est donné, les valeurs augmentent d'une unité, comme dans
le comportement initial. L'appel de fonction `serie(1, 10, 2)` devrait
renvoyer `[1, 3, 5, 7, 9]`. Assurez-vous qu'elle fonctionne aussi avec un pas
négatif de façon que `serie(5, 2, -1)` produise `[5, 4, 3, 2]`.

{{if interactive

```{test: no}
// Votre code ici.

console.log(serie(1, 10));
// → [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
console.log(serie(5, 2, -1));
// → [5, 4, 3, 2]
console.log(somme(serie(1, 10)));
// → 55
```

if}}

{{hint

{{index "summing (exercise)", [array, creation], "square brackets"}}

Produire un tableau se fait simplement en créant une variable associée
à `[]` (un tableau vide, neuf et frais) et en appelant sa méthode `push`
de façon répétée pour ajouter une nouvelle valeur. N'oubliez pas de renvoyer
votre tableau à la fin de la fonction.

{{index [array, indexing], comparison}}

Comme la borne supérieure est inclusive, vous aurez besoin d'utiliser
l'opérateur `<=` plutôt que `<` pour le test de fin de boucle.

{{index "arguments object"}}

Le paramètre `pas` peut être un paramètre optionnel avec la valeur
par défaut (en utilisant l'opérateur `=`) 1.

{{index "range function", "for loop"}}

Pour que `serie` puisse gérer un pas négatif, le mieux est probablement
d'écrire deux boucles séparées—une pour compter vers le haut, l'autre
pour compter vers le bas—car la comparaison qui intervient dans le test
de fin de boucle doit être `>=` plutôt que `<=` lorsqu'on compte vers
le bas.

Il pourrait être avantageux d'utiliser un pas par défaut différent
(-1 pour être précis)
lorsque le départ de la série est plus grand que sa fin. De cette façon,
`serie(5, 2)` renvoie ce qu'on attend plutôt que de s'enferrer dans une
((boucle infinie)). On peut se référer aux paramètres précédents pour définir
la valeur par défaut d'un paramètre... 

hint}}

### Retourner un tableau

{{index "reversing (exercise)", "reverse method", [array, methods]}}

Les tableaux disposent d'une méthode `reverse` dont l'effet est de renverser
l'ordre des éléments du tableau (le premier devient le dernier etc.).
Dans cet exercice, écrire deux fonctions `retournerTableau` et `retournerTableauEnPlace`.
La première prend un tableau et en produit un _nouveau_ dont les éléments sont ceux
du tableau original mais dans l'ordre inverse. La seconde, `retournerTableauEnPlace`,
fait la même chose que la première à cela près qu'elle _modifie_ le tableau fourni
en argument plutôt que d'en créer un nouveau. Aucune n'a le droit d'utiliser la 
méthode `reverse`.

{{index efficiency, "pure function", "side effect"}}

En vous souvenant des explications sur les effets de bords et les fonctions
pures du [précédent chapitre](functions#pure), quelle variante sera utilisée
dans la plupart des cas pratiques? Quelle est celle qui s'exécute le plus rapidement?

{{if interactive

```{test: no}
// Votre code ici.

console.log(retournerTableau(["A", "B", "C"]));
// → ["C", "B", "A"];
let valeurTableau = [1, 2, 3, 4, 5];
retournerTableauEnPlace(valeurTableau);
console.log(valeurTableau);
// → [5, 4, 3, 2, 1]
```

if}}

{{hint

{{index "reversing (exercise)"}}

Il y a deux façons à peu près évidentes d'implémenter `retournerTableau`.
La première est de parcourir simplement le tableau du début à la fin
et d'utiliser la méthode `unshift` sur le nouveau tableau pour insérer
chaque élément au début. La seconde est de parcourir le tableau à l'envers
et d'utiliser la méthode `push`. Parcourir un tableau à l'envers nécessite
d'adapter la boucle `for` (même si ce n'est pas très agréable à lire) de cette façon
`for (let i = tab.length - 1; i >= 0; i--)`.

{{index "slice method"}}

Retourner le tableau en place est un peu plus difficile. Il faut prendre
garde à ne pas écraser d'éléments dont vous pourriez avoir besoin plus tard
au cours du processus. Utiliser `retournerTableau`, autrement dit copier
le tableau (`tableau.slice(0)` est un bon moyen pour copier un tableau) fonctionne
mais c'est de la triche.

Le truc est d'_échanger_ le premier et le dernier élément, puis le second et
l'avant dernier, etc. On peut faire cela en parcourant seulement la moitié
du tableau (utilisez `Math.floor` pour tronquer—vous n'avez pas besoin
de toucher à l'élément du milieu s'il y a un nombre impair d'éléments)
et en échangeant l'élément de position `i` avec celui de position
`tableau.length - 1 - i`. Vous pouvez utiliser une variable locale
pour retenir brièvement l'un des deux éléments à échanger, remplacer
sa valeur avec l'autre dans le tableau, puis utiliser la valeur mémorisée
pour remplacer le dernier (cela ressemble à l'échange du contenu de deux
verres en en utilisant un troisième...).

hint}}

{{id list}}

### Une liste

{{index ["data structure", list], "list (exercise)", "linked list", array, collection}}

Les objets, vus comme un réceptacle générique de valeurs, peuvent servir à construire
toutes sortes de structures de données. Les _listes_ (à ne pas confondre avec les tableaux)
sont une structure de données très habituelle en informatique. Une liste peut être vue
comme un ensemble d'objets contenus les uns dans les autres, avec le premier qui fait
référence au second, le second qui fait référence au troisième et ainsi de suite.

```{includeCode: true}
let liste = {
  valeur: 1,
  reste: {
    valeur: 2,
    reste: {
      valeur: 3,
      reste: null
    }
  }
};
```

L'objet résultat forme une chaîne, comme ça:

{{figure {url: "img/linked-list.svg", alt: "A linked list",width: "8cm"}}}

{{index "structure sharing", [memory, structure sharing]}}

Une chose agréable avec les listes, c'est qu'elles peuvent partager
des parties de leur structure. Par exemple, si je crée deux nouvelles
valeurs `{valeur: 0, reste: liste}` et `{valeur: -1, reste: liste}` (où `liste`
fait référence à celle de l'exemple), chacune représente une liste
indépendante tout en partageant la partie de la structure formée des
trois derniers maillons de la chaîne. La liste d'origine est aussi
toujours une liste valide à trois éléments.

Écrire une fonction `tableauVersListe` qui construit une structure de liste
comme celle montrée précédemment si on lui fournit `[1, 2, 3]` comme argument.
Écrire aussi une fonction `listeVersTableau` qui produit un tableau à partir
d'une liste. Ajouter ensuite une fonction outil `ajouter`, qui prend un élément
ainsi qu'une liste et ajoute l'élément donné en tête de la liste fournie en argument.
Enfin, une fonction `nieme` qui prend une liste et un nombre et renvoie l'élément de position
donné dans la liste (avec zéro pour le premier élément) ou `undefined` si un tel élément
n'existe pas.

{{index recursion}}

Si vous ne l'avez pas déjà fait, écrire aussi une version récursive de `nieme`

{{if interactive

```{test: no}
// Votre code ici.

console.log(tableauVersListe([10, 20]));
// → {valeur: 10, reste: {valeur: 20, reste: null}}
console.log(listeVersTableau(tableauVersListe([10, 20, 30])));
// → [10, 20, 30]
console.log(ajouter(10, ajouter(20, null)));
// → {valeur: 10, reste: {valeur: 20, reste: null}}
console.log(nieme(tableauVersListe([10, 20, 30]), 1));
// → 20
```

if}}

{{hint

{{index "list (exercise)", "linked list"}}

Construire une liste se fait plus simplement en procédant de l'arrière vers l'avant.
Aussi `tableauVersListe` pourrait parcourir le tableau à l'envers (voir l'aide pour l'exercice
précédent) et, pour chaque élément, on ajouterait un objet à la liste. On peut utiliser
une variable locale pour faire référence à la liste construite jusqu'ici et utiliser
une affectation comme `liste = {valeur: x, reste: liste}` pour ajouter un élément.

{{index "for loop"}}

Pour parcourir une liste (dans `listeVersTableau` et `nieme`), une boucle `for`
déclarée comme suit peut être utile:

```
for (let maillon = liste; maillon; maillon = maillon.reste) {}
```

Pouvez-vous voir comment cela fonctionne? À chaque itération de la boucle, `maillon`
pointe sur la sous-liste courante, et le corps de boucle peut lire sa propriété
`valeur` pour obtenir l'élément courant. Lorsque le maillon pointe sur `null`, nous avons
atteint la fin de la liste et la boucle se termine.

{{index recursion}}

La version récursive de `nieme`, va, similairement, considérer une partie
plus petite de la «queue» de la liste et cela en décomptant l'index jusqu'à
ce qu'il atteigne zéro, moment où elle pourra renvoyer la propriété `valeur`
du maillon courant. Pour obtenir l'élément de liste de numéro 0, il suffit
de prendre la propriété `valeur` de son maillon de tête (elle-même!).
Pour obtenir le _N_ + 1 ième élément, prendre le *N*ième élément de la liste
et il se trouve dans sa propriété `reste`.

hint}}

{{id exercise_deep_compare}}

### Comparaison profonde

{{index "deep comparison (exercise)", [comparison, deep], "deep comparison", "== operator"}}

L'opérateur `==` compare l'identité de deux objets. Cependant, il arrive qu'on
préférerait comparer les valeurs de leurs propriétés.

Écrire une fonction `egaliteProfonde` qui prend deux valeurs et renvoie `true`
si, et seulement si, ce sont une seule et même valeurs pour des données atomiques ou,
dans le cas d'objets ayant les mêmes propriétés, si les valeurs correpondantes
sont profondément égales (avec un appel récursif à `egaliteProfonde`).

{{index null, "=== operator", "typeof operator"}}

Pour savoir si les valeurs doivent être comparées directement (utilisez l'opérateur
`===` pour cela) ou si c'est leurs propriétés qu'il faut comparer, on peut
utiliser l'opérateur `typeof`. S'il produit `"object"` pour les deux valeurs,
il faut réaliser une comparaison profonde. Mais vous allez devoir prendre garde
à une exception stupide: historiquement, et de façon accidentelle, `typeof null` produit
aussi `"object"`. 

{{index "Object.keys function"}}

La fonction `Object.keys` sera bien utile lorsque vous aurez besoin de parcourir
les propriétés d'un objet pour comparer leurs valeurs.

{{if interactive

```{test: no}
// Votre code ici.

let obj = {ceci: {est: "un"}, objet: 2};
console.log(egaliteProfonde(obj, obj));
// → true
console.log(egaliteProfonde(obj, {ceci: 1, objet: 2}));
// → false
console.log(egaliteProfonde(obj, {ceci: {est: "un"}, objet: 2}));
// → true
```

if}}

{{hint

{{index "deep comparison (exercise)", [comparison, deep], "typeof operator", "=== operator"}}

Votre test pour savoir si vous êtes face à un véritable objet ressemble
à quelque chose comme `typeof x == "object" && x != null`. Faites
attention à ne comparer les propriétés que lorsque vous avez deux
objets en arguments. Dans tous les autres cas, il vous suffit de renvoyer
immédiatement le résultat de la comparaison avec `===`.

{{index "Object.keys function"}}

Utilisez `Object.keys` pour parcourir les noms de propriétés. Vous devez tester
si les deux objets ont le même ensemble de noms de propriétés et si ces
propriétés ont des valeurs identiques. Un moyen pour faire cela est de
s'assurer que les deux objets ont le même nombre de propriétés (la
longueur de la liste des propriétés est la même). Et ensuite, en parcourant
les noms des propriétés d'un des deux objets pour les comparer,
commencez toujours par vous assurer que l'autre objet a une propriété de même nom.
S'ils ont le même nombre de propriétés et que le nom de propriété de l'un est aussi
un nom de propriété de l'autre, vous êtes sûr qu'ils ont en commun
le même ensemble de noms de propriétés.

{{index "return value"}}

Pour renvoyer la valeur correcte depuis la fonction, le mieux est de 
renvoyer immédiatement `false` lorsqu'une différence apparaît, et de
ne renvoyer `true` qu'à la toute fin de la fonction.

hint}}
