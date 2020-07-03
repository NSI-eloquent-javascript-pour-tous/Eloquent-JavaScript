{{meta {load_files: ["code/scripts.js", "code/chapter/05_higher_order.js", "code/intro.js"], zip: "node/html"}}}

# Fonctions d'ordre supérieur

{{if interactive

{{quote {author: "Master Yuan-Ma", title: "The Book of Programming", chapter: true}

Tzu-li and Tzu-ssu were boasting about the size of their latest
programs. 'Two-hundred thousand lines,' said Tzu-li, 'not counting
comments!' Tzu-ssu responded, 'Pssh, mine is almost a *million* lines
already.' Master Yuan-Ma said, 'My best program has five hundred
lines.' Hearing this, Tzu-li and Tzu-ssu were enlightened.

quote}}

if}}

{{quote {author: "C.A.R. Hoare", title: "1980 ACM Turing Award Lecture", chapter: true}

{{index "Hoare, C.A.R."}}

There are two ways of constructing a software design: One way is to
make it so simple that there are obviously no deficiencies, and the
other way is to make it so complicated that there are no obvious
deficiencies.

quote}}

{{figure {url: "img/chapter_picture_5.jpg", alt: "Letters from different scripts", chapter: true}}}

{{index "program size"}}

Un long programme est un programme coûteux, et pas seulement à cause du
temps qu'il faut pour le mettre au point. La longueur entraîne presque
toujours de la ((complexité)), et la complexité embrouille les programmeurs.
Ces programmeurs embrouillés, à leur tour, introduisent des erreurs (_((bug))s_) 
dans les programmes. Un long programme fournit beaucoup d'espace pour permettre
à ces erreurs ou bugs de se cacher, ce qui les rend difficiles à dénicher.

{{index "summing example"}}

Revenons brièvement aux deux derniers exemples de programmes de
l'introduction. Le premier se suffit à lui-même et fait six lignes.

```
let total = 0, compte = 1;
while (compte <= 10) {
  total += compte;
  compte += 1;
}
console.log(total);
```

Le second s'appuie sur deux fonctions externes et fait une ligne.

```
console.log(sum(range(1, 10)));
```

Lequel est le plus susceptible de contenir un bug? 

{{index "program size"}}

Si nous prenons en compte la taille de la défintion de `sum` et `range`,
le second programme est aussi long—même plus long que le premier. Et pourtant,
je défendrais que sa correction est plus probable.

{{index [abstraction, "with higher-order functions"], "domain-specific language"}}

Sa correction est plus probable car sa solution est exprimée dans un
((vocabulaire)) qui correspond au problème à résoudre. Sommer les nombres d'un intervalle
ne nous parle pas d'itérations et de compteur, mais d'intervalles et de somme.

Les définitions pour ce vocabulaire (les fonctions `sum` et `range`)
impliquent toujours des itérations, des compteurs et d'autres détails incidents. Mais
comme ils expriment des concepts plus simples relativement au programme dans sa globalité,
il est plus facile de ne pas se tromper.

## Abstraction

Dans le contexte de la programmation, ces sortes de mots sont
communément appelés des _((abstraction))s_. Les abstractions servent à
cacher les détails et nous rendent capables de parler des problèmes à un
plus haut niveau (ou de façon plus abstraite). 

{{index "recipe analogy", "pea soup"}}

Par analogie, comparez ces deux recettes de soupe de pois cassés. 
La première décrit les choses ainsi:

{{quote

Mettre un verre de pois cassés par personne dans un récipient. Ajouter de l'eau
jusqu'à avoir bien recouvert les pois. Laisser les pois dans l'eau
pendant au moins 12 heures. Puis, sortir les pois de l'eau et les 
mettre dans une casserole. Ajouter quatre verres d'eau par personne.
Couvrir la casserole et laisser mijoter pendant deux heures. Prendre
la moitié d'un oignon par personne. Le découper en petits morceaux avec
un couteau et ajouter aux pois . Prendre une branche de céleri par personne.
Découper en petits morceaux avec un couteau. Ajouter aux pois. Prendre
une carotte par personne. Découper en morceaux. Avec un couteau! Ajouter
aux pois. Laisser cuire 10 minutes de plus.

quote}}

Voici la seconde recette:

{{quote

Par personne: 1 verre de pois cassés, la moitié d'un oignon, une branche
de céleri et une carotte.

Laisser tremper les pois pendant 12 heures. Faire mijoter pendant deux heures
dans 4 verres d'eau (par personne). Émincer et ajouter les légumes. Laisser cuire
10 minutes de plus.

quote}}

