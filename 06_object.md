{{meta {load_files: ["code/chapter/06_object.js"], zip: "node/html"}}}

# La Vie Secrète Des Objets

{{quote {author: "Barbara Liskov", title: "Programming with Abstract Data Types", chapter: true}

An abstract data type is realized by writing a special kind of program
[…] which defines the type in terms of the operations which can be
performed on it.

quote}}

{{index "Liskov, Barbara", "abstract data type"}}

{{figure {url: "img/chapter_picture_6.jpg", alt: "Picture of a rabbit with its proto-rabbit", chapter: framed}}}

Le [Chapitre ?](data) a présenté les objets de JavaScript.
Dans la culture informatique, on entend souvent parler de
_((programmation orientée objet))_, un ensemble de techniques
qui placent les objets (et des concepts associés)
au centre de l'organisation d'un programme.

Bien qu'on ne s'accorde pas sur une définition précise, la programmation
orienté objet organise la structure de la plupart des langages de programmation,
JavaScript y compris. Ce chapitre décrit la façon dont ces
idées peuvent s'appliquer en JavaScript.

## Encapsulation

{{index encapsulation, isolation, modularity}}

L'idée principale en programmation orientée objet est de diviser
un programme en fragments plus petits et de faire en sorte que
chaque fragment soit responsable de la gestion de son propre état.

De cette façon, le détail des connaissances sur le fonctionnement
interne d'un tel fragment reste localisé dans ce fragment. Quelqu'un
qui travaille sur un autre partie du programme n'a pas bessoin de se
souvenir ou même d'être au courant de ces détails. Même si l'organisation
interne du fragment est modifiée, seul le code qui l'environne
immédiatement doit être mis à jour. 

{{id interface}}
{{index [interface, object]}}

Les différents fragments d'un tel programme intéragissent les uns
avec les autres par l'intermédiaire d'_interfaces_, un nombre limité
de fonctions et/ou variables qui fournissent des fonctionnalités utiles à
un plus haut niveau d'abstraction, en cachant les détails de leur implémentation.

{{index "public properties", "private properties", "access control", [method, interface]}}

Ces fragments de programme sont réalisés en utilisant des ((objet))s.
Leur interface est formée de méthodes et de propriétés. Les propriétés
qui font partie de l'interface sont dites _publiques_. Les autres, que le
code extérieur ne devrait pas toucher, sont dites _privées_.

{{index "underscore character"}}

La plupart des langages fournissent un moyen de distinguer les propriétés
publiques des propriétés privées tout en limitant les interactions aux premières.
JavaScript, à nouveau, adopte une approche minimaliste et ne fournit pas,
du moins pour l'instant, un tel moyen. Cela devrait changer avec une 
prochaine version du langage. 

Même si le langage ne permet pas ces distinctions, les programmeurs JavaScript
utilisent ces idées avec succès. Habituellement, les interfaces sont
décrites par des commentaires ou dans la documentation. Il est aussi
courant d'indiquer qu'une propriété est privée en faisant précéder son nom
par un underscore (`_`).

Séparer l'interface de l'implémentation est une bonne idée. On
appelle cela l'_((encapsulation))_.

{{id obj_methods}}

## Méthodes

{{index "rabbit example", method, [property, access]}}

Les méthodes ne sont, ni plus ni moins, que des propriétés qui font référence
à des valeurs-fonctions. Voici un exemple de méthode.

```
let lapin = {};
lapin.parler = function(blabla) {
  console.log(`Le lapin dit '${blabla}'`);
};

lapin.parler("Je suis vivant.");
// → Le lapin dit 'Je suis vivant.'
```

{{index "this binding", "method call"}}

Habituellement, une méthode doit faire quelque chose avec l'objet
sur lequel elle a été appelée. Lorsqu'une fonction est appelée comme
une méthode—recherchée comme une propriété et immédiatement appelée, comme
dans `object.method()`—la variable `this`, disponible dans le corps de la
fonction, pointe automatiquement l'objet sur lequel elle a été appelée.

```{includeCode: "top_lines:6", test: join}
function parler(blabla) {
  console.log(`Le lapin ${this.type} dit '${blabla}'`);
}
let lapinBlanc = {type: "blanc", parler};
let lapinAffame = {type: "affamé", parler};

lapinBlanc.parler("Par mes oreilles et mes moustaches, " +
                  "je vais être en retard!");
// → Le lapin blanc dit 'Par mes oreilles et mes
//    moustaches, je vais être en retard!'
lapinAffame.parler("Je pourrais engloutir une carotte à l'instant.");
// → Le lapin affamé dit 'Je pourrais engloutir
//    une carotte à l'instant.'
```

{{id call_method}}

{{index "call method"}}

Vous pouvez imaginer que `this` est un ((paramètre)) additionnel
auquel on transmet l'objet implicitement lors de l'appel.
Il est possible de faire cela explicitement, en utilisant la
méthode `call` des fonctions, laquelle prend la valeur de `this`
comme premier argument et traite ses autres arguments
de la façon habituelle.

```
parler.call(lapinAffame, "Burp!");
// → Le lapin affamé dit 'Burp!'
```

Puisque chaque fonction dispose de sa propre variable `this` et que sa
valeur dépend de la façon dont elle a été appelée, il n'est pas possible
d'accéder à la valeur de `this` de la porté englobante avec une fonction
définie avec le mot clé `function`.

{{index "this binding", "arrow function"}}

Les fonctions «flèches» sont différentes—elle ne dispose pas de leur
propre variable `this` ce qui leur permet d'accéder au `this` de la
portée englobante. Ainsi, on peut faire quelque chose comme cela, où
`this` fait réfèrence à celui de la fonction `normaliser`:

```
function normaliser() {
  console.log(this.coords.map(n => n / this.taille));
}
normaliser.call({coords: [0, 2, 3], taille: 5});
// → [0, 0.4, 0.6]
```

{{index "map method"}}

Si j'avais écrit la fonction argument de `map` en utilisant le mot
clé `function`, cela n'aurait pas fonctionné.

{{id prototypes}}

## Prototypes