{{index vocabulary}}

La seconde est plus courte et plus facile à interpréter. Mais vous devez connaître
un peu plus de vocabulaire de cuisine comme _tremper_, _mijoter_, _émincer_ 
et, je suppose, _légume_.

Lorsque nous programmons, nous ne pouvons nous appuyer sur tous les mots 
dont nous aurions besoin et qui nous attendent dans un dictionnaire. Ainsi,
nous avons tendance à procéder comme dans la première recette—en détaillant chaque
étape que la machine doit suivre, une par une, aveugle aux concepts 
de plus haut niveau qu'elles expriment.

{{index abstraction}}

C'est une compétence précieuse, en programmation, de se rendre compte qu'on
travaille à un niveau d'abstraction trop bas.

## Abstraire la répétition

{{index [array, iteration]}}

Les fonctions, comme nous celles que nous avons vues jusqu'ici, sont un bon moyen pour construire des abstractions.
Mais parfois, cela tourne court.

{{index "for loop"}}

Il est commun dans un programme de répéter une action un certain nombre de fois.
Vous pouvez écrire une ((boucle)) `for` pour cela, comme ceci:

```
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

Pouvons-nous abstraire «faire quelque chose _N_ fois» dans une fonction? 
Bien sûr, il est facile d'écrire une fonction qui appelle `console.log` _N_ fois.

```
function repeterLog(n) {
  for (let i = 0; i < n; i++) {
    console.log(i);
  }
}
```

{{index [function, "higher-order"], loop, [function, "as value"]}}

{{indexsee "higher-order function", "function, higher-order"}}

Mais comment s'y prendre pour faire autre chose que d'afficher des nombres?
Comme «faire quelque chose» peut être représenté par une fonction et que les
fonctions sont juste des valeurs, nous pouvons passer notre action comme une valeur (de type) fonction.

```{includeCode: "top_lines: 5"}
function repeter(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}

repeter(3, console.log);
// → 0
// → 1
// → 2
```

Nous n'avons pas forcément à passer à `repeter` une fonction prédéfinie.
Souvent, il est plus commode de créer une valeur (de type) fonction sur place.

```
let labels = [];
repeter(5, i => {
  labels.push(`Unité ${i + 1}`);
});
console.log(labels);
// → ["Unité 1", "Unité 2", "Unité 3", "Unité 4", "Unité 5"]
```

{{index "loop body", [braces, body], [parentheses, arguments]}}

L'organisation est similaire à celle d'une boucle `for`—on décrit tout d'abord
le type de boucle qu'on souhaite et ensuite on lui fournit un corps de boucle. La différence principale est que
le corps est à présent une valeur de type fonction, laquelle est enveloppée dans les
parenthèses de l'appel à `repeter`. C'est pourquoi il est nécessaire
de fermer avec une accolade fermante _et_ une parenthèse fermante. Dans les cas
comme celui-ci, où le corps est formé d'une seule courte expression, on peut même 
se passer des accolades et écrire la boucle sur une seule ligne.

## Fonctions d'ordre supérieur

{{index [function, "higher-order"], [function, "as value"]}}

Les fonctions qui opèrent sur d'autres fonctions, soit en les prenant comme arguments
soit en les renvoyant, sont appelées _fonctions d'ordre supérieur_.
Puisque nous avons déjà vu que les fonctions sont des valeurs comme les autres,
il n'y a rien de particulièrement surprenant à ce que de telles fonctions existent.
Le terme vient des ((mathématiques)), où la distinction entre les fonctions et les autres
valeurs se fait de manière plus rigoureuse.

{{index abstraction}}

Les fonctions d'ordre supérieur nous permettent d'abstraire les _actions_,
pas seulement les valeurs.  On les rencontre sous plusieurs formes.
Par exemple, on peut avoir des fonctions qui créent d'autres fonctions. 

```
function plusGrandQue(n) {
  return m => m > n;
}
let plusGrandQue10 = plusGrandQue(10);
console.log(plusGrandQue10(11));
// → true
```

Et nous pouvons aussi avoir des fonctions qui modifient d'autres fonctions.

```
function verbeux(f) {
  return (...args) => {
    console.log("a été appelée avec", args);
    let resultat = f(...args);
    console.log("appelées avec", args, ", renvoie", resultat);
    return resultat;
  };
}
verbeux(Math.min)(3, 2, 1);
// → a été appelée avec [3, 2, 1]
// → appelées avec [3, 2, 1] , renvoie 1
```

On peut même écrire des fonctions pour abstraire des structures conditionnelles.

```
function saufSi(test, alors) {
  if (!test) alors();
}

repeter(3, n => {
  saufSi(n % 2 == 1, () => {
    console.log(n, "est pair");
  });
});
// → 0 est pair
// → 2 est pair
```

{{index [array, methods], [array, iteration], "forEach method"}}

Il existe une méthode prédéfinie sur les tableaux, `forEach`, qui fournit
une sorte de boucle `for`/`of` comme une fonction d'ordre supérieur.

```
["A", "B"].forEach(l => console.log(l));
// → A
// → B
```

## Jeu de données d'écritures

Un domaine dans lequel les fonctions d'ordre supérieur excellent est le traitement de données.
Pour traiter des données, nous aurons besoin de données réelles. Ce chapitre utilisera
un ((jeu de données)) d'écritures—((systèmes d'écriture)) comme le latin, le cyrillique, ou l'arabe. 

Vous souvenez-vous d'((Unicode)) vu dans le [Chapitre ?](values#unicode), ce système qui
assigne un nombre à chaque caractère d'une langue écrite? La plupart
de ces caractères sont associés à un système d'écriture en particulier. Le standard
contient 140 différents systèmes d'écriture—81 sont toujours utilisés de nos jours, et
59 sont historiques.

Bien que je ne lise de manière fluide que les caractères latins,
j'apprécie que des gens écrivent des textes dans au moins 80 autres
écritures, même si je suis incapable de reconnaître les caractères. Par
exemple, voici un échantillon d'écriture ((tamoul)):

{{figure {url: "img/tamil.png", alt: "Tamil handwriting"}}}

{{index "SCRIPTS data set"}}

L'exemple du ((jeu de données)) contient quelques informations à propos des 140 systèmes d'écriture
définies par Unicode. Il est disponible dans le [bac à sable](https://eloquentjavascript.net/code#5)
de codage pour ce chapitre[
([_https://eloquentjavascript.net/code#5_](https://eloquentjavascript.net/code#5))]{if
book} via `SCRIPTS`. Il contient un tableau d'objets où chaque objet décrit une écriture.

```{lang: "application/json"}
{
  name: "Coptic",
  ranges: [[994, 1008], [11392, 11508], [11513, 11520]],
  direction: "ltr",
  year: -200,
  living: false,
  link: "https://en.wikipedia.org/wiki/Coptic_alphabet"
}
```

Un tel objet nous donne le nom de l'écriture, les intervalles qu'Unicode lui
assigne, la direction dans laquelle on l'écrit, la date approximative de son
apparition, si elle est toujours utilisée et un lien pour obtenir plus d'informations.
La direction peut être `"ltr"` (_left to right_) - gauche vers droite, `"rtl"`
(_right to left_) - droite vers gauche (la manière dont les Arabes et les Hébreux écrivent),
ou `"ttb"` (_top to bottom_) haut vers bas (comme dans l'écriture mongole). 

{{index "slice method"}}

La propriété `ranges` contient un tableau d'intervalles de caractères Unicode.
Chaque intervalle est un tableau à deux éléments contenant une borne basse et une borne haute.
Chaque code de caractère situé dans l'un de ces intervalles est associé à une écriture.
La ((borne)) basse est inclusive (le code 994 est un caractère copte) mais pas la borne haute (le code 1008 non).

## Filtrer les tableaux

{{index [array, methods], [array, filtering], "filter method", [function, "higher-order"], "predicate function"}}

Pour trouver les écritures du jeu de données qui sont toujours en usage,
la fonction suivante peut être utile. Elle élimine les éléments d'un tableau qui
invalident le test.

```
function filtrer(tableau, test) {
  let a_conserver = [];
  for (let element of tableau) {
    if (test(element)) {
      a_conserver.push(element);
    }
  }
  return a_conserver;
}