{{index "toString method"}}

Regarder attentivement.

```
let vide = {};
console.log(vide.toString);
// → function toString(){…}
console.log(vide.toString());
// → [object Object]
```

{{index magic}}

J'ai fait surgir une propriété d'un objet vide. Magique!

{{index [property, inheritance], [object, property]}}

Pas vraiment en fait. J'ai simplement un peu mis en évidence
la façon dont les objets JavaScript fonctionnent. En plus de leurs
propriétés, la plupart des objets ont aussi un _((prototype))_. Un
prototype est un autre objet utilisé comme source de propriétés par défaut.
Lorsqu'on tente d'accéder à une propriété qu'un objet n'a pas,
elle est recherchée dans son prototype, puis dans le prototype de ce 
prototype et ainsi de suite.

{{index "Object prototype"}}

Mais alors quel est le ((prototype)) de notre objet `vide`? C'est
l'ancêtre des prototypes, l'entité derrière la plupart
des objets, `Object.prototype`.

```
console.log(Object.getPrototypeOf({}) ==
            Object.prototype);
// → true
console.log(Object.getPrototypeOf(Object.prototype));
// → null
```

{{index "getPrototypeOf function"}}

Comme vous le devinez, `Object.getPrototypeOf` renvoie le prototype d'un
objet.

{{index "toString method"}}

La relation «est le prototype de» entre les objets JavaScript forme une
structure ((arborescente)) dont la racine est `Object.prototype`. Cet objet
fourni quelques méthodes partagées par tous les objets, comme `toString`,
qui convertit un objet en une chaîne de caractères qui le représente.

{{index inheritance, "Function prototype", "Array prototype", "Object prototype"}}

Beaucoup d'objets n'ont pas directement `Object.prototype` comme ((prototype))
mais un autre objet qui leur fourni un ensemble différent de propriétés par défaut.
Les fonctions sont dérivées de `Function.prototype` et les tableaux de `Array.prototype`.

```
console.log(Object.getPrototypeOf(Math.max) ==
            Function.prototype);
// → true
console.log(Object.getPrototypeOf([]) ==
            Array.prototype);
// → true
```

{{index "Object prototype"}}

Un tel objet prototype a, à son tour, un prototype, souvent `Object.prototype`,
de telle sorte que ses propriétés comme `toString` sont toujours disponibles bien
qu'indirectement.

{{index "rabbit example", "Object.create function"}}

On peut utiliser `Object.create` pour créer un objet avec un ((prototype))
spécifique.

```
let lapinProto = {
  parler(blabla) {
    console.log(`Le lapin ${this.type} dit '${blabla}'`);
  }
};
let lapinTueur = Object.create(lapinProto);
lapinTueur.type = "tueur";
lapinTueur.parler("YAHAAAAA!");
// → Le lapin tueur dit 'YAHAAAAA!'
```

{{index "shared property"}}

Une propriété comme `parler(blabla)` dans une expression objet est un
raccourci pour définir une méthode. Cela crée une propriété nommée `parler`
et lui associe une fonction comme valeur.

Le lapin «proto» sert de conteneur pour les propriétés partagées
par tous les lapins. Un objet lapin individuel, comme le lapin tueur,
ne contient que les propriétés qui ne s'appliquent qu'à lui—son type
dans ce cas—et il dérive les propriétés partagées de son prototype.

{{id classes}}

## Classes

{{index "object-oriented programming"}}

Le système de ((prototype)) de JavaScript peut s'interpréter comme un moyen
de réaliser un concept de la programmation orientée objet appelé _((classe))_.
Une classe définit l'organisation d'un _type_ d'objet—en précisant quelles
sont ses méthodes et propriétés.
Un tel objet est dit être une _((instance))_ de la classe.

{{index [property, inheritance]}}

Les prototypes sont utiles pour définir des propriétés dont les valeurs
sont partagées par toutes les instances d'une classe, comme des ((méthode))s.
Les propriétés qui varient avec l'instance, comme la propriété `type` de
nos lapins, doivent être stockées directement dans les objets eux-mêmes.

{{id constructors}}