console.log(filtrer(SCRIPTS, script => script.living));
// → [{name: "Adlam", …}, …]
```

{{index [function, "as value"], [function, application]}}

La fonction utilise un argument nommé `test`, une valeur-fonction, pour
boucher un «trou» dans le traitement—la sélection des éléments à collecter.

{{index "filter method", "pure function", "side effect"}}

Remarquez comment la fonction `filtrer`, plutôt que de supprimer les éléments
du tableau fourni, produit un nouveau tableau avec les éléments qui valident le test.
Cette fonction est _pure_. Elle ne modifie pas le tableau qu'elle reçoit.

De même que `forEach`, il y a une méthode ((standard)) `filter` pour les tableaux qui fait la même chose.
L'exemple détaille la fonction seulement afin de mettre en évidence son fonctionnement interne.
Par la suite, nous utiliserons la fonction standard comme suit:

```
console.log(SCRIPTS.filter(e => e.direction == "ttb"));
// → [{name: "Mongolian", …}, …]
```

{{id map}}

## Transformer avec map

{{index [array, methods], "map method"}}

Mettons que nous ayons un tableau d'objets qui représentent des systèmes d'écriture,
obtenu en filtrant le tableau `SCRIPTS` d'une certaine façon. Mais nous préférerions
avoir un tableau de noms, qui est plus facile à inspecter.

{{index [function, "higher-order"]}}

La fonction `associer` (_map_) transforme un tableau en appliquant une fonction à chacun
de ses éléments et en construisant un nouveau tableau à partir des valeurs renvoyées.
Le nouveau tableau aura la même taille que celui fourni en argument, mais son contenu
aura pris une nouvelle forme suite à l'application de la fonction.

```
function associer(tableau, transformer) {
  let images = [];
  for (let element of tableau) {
    images.push(transformer(element));
  }
  return images;
}

let ecritures_rtl = SCRIPTS.filter(e => e.direction == "rtl");
console.log(associer(ecritures_rtl, e => e.name));
// → ["Adlam", "Arabic", "Imperial Aramaic", …]
```

Comme `forEach` et `filter`, `map`, qui fait la même chose qu'`associer`, est une méthode standard des tableaux.

## Agréger (résumer) avec reduce

{{index [array, methods], "summing example", "reduce method"}}

Une autre tâche commune avec les tableaux est de produire une valeur à partir de ses éléments.
Notre exemple récurrent, sommer une collection de nombres, est une instance de cette tâche.
Un autre exemple est de trouver le système d'écriture qui contient le plus de caractères.

{{indexsee "fold", "reduce method"}}

{{index [function, "higher-order"], "reduce method"}}

L'opération d'ordre supérieur qui représente ce cas d'utilisation est appelée
_reduce_ (parfois aussi appelée _fold_). Elle produit une valeur en prenant un
élément du tableau de façon répétée et en le combinant avec la valeur courante.
Lorsqu'on somme des nombres, on commence avec le nombre zéro et chaque élément du tableau
est ajouté à la somme. 

Les paramètres de `reduce` sont, le tableau mis à part, une fonction
pour combiner et une valeur de départ. Cette fonction est un peu moins
évidente que `filter` et `map`, alors regardez-la de près:

```
function reduire(tableau, combiner, vdepart) {
  let vcourante = vdepart;
  for (let elt of tableau) {
    vcourante = combiner(vcourante, elt);
  }
  return vcourante;
}

console.log(reduire([1, 2, 3, 4], (a, b) => a + b, 0));
// → 10
```

{{index "reduce method", "SCRIPTS data set"}}

La méthode standard pour les tableaux `reduce`, qui correspond évidemment
à cette fonction, ajoute une facilité. Si votre tableau contient au
moins un élément, vous pouvez ne pas préciser l'argument `start` (qui correspond à `vdepart` dans l'exemple).
Dans ce cas, la méthode prend comme valeur de départ le premier élément du tableau et commence
la réduction à partir du second.

```
console.log([1, 2, 3, 4].reduce((a, b) => a + b));
// → 10
```

{{index maximum, "characterCount function"}}

Pour utiliser `reduce` (deux fois) afin de trouver le système d'écriture
qui contient le plus de caractères, on peut écrire quelque chose comme:

```
function compterCaracteres(ecriture) {
  return ecriture.ranges.reduce((compte, [de, a]) => {
    return compte + (a - de);
  }, 0);
}

console.log(SCRIPTS.reduce((a, b) => {
  return compterCaracteres(a) < compterCaracteres(b) ? b : a;
}));
// → {name: "Han", …}
```

La fonction `compterCaracteres` réduit les intervalles associés à un système d'écriture
en ajoutant le nombre de caractères qu'ils contiennent. Remarquez l'utilisation d'une déconstruction de liste dans la fonction
de réduction. Le deuxième appel à `reduce` utilise cela pour trouver le système
d'écriture qui contient le plus grand nombre de caractères en comparant de façon répétée deux systèmes
d'écriture et en renvoyant le plus étendu.

Le système d'écriture Han contient plus de 89.000 caractères dans le standard
Unicode, ce qui en fait de loin le plus grand système d'écriture du jeu de données.
Han est un système d'écriture (parfois) utilisé par la Chine, le Japon et les Coréens.
Leurs écritures partagent beaucoup de caractères, bien qu'ils les écrivent de façon un peu différente.
Le consortium Unicode (basé aux USA) a décidé de les traiter comme un système d'écriture unique
pour économiser des points de codes. On appelle cela l'_unification Han_ et cela ne plaît pas toujours aux principaux intéressés.

## Composabilité

{{index loop, maximum}}

Voici comment nous aurions pu écrire l'exemple précédent (trouver le plus
grand jeu de caractères) sans recourir à des fonctions d'ordre supérieur.
Le code obtenu n'est pas beaucoup moins lisible.

```{test: no}
let plusGrand = null;
for (let ecriture of SCRIPTS) {
  if (plusGrand == null ||
      characterCount(plusGrand) < characterCount(ecriture)) {
    plusGrand = ecriture;
  }
}
console.log(plusGrand);
// → {name: "Han", …}
```

Il y a quelques affectations supplémentaires, et le programme fait quatre lignes de plus.
Mais cela reste très lisible.

{{index "average function", composability, [function, "higher-order"], "filter method", "map method", "reduce method"}}

{{id average_function}}

Les fonctions d'ordre supérieur commencent à briller lorsqu'on a besoin
de _composer_ (d'enchaîner) les opérations. Par exemple, écrivons du code afin de trouver
l'âge moyen d'origine des systèmes d'écritures vivants et morts du
jeu de données. 

```
function moyenne(tab) {
  return tab.reduce((a, b) => a + b) / tab.length;
}

console.log(Math.round(moyenne(
  SCRIPTS.filter(e => e.living).map(e => e.year))));
// → 1165
console.log(Math.round(moyenne(
  SCRIPTS.filter(e => !e.living).map(e => e.year))));
// → 204
```

On observe donc que les langues mortes d'Unicode sont, en moyenne, plus anciennes
que les langues vivantes. Certes cette statistique n'est ni surprenante ni très enrichissante.
Mais j'espère que vous conviendrez que le code utilisé pour la produire n'est pas
difficile à lire. On peut le voir comme une sorte de pipeline ou chaîne de traitement: on commence avec tous
les systèmes d'écritures, puis on sélectionne ceux qui sont estimés vivants (ou les morts), on prend leur
année d'origine, on calcule leur moyenne, qu'on arrondit enfin.

On pourrait bien sûr coder ce traitement à l'aide d'une seule «grosse» ((boucle)).

```
let total = 0, compte = 0;
for (let ecriture of SCRIPTS) {
  if (ecriture.living) {
    total += ecriture.year;
    compte += 1;
  }
}
console.log(Math.round(total / compte));
// → 1165
```

Mais il est un peu plus difficile de voir ce qui est calculé et comment.
Et comme les résultats intermédiaires n'ont pas de signification propre,
il faudrait bien plus de travail pour en extraire quelque chose comme `moyenne`
dans une fonction séparée.

{{index efficiency, [array, creation]}}

Si l'on se penche sur ce que l'ordinateur fait effectivement, ces deux approches
sont aussi assez différentes. La première va produire de nouveaux tableaux en
exécutant `filter` et `map`, alors que la seconde calcule simplement quelques
nombres, ce qui représente moins de travail. La plupart du temps, on peut se permettre
d'utiliser la démarche la plus lisible, mais si vous travaillez sur de très gros tableaux,
et que vous faites cela plusieurs fois, alors l'approche détaillée et moins abstraite devrait
accélérer le temps d'exécution. 

## Chaînes et codes de caractères


{{index "SCRIPTS data set"}}

Une utilisation possible du jeu de données serait de reconnaître le jeu de caractères utilisé par une portion de texte.
Partons à la découverte d'un programme qui traite ce problème.

Souvenez-vous que chaque système d'écriture a un tableau formé d'intervalles des codes de caractères qui lui sont associés.
Ainsi, étant donné le code d'un caractère, nous pourrions utiliser une fonction comme
celle-ci pour trouver le système d'écriture correspondant (s'il y en a un):

{{index "some method", "predicate function", [array, methods]}}

```{includeCode: strip_log}
function jeuDeCaracteres(code) {
  for (let ecriture of SCRIPTS) {
    if (ecriture.ranges.some(([de, a]) => {
      return code >= de && code < a;
    })) {
      return ecriture;
    }
  }
  return null;
}