Ainsi, pour créer une instance d'une classe donnée, on doit produire
un objet qui dérive d'un prototype approprié, mais on doit _aussi_
s'assurer qu'il dispose des propriétés (dont la valeur est propre à l'objet)
que les instances de cette classe sont supposées avoir.
C'est précisément le rôle d'un _((constructeur))_.

```
function produireLapin(type) {
  let lapin = Object.create(lapinProto);
  lapin.type = type;
  return lapin;
}
```

{{index "new operator", "this binding", "return keyword", [object, creation]}}

JavaScript fournit un moyen pour définir plus simplement ce genre de
fonction. Si vous placer le mot clé `new` avant un appel de fonction,
la fonction est interprétée comme un constructeur. Cela signifie
qu'un objet avec le prototype approprié est automatiquement produit et lié
à la variable `this` dans la fonction puis renvoyé lorsque la fonction se
termine.

{{index "prototype property"}}

L'objet prototype utilisé pour la construction des objets est trouvé
en prenant la propriété `prototype` de la fonction constructeur.

{{index "rabbit example"}}

```
function Lapin(type) {
  this.type = type;
}
Lapin.prototype.parler = function(blabla) {
  console.log(`Le lapin ${this.type} dit '${blabla}'`);
};

let lapinBizarre = new Lapin("bizarre");
```

{{index constructor}}

Les constructeurs (toutes les fonctions en fait) ont automatiquement
une propriété nommée `prototype` liée par défaut à un objet vide
qui dérive de `Object.prototype`. On peut le remplacer avec un nouvel
objet si on veut. Ou on peut ajouter des propriétés à l'objet de base,
comme dans l'exemple.

{{index capitalization}}

Par convention, les noms des constructeurs ont leur première lettre
en majuscule de façon à pouvoir les distinguer facilement des autres
fonctions.

{{index "prototype property", "getPrototypeOf function"}}

Il est important de bien comprendre la distinction entre la façon
dont un prototype est associé à un constructeur (à travers sa propriété `prototype`)
et celle par laquelle les objets _ont_ un prototype (qu'on peut
trouver avec `Object.getPrototypeOf`). Le prototype effectif d'un
constructeur est `Function.prototype` puisqu'un contructeur est
une fonction. Sa _propriété_ `prototype` pointe vers le prototype
utilisée pour les instances créées à travers lui.

```
console.log(Object.getPrototypeOf(Lapin) ==
            Function.prototype);
// → true
console.log(Object.getPrototypeOf(lapinBizarre) ==
            Lapin.prototype);
// → true
```

## Notation Class

Ainsi les ((classe))s JavaScript sont les fonctions ((constructeur))s
avec une propriété ((prototype)). C'est de cette façon qu'elles fonctionnent
et, jusqu'en 2015, il fallait les écrire comme cela. Aujourd'hui, nous disposons
d'une notation moins lourde.

```{includeCode: true}
class Lapin {
  constructor(type) {
    this.type = type;
  }
  parler(blabla) {
    console.log(`La lapin ${this.type} dit '${blabla}'`);
  }
}

let lapinTueur = new Lapin("tueur");
let lapinNoir = new Lapin("noir");
```

{{index "rabbit example", [braces, class]}}

Le mot clé `class` débute une ((déclaration de classe)) qui permet de
définir un constructeur et un ensemble de méthodes au même endroit.
On peut écrire autant de méthodes que souhaitées entre les crochets de
la déclaration. Celle qui est nommée `constructor`  joue un rôle particulier.
Elle fournie la fonction constructeur effective et sera liée au nom (de classe)
`Lapin`. Les autres seront placées dans le prototype de ce constructeur.
Ainsi, la déclaration de classe qui précède est équivalente à la définition
du constructeur de la section précédente. Elle est juste plus agréable à lire.

{{index ["class declaration", properties]}}

Les déclarations de classe, pour l'instant, permettent seulement d'ajouter
des _méthodes_—des propriétés associées à des valeurs fonctions—au ((prototype)).
Cela n'est pas très pratique lorsqu'on veut stocker d'autre sortes
de valeurs là dedans.
La prochaine version du langage devrait améliorer ce point. Pour l'instant,
on peut créer de telles propriétés en manipulant directement le prototype après
avoir défini la classe.

Comme `function`, on peut utiliser `class` aussi bien dans des instructions
que dans des expressions. Lorsqu'on l'utilise comme une expression, la valeur
produite est le constructeur sans que celui-ci soit associé à un nom.
On peut omettre le nom de classe dans une telle expression.

```
let objet = new class { obtenirMot() { return "bonjour"; } };
console.log(objet.obtenirMot());
// → bonjour
```

## Surcharge de propriétés dérivées

{{index "shared property", overriding, [property, inheritance]}}

Lorsqu'on ajoute une propriété à un objet, qu'elle soit présente dans
son prototype ou non, elle est ajoutée à l'objet _lui-même_.
Si il y avait déjà une propriété de même nom dans son prototype,
cette propriété n'affecte plus l'objet, elle est à présent masquée
par la propriété ajoutée.

```
Lapin.prototype.dents = "petites";
console.log(lapinTueur.dents);
// → petites
lapinTueur.dents = "longues, aiguisées et sanglantes";
console.log(lapinTueur.dents);
// → longues, aiguisées et sanglantes
console.log(lapinNoir.dents);
// → petites
console.log(Lapin.prototype.dents);
// → petites
```

{{index [prototype, diagram]}}

Le diagramme qui suit dépeint la situation après avoir exécuté cette portion
de code. Les ((prototype))s de `Lapin` et `Object` sont en arrière plan de
`lapinTueur` afin de fournir une valeur par défaut aux propriétés qui ne
sont pas trouvées dans l'objet lui-même.

{{figure {url: "img/rabbits.svg", alt: "Rabbit object prototype schema",width: "8cm"}}}

{{index "shared property"}}

Surcharger des propriétés qui font partie du prototype peut être très utile.
Comme le montre l'exemple des dents de lapin, la surcharge peut servir à donner
des valeurs particulières à certaines propriétés de quelques instances d'une classe,
tout en permettant aux instances ordinaires d'avoir une valeur par défaut pour ces
propriétés.

{{index "toString method", "Array prototype", "Function prototype"}}

La surcharge est aussi utilisée pour permettre aux prototypes des fonctions
standards et des tableaux d'avoir une méthode `toString` plus adaptée à ces
objets que celle du prototype de `Object`.

```
console.log(Array.prototype.toString ==
            Object.prototype.toString);
// → false
console.log([1, 2].toString());
// → 1,2
```

{{index "toString method", "join method", "call method"}}

Appeler `toString` sur un tableau donne un résultat similaire à celui
obtenu en appelant `.join(",")` dessus—cela place des virgules
entre les valeurs du tableau. Appeler directement `Object.prototype.toString`
avec un tableau produit une chaîne différente. Cette fonction ne sait rien
des tableaux, elle produit donc simplement le mot _object_ suivi du nom
de type le tout entre crochets.

```
console.log(Object.prototype.toString.call([1, 2]));
// → [object Array]
```

## Tableaux associatifs - Maps

{{index "map method"}}

Nous avons déjà vu le mot _map_ dans le [chapitre précédent](higher_order#map),
il désignait une opération qui transforme un conteneur en appliquant
une fonction à chacun de ses éléments. Bien que cela puisse être un
peu perturbant, c'est aussi le nom d'une structure de données classique
en programmation.

{{index "map (data structure)", "ages example", ["data structure", map]}}

Le nom _map_ (tableau associatif) désigne une structure de données
qui fait correspondre des valeurs (appelées _clés_ dans ce contexte)
à d'autres valeurs. Par exemple, on pourrait vouloir faire correspondre
des âges à des noms.  On peut utiliser des objets pour cela.

```
let ages = {
  Boris: 39,
  Liang: 22,
  Julie: 62
};

console.log(`Julie a ${ages["Julie"]} ans`);
// → Julie a 62 ans
console.log("Connait-on l'âge de Jacques?", "Jacques" in ages);
// → Connait-on l'âge de Jacques? false
console.log("Est-ce que l'âge de toString est connu?",
   "toString" in ages);
// → Est-ce que l'âge de toString est connu? true
```

{{index "Object.prototype", "toString method"}}

Ici, les noms de propriétés de l'objet représentent des noms de gens et les
valeurs associées leurs âges. Mais nous n'avons certainement
pas voulu parler de l'âge de toString dans notre tableau associatif. Encore une fois,
comme les objets dérive de `Object.prototype`, tout se passe comme si la propriété
était bien là. 

{{index "Object.create function", prototype}}

Aussi, utiliser des objets bruts comme des tableaux associatifs est
dangereux. Il y a plusieurs moyens pour éviter ce problème. tout d'abord,
on peut créer des objets _sans_ prototype. Pour cela, il suffit de 
passer `null` à `Object.create` et l'objet résultant ne dérive plus
de `Object.prototype` ce qui lui permet d'être utilisé comme un tableau
associatif sans problème.

```
console.log("toString" in Object.create(null));
// → false
```

{{index [property, naming]}}

Les noms de propriétés des objets doivent-être des chaînes. Si on a
besoin d'un tableau associatif dont les clés ne peuvent être facilement
converties en chaînes—comme des objets—on ne peut pas utiliser un objet
comme tableau associatif.

{{index "Map class"}}

Bien heureusement, JavaScript fournit une classe `Map` qui a été conçue
précisément pour cela. elle permet de créer des tableaux associatifs
dont les clés sont arbitraires.

```
let ages = new Map();
ages.set("Boris", 39);
ages.set("Liang", 22);
ages.set("Julie", 62);

console.log(`Julie a ${ages.get("Julie")} ans`);
// → Julie a 62 ans
console.log("Connait-on l'âge de Jacques?", ages.has("Jacques"));
// → Connait-on l'âge de Jacques? false
console.log(ages.has("toString"));
// → false
```

{{index [interface, object], "set method", "get method", "has method", encapsulation}}

Les méthodes `set`, `get` et `has` font partie de l'interface de l'objet
`Map`. Écrire une structure de données qu'on puisse rapidement mettre à jour
et dans laquelle on puisse faire des recherches efficaces, même lorsqu'elle
groupe un grand nombre de valeurs, est une chose difficile. Mais nous
n'avons pas à nous en soucier. Quelqu'un d'autre s'en est occupé pour nous,
et il nous suffit d'utiliser cette simple interface pour profiter de son travail.

{{index "hasOwnProperty method", "in operator"}}

Si vous disposez d'un objet ordinaire et que vous avez besoin de l'utiliser comme
un tableau associatif, il est utile de savoir que `Object.keys` renvoie seulement
les propriétés propres à l'objet, en excluant celles de son prototype.
Comme alternative à l'opérateur `in`, on peut utiliser la méthode `hasOwnProperty`
qui ignore le prototype de l'objet.

```
console.log({x: 1}.hasOwnProperty("x"));
// → true
console.log({x: 1}.hasOwnProperty("toString"));
// → false
```

## Polymorphisme

{{index "toString method", "String function", polymorphism, overriding, "object-oriented programming"}}

Lorqu'on invoque la fonction `String` (qui convertit une valeur en chaîne)
sur un objet, elle va appeler la méthode `toString` sur cet objet
afin d'essayer d'en obtenir une chaîne appropriée.
J'ai déjà indiqué que certains des prototypes standards
définissent leur propre version de `toString` de façon à pouvoir produire
une chaîne qui contient des informations plus utiles que `"[object Object]"`.
On peut aussi le faire soi-même.

```{includeCode: "top_lines: 3"}
Lapin.prototype.toString = function() {
  return `un lapin ${this.type}`;
};

console.log(String(lapinNoir));
// → un lapin noir
```

{{index "object-oriented programming", [interface, object]}}

Il s'agit d'une instance d'une idée puissante. Lorsqu'un morceau de code
prévoit de travailler avec des objets qui disposent d'une certaine
interface—dans ce cas, une méthode `toString`—n'importe quelle sorte
d'objets qui supportent cet interface pourra être manipulé par ce code, sans
qu'il cesse de fonctionner.

Cette technique est appelée _polymorphisme_. Un code polymorphe peut travailler
avec des objets de structures différentes pourvu qu'ils supportent l'interface
que le code attend.

{{index "for/of loop", "iterator interface"}}

J'ai déjà mentionné, dans le [chapitre ?](data#for_of_loop) qu'une boucle
`for`/`of` peut parcourir différentes sortes de structures de données.
Cela est rendu possible par polymorphisme—une telle boucle attend que la
structure de données respecte une interface spécifique, ce que font les
tableaux et les chaînes. Nous pouvons aussi ajouter cette interface à nos
propres objets! Mais avant de pouvoir le faire, nous devons comprendre la
notion de symbole.

## Symboles

Différentes interfaces peuvent utilisées un même nom de propriété pour
faire différentes choses. Par exemple, rien ne m'empêche de définir
une interface dans laquelle la méthode `toString` serait supposée convertir
un objet en un morceau de fil d'écosse. Il ne serait alors pas possible
pour un objet de respecter cette interface et celle de l'utilisation
standard de `toString`.

Ce serait une mauvaise idée, et ce problème n'est pas très courant.
La plupart des programmeurs JavaScript n'y pensent même pas. Mais les
concepteurs du langage, dont le _travail_ est de penser à ce genre de
choses, ont développé une solution pour ce problème.

{{index "Symbol function", [property, naming]}}

Lorsque j'ai prétendu que les noms de propriété sont des chaînes, ce n'était
pas tout à fait précis. Ils le sont le plus souvent, mais ils peuvent aussi
être des _((symbole))s_. Les symboles sont des valeurs créées avec la fonction
`Symbol`. Contrairement aux chaînes, un symbole fraîchement créé est
unique—impossible de créer un même symbole deux fois.

```
let sym = Symbol("nom");
console.log(sym == Symbol("nom"));
// → false
Lapin.prototype[sym] = 55;
console.log(lapinNoir[sym]);
// → 55
```

La chaîne passée à `Symbol` est retranscrite lorsqu'on converti
un symbole en une chaîne de façon à pouvoir le reconnaître plus facilement
lorsque, par exemple, on souhaite l'afficher dans la console. Mais elle
n'a pas de signification particulière—plusieurs symboles peuvent avoir
le même nom.

Être à la fois unique et utilisable comme nom de propriétés permet
aux symboles de définir des interfaces qui peuvent cohabitées pacifiquement
à côté d'autres propriétés sans avoir à se soucier de leurs noms.

```{includeCode: "top_lines: 1"}
const toStringSymbol = Symbol("toString");
Array.prototype[toStringSymbol] = function() {
  return `${this.length} cm de fil d'écosse bleu`;
};

console.log([1, 2].toString());
// → 1,2
console.log([1, 2][toStringSymbol]());
// → 2 cm de fil d'écosse bleu
```

{{index [property, naming]}}

On peut inclure des noms symboliques de propriétés dans les expressions
d'objets et de classe en les plaçant entre crochets. Cela a pour effet
d'évaluer ces noms de propriétés, un peu comme l'accès au propriétés par
l'intermédiaires des crochets, et nous permet de faire référence à la valeur
associée au symbole.

```
let objetChaine = {
  [toStringSymbol]() { return "une corde de jute"; }
};
console.log(objetChaine[toStringSymbol]());
// → une corde de jute
```

## l'interface des itérables

{{index "iterable interface", "Symbol.iterator symbol", "for/of loop"}}

L'objet mentionné dans une boucle `for`/`of` doit être (un) _iterable_.
Cela veut dire qu'il dispose d'une méthode nommée avec le symbole
`Symbol.iterator` (une valeur symbolique définie par le langage et stockée comme
une propriété de la fonction `Symbol`).

{{index "iterator interface", "next method"}}

Lorsqu'on l'appelle, cette méthode devrait renvoyer un objet qui implémente
une deuxième interface, _iterator_. C'est cet objet qui gère effectivement
l'itération. Pour cela, il doit disposer d'une méthode `next` qui renvoie le prochain
résultat. Ce résultat devrait être un objet avec une propriété `value` qui fournie
la prochaine valeur, s'il y en a une, ainsi qu'une propriété `done` qui devrait
être `true` lorsqu'il n'y a plus de valeur à parcourir et `false` autrement. 

Remarquez que les noms de propriétés `next`, `value` et `done` sont des
chaînes, non des symboles. Seul `Symbol.iterator`, qui sera probablement
ajoutée à _beaucoup_ d'objets différents, est effectivement un symbole.

On peut utiliser cette interface directement nous-même.

```
let okIterator = "OK"[Symbol.iterator]();
console.log(okIterator.next());
// → {value: "O", done: false}
console.log(okIterator.next());
// → {value: "K", done: false}
console.log(okIterator.next());
// → {value: undefined, done: true}
```

{{index "matrix example", "Matrix class", [array, "as matrix"]}}

{{id matrix}}

Implémentons une structure de données itérable. Nous construirons une
classe _Matrice_ pour représenter un tableau à deux dimensions.

```{includeCode: true}
class Matrice {
  constructor(largeur, hauteur, element = (x, y) => undefined) {
    this.largeur = largeur;
    this.hauteur = hauteur;
    this.contenu = [];

    for (let y = 0; y < hauteur; y++) {
      for (let x = 0; x < largeur; x++) {
        this.contenu[y * largeur + x] = element(x, y);
      }
    }
  }

  get(x, y) {
    return this.contenu[y * this.largeur + x];
  }
  set(x, y, valeur) {
    this.contenu[y * this.largeur + x] = valeur;
  }
}
```

La classe stocke son contenu dans un simple tableau contenant _largeur_ × _hauteur_
éléments. Les éléments sont rangés ligne par ligne, ainsi, par exemple,
le troisième élément de la cinquième ligne est (en utilisant la numérotation
à partir de 0) stocké à la position 4 × _largeur_ + 2.

La fonction constructeur attend une largeur, une hauteur et une fonction optionnelle
`element` qui sera utilisée pour remplir initialement le tableau.
Il y a aussi deux méthodes `get` et `set` afin de récupérer et de mettre
à jour les éléménts de la matrice.

Lorsqu'on parcourt une matrice, on souhaite souvent récupérer aussi bien
la valeur de l'élément courant que sa position, aussi notre itérateur
produira des objets avec les propriétés `x`, `y` et `valeur`.

{{index "MatrixIterator class"}}

```{includeCode: true}
class IterateurMatrice {
  constructor(matrice) {
    this.x = 0;
    this.y = 0;
    this.matrice = matrice;
  }

  next() {
    if (this.y == this.matrice.hauteur) return {done: true};

    let value = {x: this.x,
                 y: this.y,
                 valeur: this.matrice.get(this.x, this.y)};
    this.x++;
    if (this.x == this.matrice.largeur) {
      this.x = 0;
      this.y++;
    }
    return {value, done: false};
  }
}
```

La classe suit la progression du parcours de la matrice selon ses
propriétés `x` et `y`. La méthode `next` commence par vérifier si le
bas de la matrice a été atteint. Si ce n'est pas le cas, elle _commence_
par créer l'objet qui détient la valeur courante _puis_ met à jour
sa position, en se positionnant sur la ligne suivante si nécessaire.

Configurons la classe `Matrice` de façon à la rendre itérable. Tout au long
du livre, il m'arrivera de manipuler le prototype pour pouvoir ajouter
une méthode à une classe «après coup», pour que les fragments de code restent
cours et concis. Dans un programme réel, où il n'est pas utile de découper
le code en petits bouts, on déclarerait ces méthodes directement dans la classe.

```{includeCode: true}
Matrice.prototype[Symbol.iterator] = function() {
  return new IterateurMatrice(this);
};
```

{{index "for/of loop"}}

Nous pouvons à présent parcourir une matrice avec la boucle `for`/`of`.

```
let matrice = new Matrice(2, 2, (x, y) => `valeur ${x},${y}`);
for (let {x, y, valeur} of matrice) {
  console.log(x, y, valeur);
}
// → 0 0 valeur 0,0
// → 1 0 valeur 1,0
// → 0 1 valeur 0,1
// → 1 1 valeur 1,1
```

## Propriétés statiques et accesseurs: Getters et Setters

{{index [interface, object], [property, definition], "Map class"}}

Les interfaces sont principalement constituées de méthodes, mais il
n'y a pas de problèmes à y inclure des propriétés qui ne correspondent
pas à des valeurs-fonctions. Par exemple, les objets `Map` ont une
propriété `size` qui donne le nombre de clés d'un tel objet.

Il n'est pas même nécessaire pour un tel objet de stocker effectivement
la valeur d'une telle propriété dans l'instance. Même les propriétés auxquels
on accède directement peuvent induire un appel à une méthode. Ces méthodes
sont ce qu'on appelle des _((getter))s_ et elles sont définies en précédant
le nom de méthode par le mot clé `get` dans une expression d'objet ou dans
une déclaration de classe.

```{test: no}
let tailleVariable = {
  get taille() {
    return Math.floor(Math.random() * 100);
  }
};

console.log(tailleVariable.taille);
// → 73
console.log(tailleVariable.taille);
// → 49
```

{{index "temperature example"}}

À chaque fois qu'on accède en lecture à la propriété `taille` de cet objet,
la méthode associée est invoquée. On peut faire quelque chose de similaire
lorsqu'une propriété est accédée en écriture, en utilisant un _((setter))_.

```{test: no, startCode: true}
class Temperature {
  constructor(celsius) {
    this.celsius = celsius;
  }
  get fahrenheit() {
    return this.celsius * 1.8 + 32;
  }
  set fahrenheit(valeur) {
    this.celsius = (valeur - 32) / 1.8;
  }

  static depuisFahrenheit(valeur) {
    return new Temperature((valeur - 32) / 1.8);
  }
}

let temp = new Temperature(22);
console.log(temp.fahrenheit);
// → 71.6
temp.fahrenheit = 86;
console.log(temp.celsius);
// → 30
```

La classe `Temperature` nous permet de lire et d'écrire la température
aussi bien en degrés ((Celsius)) qu'en degré ((Fahrenheit)), mais en interne
la température n'est stockée qu'en Celsius tout en étant automatiquement
convertie lorsqu'on lit ou modifie la propriété `fahrenheit`.

{{index "static method"}}

Il arrive qu'on souhaite attacher certaines propriétés directement sur le
constructeur plutôt que sur son prototype. Une telle méthode ne pourra
pas avoir accès aux instances de la classe mais permet, par exemple,
d'être utilisée pour disposer de moyens supplémentaires pour créer
des instances.

À l'intérieur d'une déclaration de classe, les méthodes dont le nom
est précédé par le mot clé `static` sont stockées sur le constructeur.
Pour cette raison, la classe `Temperature` nous permet d'écrire
`Temperature.depuisFahrenheit(100)` pour créer une température en
utilisant des degrés Fahrenheit.

## Héritage

{{index inheritance, "matrix example", "object-oriented programming", "SymmetricMatrix class"}}

Certaines matrices ont la particularité d'être _symétriques_. Si vous
appliquez à une telle matrice une symétrie miroir par rapport
à sa diagonale «descendante» (haut gauche vers bas droite),
elle reste la même. Autrement dit, la valeur située à la position
_x_, _y_ est toujours la même que celle située en _y_, _x_.

Imaginez que nous ayons besoin d'une structure de données similaire
à `Matrice` mais qui s'assure que la matrice est et reste symétrique.
On pourrait l'écrire en partant de zéro, mais cela impliquerait d'écrire
un code très proche de celui qu'on a déjà écrit pour `Matrice`.

{{index overriding, prototype}}

Le système de prototype de JavaScript rend possible la création d'une
_nouvelle_ classe, semblable à une ancienne, mais avec de nouvelles
définitions pour certaines de ses propriétés. Le prototype de la nouvelle
classe dérive de celui de l'ancienne mais ajoute une nouvelle définition
pour, mettons, la méthode `set`.

Dans le vocabulaire de la programmation orientée objet, on appelle cela
l'_((héritage))_. La nouvelle classe hérite des propriétés et du
comportement de l'ancienne.

```{includeCode: "top_lines: 17"}
class MatriceSym extends Matrice {
  constructor(taille, element = (x, y) => undefined) {
    super(taille, taille, (x, y) => {
      if (x < y) return element(y, x);
      else return element(x, y);
    });
  }

  set(x, y, valeur) {
    super.set(x, y, valeur);
    if (x != y) {
      super.set(y, x, valeur);
    }
  }
}

let matrice = new MatriceSym(5, (x, y) => `${x},${y}`);
console.log(matrice.get(2, 3));
// → 3,2
```
L'utilisation du mot clé `extends` sert à indiquer que cette classe
ne doit pas se baser sur le prototype par défaut `Object` mais plutôt sur une
autre classe qu'on appelle la _((classe mère))_ (ou _superclasse_).
La classe dérivée est appelée _((classe fille))_ (ou _sous-classe_).

Pour initialiser une instance de `MatriceSym`, le constructeur invoque celui
de la classe mère à travers le mot clé `super`. C'est indispensable car si
ce nouvel objet doit se comporter (dans les grandes lignes) comme une `Matrice`,
il va avoir besoin des propriétés dont dispose une instance de matrice. Pour
s'assurer que la matrice est symétrique, le constructeur enveloppe la fonction
`element` afin d'échanger les coordonnées d'un élément sous la diagonale.

La méthode `set` utilise à nouveau `super`, pas pour invoquer le constructeur
cette fois, mais pour appeler une méthode spécifique parmi l'ensemble de celles
de la classe mère. Nous redéfinissons `set` tout en souhaitant réutiliser
le comportement initial. Comme `this.set` fait référence à la _nouvelle_
méthode `set`, l'appeler ne fonctionnerait pas. À l'intérieur d'une méthode
d'une classe, `super` fourni un moyen d'invoquer une méthode qui a été
définie dans la classe mère. 

L'héritage nous permet, à peu de frais, de construire des structures de données
légèrement différentes, à partir d'une autre prise pour base.
C'est l'un des concepts fondamentaux de la philosophie orientée
objet, avec l'encapsulation et le polymorphisme. Cependant, tandis
que ces deux derniers sont considérés comme de brillantes idées,
l'héritage est plus controversé.

{{index complexity, reuse, "class hierarchy"}}

Alors que l'encapsulation et le polymorphisme peuvent servir à _séparer_
des fragments de code les uns des autres, en réduisant l'imbrication du
programme dans son ensemble, l'héritage introduit un couplage
(une dépendance) fort(e) entre les classes, ce qui accroît
l'imbrication. Lorsqu'une classe hérite
d'une autre, on a souvent besoin d'en savoir davantage sur son fonctionnement
pour pouvoir l'utiliser simplement. L'héritage peut être un outil très
utile, je l'utilise ici et là dans mes propres programmes. Mais
ce ne devrait pas être la première technique à considérer pour un problème donné,
et il vaudrait mieux éviter de rechercher activement des opportunités
de construire des hiérarchie de classes (des familles arborescentes de classes).

## Opérateur instanceof

{{index type, "instanceof operator", constructor, object}}

Il est parfois intéressant de savoir si un objet a été dérivé d'une classe
spécifique. Pour cela, JavaScript fourni l'opérateur binaire `instanceof`.

```
console.log(
  new MatriceSym(2) instanceof MatriceSym);
// → true
console.log(new MatriceSym(2) instanceof Matrice);
// → true
console.log(new Matrice(2, 2) instanceof MatriceSym);
// → false
console.log([1] instanceof Array);
// → true
```

{{index inheritance}}

L'opérateur remonte la hiérachie des types, ce qui explique qu'une
`MatriceSym` est aussi une instance de `Matrice`. L'opérateur peut
aussi être appliqué aux constructeurs standards comme `Array`. Presque
tous les objets sont des instances d'`Object`.

## Résumé

Ainsi, les objets font plus que de simplement regrouper leur propres
propriétés. Ils ont des prototypes qui sont d'autres objets et
se comportent comme s'ils avaient directement les propriétés de leur
prototype, et celles du prototype de celui-ci etc. Les objets simples
ont `Object.prototype` pour prototype.

Les constructeurs, des fonctions dont le nom débute
ordinairement par une majuscule, peuvent-être utilisés avec l'opérateur
`new` pour créer de nouveaux objets. Ces nouveaux objets auront pour
prototype l'objet associé à la propriété `prototype` de leur constructeur.
On peut mettre cela à profit pour permettre à des objets d'un même type de
partager les valeurs de certaines de leurs propriétés. 
On dispose de la notation `class` qui nous donne un moyen commode
pour définir un constructeur et son prototype.

On peut définir des accesseurs (getters et setters) pour invoquer
secrètement des méthodes à chaque fois qu'on accède en lecture ou
en écriture à certaines propriétés de l'objet.
Les méthodes statiques sont des méthodes stockées sur le constructeur
de la classe plutôt que sur son prototype.

Étant donné un objet et un constructeur, l'opérateur `instanceof` indique
si l'objet est une instance de ce constructeur.

Une chose intéressante à faire avec des objets est de les munir d'une
interface et de dire à tout le monde de ne communiquer avec eux qu'à
travers cette interface. Les détails additionnels qui servent à réaliser
effectivement ces objets sont alors encapsulés, cachés derrière l'interface.

Plus d'un type peut proposer la même interface. Du code écrit pour
utiliser une interface particulière sait automatiquement comment
travailler avec nombres d'objets différents qui respectent cette
interface. On appelle cela le _polymorphisme_.

Lorsqu'on implémente plusieurs classes qui ne diffèrent que par
quelques détails, il peut être utile d'écrire de nouvelles classes
comme des classes filles d'une classe existante, de façon à hériter
d'une partie de son comportement.

## Exercices

{{id exercise_vector}}

### A vector type

{{index dimensions, "Vec class", coordinates, "vector (exercise)"}}

Écrire une ((classe)) `Vec` qui représente un vecteur d'un espace à deux dimensions.
Construire un vecteur demande deux paramètres `x` et `y` (nombres) qui devraient
être sauvegardés dans des propriétés de même nom.

{{index addition, subtraction}}

Donner au prototype de `Vec` deux méthodes, `plus` et `moins`, lesquelles
attendent un autre vecteur en argument et renvoie un nouveau vecteur dont
les coordonnées sont obtenues en faisant la somme ou la différence
des coordonnées _x_ et _y_ des deux vecteurs (`this` et celui fournit en argument) 

Ajouter un getter `norme`, au prototype, qui calcule la longueur
du vecteur—c'est-à-dire, la distance du point (_x_, _y_) à l'origine (0, 0). 

{{if interactive

```{test: no}
// Votre code ici.

console.log(new Vec(1, 2).plus(new Vec(2, 3)));
// → Vec{x: 3, y: 5}
console.log(new Vec(1, 2).moins(new Vec(2, 3)));
// → Vec{x: -1, y: -1}
console.log(new Vec(3, 4).norme);
// → 5
```
if}}

{{hint

{{index "vector (exercise)"}}

Jetez un oeil à l'exemple de la classe `Lapin` pour vous remémorer
l'allure d'une déclaration de classe si vous ne vous en souvenez plus.

{{index Pythagoras, "defineProperty function", "square root", "Math.sqrt function"}}

Ajouter un getter au prototype du constructeur consiste à placer le mot `get` devant
le nom de méthode. Pour calculer la distance de (0, 0) à (_x_, _y_), on peut
utiliser le théorème de Pythagore qui dit que le carré de la distance
que nous cherchons est égale à la somme du carré de _x_ et de celui de _y_.
Ainsi, [√(x^2^ + y^2^)]{if html}[[$\sqrt{x^2 + y^2}$]{latex}]{if tex}
est le nombre voulu. La racine carrée se calcule en utilisant `Math.sqrt`.

hint}}

### Ensembles

{{index "groups (exercise)", "Set class", "Group class", "set (data structure)"}}

{{id groups}}

L'environnement standard de JavaScript fournit encore une autre structure
de données appelée `Set` (ensemble). De la même manière qu'une instance
de `Map`, un ensemble est une collection de valeurs. À la différence de
`Map`, il n'associe pas d'autres valeurs à celles-ci—il se contente de
surveiller si une valeurs fait ou non partie de l'ensemble. Une valeur ne peut
faire partie d'un ensemble qu'une fois—l'ajouter à nouveau n'a aucun
effet.

{{index "add method", "delete method", "has method"}}

Écrire une classe `Ensemble`. Similairement à `Set`, elle dispose des méthodes
`ajouter` (_add_), `supprimer` (_delete_) et `contient` (_has_).
Son constructeur produit un ensemble vide, `ajouter` ajoute une valeur à
l'ensemble (mais seulement si cette valeur ne s'y trouve pas déjà),
`supprimer` supprime son argument de l'ensemble (si il y était) et
`contient` renvoie un booléen qui indique si son argument est un élément
de l'ensemble.

{{index "=== operator", "indexOf method"}}

Utilisez l'opérateur `===` ou quelque chose d'équivalent comme `indexOf` pour
savoir si deux valeurs sont identiques.

{{index "static method"}}

Ajoutez à la classe une méthode statique `depuis` qui prend un objet
itérable en argument et crée un ensemble formé de toutes les valeurs
obtenues en le parcourant.

{{if interactive

```{test: no}
class Ensemble {
  // Votre code ici.
}

let ens = Ensemble.depuis([10, 20]);
console.log(ens.contient(10));
// → true
console.log(ens.contient(30));
// → false
ens.ajouter(10);
ens.supprimer(10);
console.log(ens.contient(10));
// → false
```

if}}

{{hint

{{index "groups (exercise)", "Group class", "indexOf method", "includes method"}}

La façon la plus simple de faire cela est de ranger les éléments
de l'ensemble dans un tableau associé à une propriété de l'instance.
Les méthodes de tableux `includes` ou `indexOf` peuvent être utilisées
pour vérifier si une valeur donnée se trouve déjà dans le tableau.

{{index "push method"}}

Le ((constructeur)) de votre classe peut associer à une propriété appelée
`collection` un tableau vide. Lorsque `ajouter` est appelée, elle doit
d'abord vérifier si la valeur fournie se trouve déjà dans le tableau et
si ce n'est pas le cas, elle peut utiliser `push` pour y insérer la valeur.

{{index "filter method"}}

Supprimer un élément d'un tableau, dans `supprimer`, est moins évident mais
vous pouvez utiliser `filter` pour créer un nouveau tableau sans cette valeur.
N'oubliez pas de mettre à jour la propriété qui fait référence au
tableau de façon à la faire pointer sur celui obtenu après filtrage.

{{index "for/of loop", "iterable interface"}}

La méthode statique `depuis` peut utiliser une boucle `for`/`of` pour parcourir
les valeurs de l'objets itérable et appeler `ajouter` pour les insérer
dans le nouvel ensemble.

hint}}

### Ensemble itérable

{{index "groups (exercise)", [interface, object], "iterator interface", "Group class"}}

{{id group_iterator}}

Rendre itérable la classe `Ensemble` de l'exercice précédent. Reportez-vous
à la section sur l'interface itérateur vue plus tôt dans le chapitre si vous
n'avez plus les idées claires sur la forme exacte de cette interface.

Si vous avez utilisé un tableau pour représenter les éléments de l'ensemble,
ne vous contentez pas de renvoyer l'itérateur obtenu en appelant la méthode
`Symbol.iterator` du tableau. Cela fonctionnerait, mais viderait l'exercice
de son propos.

Ne vous préoccupez pas du comportement étrange que votre itérateur
pourrait avoir lorsque l'ensemble est modifié pendant l'itération.

{{if interactive

```{test: no}
// Votre code ici (et aussi celui de l'exercice précédent).

for (let valeur of Ensemble.depuis(["a", "b", "c"])) {
  console.log(valeur);
}
// → a
// → b
// → c
```

if}}

{{hint

{{index "groups (exercise)", "Group class", "next method"}}

Il est probablement préférable de définir une nouvelle classe
`IterateurPourEnsemble`. Ses instances devraient avoir une propriété
pour suivre la position courante dans l'ensemble.
À chaque fois que `next` est appelée, elle
vérifie si le parcourt est terminé et, sinon, déplace la position
après celle de la valeur courante tout en renvoyant celle-ci.

La classe `Ensemble` elle-même dispose d'une méthode dont le nom
symbolique est `Symbol.iterator` qui, lorsqu'elle est invoquée, renvoie
une nouvelle instance de l'itérateur pour cet ensemble.

hint}}

### Emprunter une méthode

Plus tôt dans le chapitre, j'ai indiqué que la méthode `hasOwnProperty`
d'un objet pouvait être utilisé comme une alternative à l'opérateur `in`
lorsqu'on souhaite ne pas prendre en compte les propriétés de son prototype.
Mais comment faire si on souhaite utiliser `"hasOwnProperty"` comme nom
de propriété? vous ne serez plus en mesure d'appeler cette méthode à nouveau
puisque la propriété va empêcher l'accès à cette méthode.

Voyez-vous un moyen d'invoquer `hasOwnProperty` sur un objet qui possède
une propriété de même nom?

{{if interactive

```{test: no}
let associations = {un: true, deux: true, hasOwnProperty: true};

// L'appel qui suit échoue. Réparez!
console.log(associations.hasOwnProperty("un"));
// → true
```

if}}

{{hint

Souvenez-vous que les méthodes qui pré-existent sur des objets simples
proviennent de `Object.prototype`.

Souvenez-vous aussi que vous pouvez appeler une fonction avec une
valeur spécifique pour `this` en utilisant la méthode `call`.

hint}}