console.log(jeuDeCaracteres(121));
// → {name: "Latin", …}
```

La méthode `some` est une autre fonction d'ordre supérieur. Elle prend une fonction de test - un prédicat -
et vous indique si ce test renvoie `true` pour au moins un élément du tableau.

{{id code_units}}

Mais comment obtenir le code d'un caractère d'une chaîne?


Dans le [Chapitre ?](values) j'ai indiqué que les chaînes de caractères de JavaScript
sont encodées comme une séquence de nombres sur 16 bits. On les appelle des unités de code.
Un code caractère du jeu Unicode devait, à l'origine, tenir dans une telle unité de 16 bits
(ce qui vous donne un peu plus de 65.000 caractères). Lorsqu'il devint clair que cela ne
serait pas assez, beaucoup de gens étaient réticents à l'idée d'utiliser plus de mémoire par caractère.
Pour pallier ce problème, ((UTF-16)), le format utilisé par les chaînes de caractères
JavaScript fut inventé. Il décrit les caractères les plus courants en utilisant une unité de
code de 16 bits et une paire de telles unités pour les autres.

{{index error}}

UTF-16 est, aujourd'hui, considéré comme une mauvaise idée. 
Il semble avoir été conçu pour induire en erreur.
Il est facile d'écrire un programme qui fait comme si les unités de code et les
caractères sont la même chose. Et si votre langue n'utilise pas de caractères
nécessitant deux unités de code, cela fonctionnera en apparence.
Mais dès que quelqu'un se mettra à utiliser ce programme avec un texte
contenant des caractères chinois moins habituels, il échouera.
Heureusement, avec l'introduction des emojis, tout le monde s'est mis à utiliser
des caractères nécessitant deux unités de code, et la nécessité de bien gérer
ces problèmes est à présent le lot de tous.

{{index [string, length], [string, indexing], "charCodeAt method"}}

Malheureusement, des opérations très simples sur les chaînes de caractères de JavaScript,
comme obtenir leur longueur via la propriété `length` ou accéder à leur contenu en
utilisant les crochets, ne portent que sur les unités de code.

```{test: no}
// Deux caractères emoji, un cheval et une chaussure
let chevalChaussure = "🐴👟";
console.log(chevalChaussure.length);
// → 4
console.log(chevalChaussure[0]);
// → (Moitié de caractère invalide)
console.log(chevalChaussure.charCodeAt(0));
// → 55357 (Code de la moitié du caractère)
console.log(chevalChaussure.codePointAt(0));
// → 128052 (Code correct pour l'emoji cheval)
```

{{index "codePointAt method"}}

La méthode `charCodeAt` de javascript renvoie une unité de code,
et non un code de caractère complet. La méthode `codePointAt`, ajoutée
plus tard, renvoie, elle, le code d'un caractère complet. Donc, nous pourrions
l'utiliser pour récupérer les caractères d'une chaîne. Mais l'argument fourni
à `codePointAt` est toujours un index dans la séquence d'unités de code. Ainsi,
pour parcourir tous les caractères d'une chaîne, nous avons toujours besoin de
savoir si le caractère courant correspond à une unité de code
ou à deux pour gérer l'index.

{{index "for/of loop", character}}

Dans le chapitre précédent, j'ai indiqué qu'une boucle `for`/`of` peut
aussi être utilisée avec les chaînes. Comme `codePointAt`, ce type de
boucle fut introduit à un moment où les gens étaient bien conscients des
problèmes induits par UTF-16. Lorsqu'on l'utilise pour parcourir une chaîne,
elle vous donne directement les caractères, non les unités de code.

```
let roseDragon = "🌹🐉";
for (let car of roseDragon) {
  console.log(car);
}
// → 🌹
// → 🐉
```

Si vous avez un caractère (qui est une chaîne formée d'une ou de deux
unités de code), vous pouvez utiliser `codePointAt(0)` pour obtenir
son code complet.

## Reconnaître les textes


{{index "SCRIPTS data set", "countBy function", [array, counting]}}

Nous disposons d'une fonction `jeuDeCaracteres` et d'un moyen de parcourir
correctement les caractères. La prochaîne étape est de compter les
caractères qui appartiennent à chaque langue. L'abstraction suivante du concept de dénombrement
va être bien utile pour ce cas:

```{includeCode: strip_log}
function denombrerPar(collection, nomGroupe) {
  let comptes = [];
  for (let elt of collection) {
    let nom = nomGroupe(elt);
    let vu = comptes.findIndex(c => c.nom == nom);
    if (vu == -1) {
      comptes.push({nom, nombre: 1});
    } else {
      comptes[vu].nombre++;
    }
  }
  return comptes;
}

console.log(denombrerPar([1, 2, 3, 4, 5], n => n > 2));
// → [{nom: false, nombre: 2}, {nom: true, nombre: 3}]
```

La fonction `denombrerPar` attend une collection (tout ce qui peut être parcouru
avec une boucle `for`/`of`) et une fonction qui produit un nom de groupe
pour un élément donné. Elle renvoie un tableau d'objets, dont chacun contient le nom
d'un groupe et le nombre d'éléments de ce groupe.

{{index "findIndex method", "indexOf method"}}

Elle utilise une autre méthode de tableau—`findIndex`. Elle ressemble à `indexOf`,
mais au lieu de chercher une valeur particulière, elle trouve la première valeur
pour laquelle la fonction de test donnée renvoie `true`. Comme `indexOf`, elle
renvoie -1 lorsqu'aucun élément de la sorte n'est trouvé.

{{index "textScripts function", "Chinese characters"}}

En utilisant `denombrerPar`, nous pouvons écrire la fonction qui nous indiquera
quelles écritures sont utilisées dans une portion de texte.

```{includeCode: strip_log, startCode: true}
function texteEcritures(texte) {
  let ecritures = denombrerPar(texte, car => {
    let ecriture = jeuDeCaracteres(car.codePointAt(0));
    return ecriture ? ecriture.name : "none";
  }).filter(({nom}) => nom != "none");

  let total = ecritures.reduce((n, {nombre}) => n + nombre, 0);
  if (total == 0) return "Aucune écriture trouvée";

  return ecritures.map(({nom, nombre}) => {
    return `${Math.round(nombre * 100 / total)}% ${nom}`;
  }).join(", ");
}

console.log(texteEcritures('英国的狗说"woof", 俄罗斯的狗说"тяв"'));
// → 61% Han, 22% Latin, 17% Cyrillique
```

{{index "characterScript function", "filter method"}}

La fonction commence par compter les caractères par nom, en utilisant
`jeuDeCaracteres` pour leur associer un nom ou en produisant la chaîne `"none"`
pour les caractères qui ne font partie d'aucun langage. L'appel à `filter`
élimine les objets dont le nom est `"none"` du tableau final puisque nous
ne sommes pas intéressés par ces caractères.

{{index "reduce method", "map method", "join method", [array, methods]}}

Pour être en mesure de calculer les ((pourcentage))s, nous avons d'abord besoin
de connaître le nombre de caractères d'une écriture donnée,
ce que nous pouvons calculer avec `reduce`. Si pour une langue donnée, aucun
caractère n'a été trouvé, la fonction renvoie une chaîne particulière. Autrement,
elle transforme chaque objet de décompte en une chaîne lisible avec `map` puis elle les combine
avec `join` pour n'en former qu'une. 

## Résumé

Être en mesure de passer des fonctions à d'autres fonctions est un aspect
profondément utile de JavaScript. Cela nous permet d'écrire des fonctions
qui modélisent des traitements contenant des portions à compléter. Le code
qui utilise ces fonctions peut les compléter en leur fournissant
d'autres fonctions.

Les tableaux fournissent un nombre important de méthodes d'ordre supérieur.
On peut utiliser `forEach` pour parcourir les éléments d'un tableau. La méthode
`filter` renvoie un nouveau tableau qui ne contient que les éléments qui sastisfont
une fonction prédicat (un test). Transformer un tableau en passant tous ses éléments
à travers une fonction se fait avec `map`. On peut utiliser `reduce` pour combiner
tous les éléments d'un tableau en une seule valeur. La méthode `some` teste si au moins
un élément du tableau valide un certain prédicat. Et `findIndex` trouve la position du
premier élément qui valide un prédicat donné. 

## Exercices

### Mettre à plat

{{index "flattening (exercise)", "reduce method", "concat method", [array, flattening]}}

Utiliser la méthode `reduce` en la combinant avec la méthode `concat` de
façon à «mettre à plat» un tableau de tableaux en un seul tableau dont les
éléments sont ceux des tableaux internes d'origine.

{{if interactive

```{test: no}
let tableaux = [[1, 2, 3], [4, 5], [6]];
// Votre code ici.
// → [1, 2, 3, 4, 5, 6]
```
if}}

### Votre propre boucle

{{index "your own loop (example)", "for loop"}}

Écrire une fonction d'ordre supérieur `boucle` qui produit un effet
similaire à une boucle `for`. Elle prend une valeur, une fonction pour le test,
une fonction de mise à jour et une fonction pour son corps. À chaque
itération, elle commence par appliquer le test à la valeur courante de boucle
et s'arrête s'il renvoie `false`. Ensuite elle applique la fonction du corps
de boucle en lui passant la valeur courante de boucle. Enfin, elle appelle la 
fonction de mise à jour pour créer la prochaine valeur de boucle et passe à 
l'itération suivante.

En définissant la fonction, vous pouvez utiliser une boucle habituelle pour
effectuer les itérations.

{{if interactive

```{test: no}
// Votre code ici.

boucle(3, n => n > 0, n => n - 1, console.log);
// → 3
// → 2
// → 1
```

if}}

### Tous - Au moins un

{{index "predicate function", "everything (exercise)", "every method", "some method", [array, methods], "&& operator", "|| operator"}}

De façon analogue à la méthode `some`, les tableaux ont aussi une méthode
`every`. Celle-là renvoie `true` lorsque la fonction fournie en argument
renvoie `true` pour _tous_ les éléments du tableau. En un sens, `some` est
une version de l'opération `||` qui agit sur les tableaux, et `every` une
version de l'opérateur `&&`.

Implémenter `every` comme une fonction qui prend un tableau et une
fonction prédicat (un test) en entrée. Écrire deux versions, l'une
en utilisant une boucle et l'autre en utilisant la méthode `some`.

{{if interactive

```{test: no}
function every(tab, test) {
  // Votre code ici.
}

console.log(every([1, 3, 5], n => n < 10));
// → true
console.log(every([2, 4, 16], n => n < 10));
// → false
console.log(every([], n => n < 10));
// → true
```

if}}

{{hint

{{index "everything (exercise)", "short-circuit evaluation", "return keyword"}}

Comme l'opérateur `&&`, la méthode `every` peut arrêter de parcourir et
d'évaluer les éléments dès lors qu'elle en a trouvé un qui ne valide pas le test.
Ainsi, la version avec boucle peut en sortir prématurément—avec `break` ou
`return` dès qu'elle a évalué un élément pour lequel la fonction prédicat renvoie
`false`. Si la boucle va au bout sans trouver un tel élément, nous savons que tous
les éléments ont validé le test et nous devrions renvoyer `true`.

Pour implémenter `every` sur la base de `some`, on peut appliquer la _((loi de De Morgan))_,
laquelle énonce que `a && b` a la même valeur que `!(!a || !b)`. Il est possible de la 
généraliser aux tableaux en disant que tous les éléments valident le test s'il n'y a pas
d'élément qui l'invalide.

hint}}

### Direction d'écriture dominante

{{index "SCRIPTS data set", "direction (writing)", "groupBy function", "dominant direction (exercise)"}}

Écrire une fonction qui calcule la direction d'écriture dominante d'un texte.
Souvenez-vous que chaque objet du jeu de données a une propriété `direction`
qui peut prendre les valeurs `"ltr"` (_left to right_ gauche vers droite), `"rtl"` (_right to left_ droite vers gauche),
ou `"ttb"` (_top_to_bottom haut vers bas).

{{index "characterScript function", "countBy function"}}

La direction dominante est la direction de la majorité des caractères qui ont un
objet associé dans le jeu de données. Les fonctions `jeuDeCaracteres` et `denombrerPar` définies
plus tôt dans le chapitre vous seront probablement bien utiles dans ce contexte.

{{if interactive

```{test: no}
function directionDominante(texte) {
  // Votre code ici.
}

console.log(directionDominante("Hello!"));
// → ltr
console.log(directionDominante("Hey, مساء الخير"));
// → rtl
```
if}}

{{hint

{{index "dominant direction (exercise)", "textScripts function", "filter method", "characterScript function"}}

Votre solution pourrait beaucoup ressembler à la première moitié
de l'exemple `texteEcritures`. Vous devez à nouveau compter les
caractères selon un critère basé sur `jeuDeCaracteres` puis éliminer
la partie du résultat qui fait référence à des caractères inintéressants (sans écriture).

{{index "reduce method"}}

Trouver la direction qui correspond au plus grand nombre de caractères
peut être fait avec `reduce`. Si comment faire n'est pas clair, reportez-vous
à l'exemple donné plus tôt dans ce chapitre, celui où `reduce` avait été utilisé
pour trouver l'écriture avec le plus grand nombre de caractères.

hint}}
