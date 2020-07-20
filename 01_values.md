{{meta {docid: values}}}

# Valeurs, Types et Opérateurs

{{quote {author: "Master Yuan-Ma", title: "The Book of Programming", chapter: true}

Below the surface of the machine, the program moves. Without effort,
it expands and contracts. In great harmony, electrons scatter and
regroup. The forms on the monitor are but ripples on the water. The
essence stays invisibly below.

quote}}

{{index "Yuan-Ma", "Book of Programming"}}

{{figure {url: "img/chapter_picture_1.jpg", alt: "Picture of a sea of bits", chapter: framed}}}

{{index "binary data", data, bit, memory}}

Dans le monde des ordinateurs, il n'y a que des données. Les données
peuvent être lues, modifiées ou créées—Mais, dans ce monde, il est impossible de se référer
à autre chose qu'une donnée. Toutes ces données sont stockées
sous la forme de longs motifs de bits, elles sont donc fondamentalent semblables.

{{index CD, signal}}

Un _bit_ est tout ce qui peut prendre seulement deux valeurs, ordinairement représentées
par zéro et un. Au coeur de l'ordinateur, il se concrétise par une charge
électrique élevée ou basse, un signal fort ou faible, ou
un point brillant ou terne à la surface d'un CD. Toutes sortes d'informations
peuvent se représenter comme une suite de zéros et de uns, et donc par des bits. 

Montrons pour l'exemple comment on peut exprimer le nombre 13 avec des bits.
Cela fonctionne comme l'écriture décimale des nombres,
mais au lieu d'utiliser 10 chiffres distincts, on a en a seulement 2, et leur poids augmentent d'un facteur 2 de la droite vers la gauche. Voici la suite de bits qui représente le
nombre 13, avec leurs poids respectifs juste en dessous: 

{{index "binary number", radix, "decimal number"}}

```{lang: null}
 ...    0   0   0   0   1   1   0   1
 ...  128  64  32  16   8   4   2   1
```

Cela donne donc le nombre binaire 00001101. Ses chiffres non nuls expriment
8, 4, et 1, et leur somme donne 13, le nombre que nous souhaitions transcrire.

## Les valeurs

{{index [memory, organization], "volatile data storage", "hard drive"}}

Imaginez une vaste étendue de bits—un océan de 0 et de 1. Le moindre ordinateur
moderne concentre plus de 30 billions (mille milliards) de bits dans
sa mémoire principale (mémoire volatile). Il en a encore une centaine
de fois plus dans sa mémoire de masse (disque dur ou équivalent).
 
Pour être en mesure de travailler sur ces quantités astronomiques de bits
sans se perdre, on doit les regrouper en fragments pour représenter des
morceaux d'informations. Ces fragments sont appelés _((valeur))s_ dans
le contexte de JavaScript. Bien que toutes ces valeurs soient faites de bits,
elles se distinguent par leurs rôles. Chaque valeur a un _((type))_ qui détermine ce
rôle. Certaines représentent des nombres, d'autres des portions de texte, 
d'autres encore des fonctions, etc.

{{index "garbage collection"}}

Pour créer une valeur, il suffit d'utiliser son nom. C'est bien pratique.
Inutile de rassembler je ne sais quel matériel de construction ou de payer pour produire
une valeur. Il suffit d'en invoquer une, et _hop_, vous l'avez. Bien sûr,
elles ne sont pas vraiment créées à partir de rien. Chaque valeur doit être
stockée quelque part et si vous en voulez une quantité considérable dans
un laps de temps très court, vous pourriez manquer de mémoire. Fort heureusement,
ce n'est un problème que si vous en avez besoin instantanément. Dès que
vous n'avez plus besoin d'utiliser une valeur, elle va disparaître, laissant
ses bits derrière elle pour qu'ils puissent être recyclés en une nouvelle
génération de valeurs. 

Dans ce chapitre, nous découvrirons les atomes d'un programme JavaScript,
c'est-à-dire les types de valeurs les plus courantes et les opérations qui
permettent d'agir sur celles-ci.

## Nombres

{{index [syntax, number], number, [number, notation]}}

Les valeurs de type _number_ représentent, comme il faut s'y attendre, des nombres.
Dans un programme JavaScript, on les écrit comme suit:

```
13
```

{{index "binary number"}}

Une telle ligne d'un programme crée et stocke, dans la mémoire de l'ordinateur, le motif binaire associé à 13.

{{index [number, representation], bit}}

JavaScript utilise un nombre fixe de bits, 64 précisément, pour stocker
une valeur numérique. Même si le nombre de motifs réalisables avec 64 bits
est grand, il n'en demeure pas moins qu'il est limité. Avec _N_ chiffres décimaux,
on peut représenter 10^N^ nombres. De même, avec 64 chiffres binaires, on peut
représenter 2^64^ nombres distincts, ce qui représente environ 18 quintillons (un 18
suivi de 18 zéros). C'est beaucoup. 

La mémoire des ordinateurs était bien plus petite autrefois et l'usage
était d'utiliser des groupes de 8 ou 16 bits pour représenter des nombres.
Il était facile de dépasser accidentellement les capacités pour ces nombres et,
au final, d'obtenir des résultats tronqués. De nos jours, même les ordinateurs
qui tiennent dans une poche ont suffisamment de mémoire. Nous sommes donc libres
d'utiliser des fragments de 64 bits sans avoir à nous soucier de ces dépassements (sauf
à manipuler des nombres vraiment astronomiques).

{{index sign, "floating-point number", "sign bit"}}

Les nombres entiers inférieurs à 18 quintillions ne sont pas les seuls à devoir
tenir dans ces 64 bits. Ces bits stockent aussi des nombres négatifs, un bit
sert donc à indiquer le signe du nombre. On doit encore pouvoir représenter
des nombres décimaux (non entiers) et la chose est plus délicate.
Pour y parvenir, on consacre encore quelques bits afin d'indiquer
la position du séparateur décimal. En fait, le plus grand nombre entier représentable
est de l'ordre de 9 quadrillions (15 zéros)—c'est encore très confortable.

{{index [number, notation], "fractional number"}}

Les nombres décimaux s'écrivent avec un point.

```
9.81
```

{{index exponent, "scientific notation", [number, notation]}}

Lorsque ces nombres sont très grands ou, au contraire, très 
proches de zéro, on peut aussi utiliser la notation scientifique.
On ajoute un _e_ (pour l'_exposant_), suivi de l'exposant du nombre.

```
2.998e8
```

Ce qui représente 2,998 × 10^8^ = 299 800 000.

{{index pi, [number, "precision of"], "floating-point number"}}

Les calculs qui font intervenir des nombres entiers (_((integer))s_)
inférieurs à la limite mentionnée plus tôt de 9 quadrillions offrent
la garantie d'être toujours exacts. Malheureusement, les calculs
avec des nombres décimaux ne le sont généralement pas. De même que
π (pi) ne peut être exprimé par une suite finie de chiffres décimaux,
la plupart des nombres décimaux ne sont qu'approximativement représentés
par une suite de 64 bits. C'est embarrassant, mais en pratique cela n'est
gênant que dans des situations très particulières. L'important est
d'être conscient de ces limites et de traiter les nombres décimaux comme
des approximations, non des valeurs exactes. 

### Arithmétique

{{index [syntax, operator], operator, "binary operator", arithmetic, addition, multiplication}}

Ce qu'on peut principalement faire avec des nombres, c'est de l'arithmétique.
Les opérations arithmétiques comme l'addtion ou la multiplication prennent 
deux valeurs numériques pour en produire une troisième. Voici à quoi elles ressemblent
en JavaScript:

```
100 + 4 * 11
```

{{index [operator, application], asterisk, "plus character", "* operator", "+ operator"}}

Les symboles `+` et `*` sont ce qu'on appelle des _opérateurs_. Le premier
pour l'addition et le second pour la multiplication. Placer un opérateur entre
deux valeurs applique celui-ci à ces valeurs et produit une nouvelle valeur.

{{index grouping, parentheses, precedence}}

Mais faut-il interpréter cet exemple comme «ajouter 4 et 100, puis
multiplier le résultat par 11» ou est-ce la multiplication qu'il
faut effectuer avant l'addition? Comme vous pouvez le deviner,
c'est la multiplication qu'il faut effectuer en premier.
Néanmoins, comme en mathématiques, on peut modifier cet ordre en
plaçant l'addition et ses opérandes entre parenthèses.

```
(100 + 4) * 11
```

{{index "hyphen character", "slash character", division, subtraction, minus, "- operator", "/ operator"}}

Pour la soustraction, on a l'opérateur `-` et pour la division l'opérateur `/`.

Lorsque plusieurs opérateurs sont utilisés sans parenthèses, l'ordre
dans lequel ils sont appliqués est déterminé par la _((précédence))_ de ces opérateurs.
L'exemple montre que la multiplication vient avant l'addition. L'opérateur `/`
a la même précédence que `*`. De même pour `+` et `-`. Lorsque plusieurs
opérateurs de même précédence se suivent, comme dans `1 - 2 + 1`, ils sont appliqués
de la gauche vers la droite: `(1 - 2) + 1`.

Ces règles de précédence ne doivent pas vous inquiéter. En cas de doute,
ajoutez simplement des parenthèses.

{{index "modulo operator", division, "remainder operator", "% operator"}}

Il reste encore un opérateur arithmétique que vous pourriez ne pas
reconnaître immédiatement.
Le symbole `%` représente le _reste_ de la division entière. `X % Y` est le
reste de la division entière de `X` par `Y`. Par exemple, `314 % 100` produira
`14`, et `144 % 12` donne `0`. La précédence de `%` est la même que celle
de la multiplication et de la division. Cet opérateur est fréquemment appelé _modulo_. 

### Nombres spéciaux

{{index [number, "special values"]}}

On dispose, en JavaScript, de trois valeurs spéciales considérées comme des nombres
mais qui ne se comportent pas comme des nombres habituels.

{{index infinity}}

Les deux premières sont `Infinity` et `-Infinity` qui représentent respectivement
les infinis positif et négatif. `Infinity - 1` n'est pas différent de `Infinity`   
et ainsi de suite. Ne vous fiez donc pas trop aux calculs avec ces infinis. 
Ils se contentent d'approcher les notions mathématiques correspondantes et vous
conduiront rapidement à la prochaîne valeur numérique spéciale: `NaN`.

{{index NaN, "not a number", "division by zero"}}

`NaN` abrège «not a number» (pas un nombre), bien que ce _soit_ une
valeur de type nombre. On obtient ce résultat lorsque, par exemple,
on tente de calculer `0 / 0` (zéro divisé par zéro), `Infinity - Infinity`
ou tout calcul qui ne produit pas une valeur numérique qui fait sens.

## Chaînes de caractères \[_Strings_\]

{{indexsee "grave accent", backtick}}

{{index [syntax, string], text, character, [string, notation], "single-quote character", "double-quote character", "quotation mark", backtick}}

Le prochain type de données élémentaires est _((string))_ (chaîne de caractères).
Il est utilisé pour représenter du texte. On les écrit en délimitant leur contenu
avec des guillemets droits. 

```
`Bas sur la mer`
"Étendu sur l'océan"
'Flotter dans l\'océan'
```

On peut utiliser des guillemets simples (`'`), doubles (`"`) ou «arrière» (`` ` `` -  _backticks_) pour
délimiter les chaînes, pourvu que le premier corresponde au dernier.

{{index "line break", "newline character"}}

On peut mettre pratiquement tout ce qu’on veut entre des guillemets
et JavaScript fera la conversion en une valeur de type `string`.
Mais pour certains caractères c’est un peu tordu.
Vous pouvez imaginer à quel point il est délicat de mettre
des guillemets entre guillemets.
Les sauts de lignes (comme vous en faites en appuyant sur [Entrée]{keyname})
ne peuvent être mis, sans échappement, dans une chaîne que si cette chaîne est délimitée
par des guillemets «arrière» (`` ` ``).

{{index [escaping, "in strings"], ["backslash character", "in strings"]}}

Pour mettre de tels caractères dans une chaîne, on emploie l’astuce suivante:
à chaque fois qu’on trouve un antislash (« \ ») dans un texte entre guillemets,
cela signifie que le caractère qui le suit a une signification particulière. On
appelle cela un caractère d'_échappement_.
Un guillemet qui est précédé d’un antislash n’achèvera pas la chaîne mais en fera partie.
Quand le caractère `n` se trouve derrière l’antislash, il est interprété
comme un saut de ligne. De même, un `t` derrière un antislash signifie un caractère de tabulation.
Observez la chaîne:

```
"Voici une première ligne\nEt maintenant la seconde"
```

Le texte correspondant est en fait:

```{lang: null}
Voici une première ligne
Et maintenant la seconde
```

Il existe bien entendu des cas où vous voudriez que l’antislash
dans une chaîne soit juste un antislash et pas un caractère d’échappement.
Si deux antislashes se succèdent, ils vont se combiner et seul
l’un d’eux sera conservé dans la chaîne résultante.
Voici comment la chaîne «_Un caractère de saut de ligne est écrit ainsi `"`\n`"`._»
peut être exprimée:

```
"Un caractère de saut de ligne est écrit ainsi \"\\n\"."
```

{{id unicode}}

{{index [string, representation], Unicode, character}}

Les chaînes, elles aussi, doivent être traduites par une série de bits
pour pouvoir exister à l'intérieur de l'ordinateur. Pour y parvenir,
JavaScript s'appuie sur le standard _((Unicode))_. Ce standard assigne
un nombre à chaque caractère dont vous pourriez avoir besoin et cela inclut
le grec, l'arabe, le japonais, l'arménien, etc. Puisque nous disposons
ainsi d'un nombre pour chaque caractère, une chaîne peut se décrire comme
une suite de tels nombres.

{{index "UTF-16", emoji}}

Et c'est ce que fait JavaScript. Cependant il y a une complication:
JavaScript utilise 16 bits pour représenter chaque caractère (ou plutôt le nombre
qui lui correspond) et cela lui permet d'en coder 2^16^ au plus.
Mais Unicode en définit davantage—environ deux fois plus pour l'instant.
Ainsi, certains caractères, comme la plupart des emojis, nécessitent «deux positions
de caractères» dans la chaîne JavaScript. Nous reviendrons sur ce point dans le
[chapitre ?](higher_order#code_units).

{{index "+ operator", concatenation}}

Les chaînes ne peuvent être divisées, multipliées ou soustraites,
mais l’opérateur `+` _peut_ tout de même être utilisé avec des chaînes.
Il n’ajoute rien au sens mathématique mais _concatène_ les chaînes—il
les colle ensemble. La ligne suivante produit la chaîne `"concaténer"`:

```
"con" + "cat" + "é" + "ner"
```

Les valeurs de type chaîne peuvent être manipulées par de nombreuses
fonctions (_méthodes_) dédiées. Nous en dirons davantage à leur propos dans
le [chapitre ?](data#methods).

{{index interpolation, backtick}}

Les chaînes écrites avec des guillemets simples ou doubles ont un comportement
similaire—la seule différence réside dans la sorte de guillemet qu'il
faut échapper lorsqu'on veut qu'il fasse partie de la chaîne.
Les chaînes délimitées par des guillemets
«arrière», souvent appelées _((littéraux de gabarits))_, ont quelques particularités
supplémentaires. Hormis la possibilité d'y mettre plusieurs lignes, elles peuvent aussi
embarquer d'autres valeurs: 

```
`La moitié de 100 est ${100 / 2}`
```

Lorsqu'on écrit quelque chose à l'intérieur de `${}` dans un gabarit,
son résultat est calculé, converti en chaîne, laquelle est incluse à cette position.
L'exemple produira "_La moitié de 100 est 50_".

## Opérateurs unaires

{{index operator, "typeof operator", type}}

Les opérateurs ne sont pas tous des symboles. Certains sont des mots.
L'opérateur `typeof` en est un exemple. Il produit une valeur de type
chaîne pour donner le nom du type de la valeur qui le suit. 

```
console.log(typeof 4.5)
// → number
console.log(typeof "x")
// → string
```

{{index "console.log", output, "JavaScript console"}}

{{id "console.log"}}

Nous utiliserons `console.log` dans les exemples de code afin d'indiquer
que nous souhaitons voir le résultat de l'_évaluation_ de quelque chose.
Nous en reparlerons dans le [prochain chapitre](program_structure).

{{index negation, "- operator", "binary operator", "unary operator"}}

Les autres opérateurs déjà mentionnés opèrent tous sur deux valeurs, alors
que `typeof` n'en attend qu'une. Les opérateurs qui utilisent deux valeurs
sont appelés opérateurs _binaires_, tandis que ceux qui n'en utilisent qu'une
sont appelés opérateurs _unaires_. L'opérateur `-` peut être utilisé comme
un opérateur binaire ou comme un opérateur unaire.

```
console.log(- (10 - 2))
// → -8
```
## Valeurs booléennes

{{index Boolean, operator, true, false, bit}}

Il est bien souvent utile de disposer d'une valeur qui distingue exactement
deux possibilités, comme «oui» et «non» ou «allumé» et «éteint». Pour cela,
JavaScript dispose du type _Boolean_ qui n'a que de deux valeurs: `true`
et `false`. 

### Comparaison

{{index comparison}}

Voici une façon de produire une valeur booléenne:

```
console.log(3 > 2)
// → true
console.log(3 < 2)
// → false
```

{{index [comparison, "of numbers"], "> operator", "< operator", "greater than", "less than"}}

Les signes `>` et `<` sont les symboles traditionnels pour exprimer
«est (strictement) supérieur à» et «est (strictement) inférieur à». 
Ce sont des opérateurs binaires. Leur utilisation produit une valeur booléenne
qui indique si l'inégalité est satisfaite (auquel cas elle produit `true`) ou non (`false`).

On peut comparer des chaînes de la même façon:

```
console.log("Godzilla" < "Goldorak")
// → true
```

{{index [comparison, "of strings"]}}

Le classement des chaînes suit plus ou moins l’ordre alphabétique.
Plus ou moins parce que… les lettres majuscules sont toujours
«plus petites que» les minuscules, donc `"Z" < "a"`
(«Z» en majuscule, «a» en minuscule) vaut `true` et
les caractères non alphabétiques («!», «-», etc.) sont
également inclus dans ce classement.
Pour comparer les chaînes, JavaScript se contente de comparer,
un à un et de gauche à droite, les codes ((Unicode)) associés
aux caractères dans la chaîne.

{{index equality, ">= operator", "<= operator", "== operator", "!= operator"}}

Voici d’autres opérateurs du même genre:
`>=` (supérieur ou égal à), `<=` (inférieur ou égal à),
`==` (égal à), et `!=` (n’est pas égal à).

```
console.log("Itchy" != "Scratchy")
// → true
console.log("Pomme" == "Orange")
// → false
```

{{index [comparison, "of NaN"], NaN}}

Seule une valeur n'est pas égale à elle-même en JavaSript: `NaN` ("not a number")

```
console.log(NaN == NaN)
// → false
```

En effet, `NaN` représente le résultat d'un calcul qui n'a pas de sens,
dès lors, il ne doit pas être égal au résultat d'un _autre_ calcul insensé.

### Opérateurs logiques

{{index reasoning, "logical operators"}}

On trouve aussi des opérations qui s'appliquent à des valeurs booléennes.
JavaScript propose trois opérateurs logiques: _et_, _ou_ et _non_.
On peut les utiliser pour «raisonner» sur les booléens.

{{index "&& operator", "logical and"}}

L'opérateur `&&` représente le _et_ logique. C'est un opérateur binaire,
et son résultat n'est `true` que si les deux valeurs qu'on lui soumet valent `true`.

```
console.log(true && false)
// → false
console.log(true && true)
// → true
```

{{index "|| operator", "logical or"}}

L'opérateur `||` correspond au _ou_ logique. Il ne produit `true` que lorsqu'au moins l'une des valeurs fournies vaut elle-même `true`.

```
console.log(false || true)
// → true
console.log(false || false)
// → false
```

{{index negation, "! operator"}}

_Non_ s'écrit en utilisant un point d'exclamation (`!`). C'est un opérateur
unaire qui inverse la valeur qu'on lui fournit—`!true` produit `false` et
`!false` produit `true`.

{{index precedence}}

Lorsqu'on mélange ces opérateurs booléens à des opérateurs arithmétiques
et autres, il n'est pas toujours évident de savoir si des parenthèses
sont requises. En pratique, on peut s'en sortir avec la précédence de
ceux qu'on a vus jusqu'ici, `||` a la plus faible, puis vient `&&`, ensuite
les opérateurs de comparaison (`>`, `==`, etc.), enfin les autres.
Cet ordre a été choisi de façon que, dans une expression typique comme
celle qui suit, on ait besoin du moins possible de parenthèses.

```
1 + 1 == 2 && 10 * 10 > 50
```

{{index "conditional execution", "ternary operator", "?: operator", "conditional operator", "colon character", "question mark"}}

Le dernier opérateur logique que nous verrons n'est ni unaire,
ni binaire, mais _ternaire_: il opère sur trois valeurs.
Il s'écrit en combinant un point d'interrogation (`?`) et
deux points (`:`), comme ceci:

```
console.log(true ? 1 : 2);
// → 1
console.log(false ? 1 : 2);
// → 2
```

On le nomme opérateur _conditionnel_ (ou parfois l'opérateur _ternaire_ car c'est le
seul que propose le langage). La valeur située à gauche du point d'interrogation
sélectionne celle des deux autres valeurs qui formera le résultat. Lorsqu'elle vaut
`true`, la valeur du milieu est choisie et, lorsqu'elle vaut `false`, c'est la dernière.

## Valeurs vides

{{index undefined, null}}

On dispose encore de deux valeurs particulières qui s'écrivent `null` et `undefined`.
Elles sont utilisées pour indiquer l'absence d'une valeur _utile_ ou faisant sens.
Bien qu'elles soient elles-mêmes des valeurs, elles ne portent pas d'information.  

Beaucoup d'opérations du langage ne produisent pas de valeur significative
(nous en verrons plus tard) mais elles produisent tout de même `undefined`
pour la simple et bonne raison qu'elles _doivent_ produire _une_ valeur.

La différence de sens entre `undefined` et `null` est due à un accident
dans la conception de JavaScript, elle n'a pas d'incidence pratique la plupart du temps.
Je vous recommande de les considérer comme interchangeables.

## Conversion automatique de type

{{index NaN, "type coercion"}}

Dans l'introduction, je vous disais que JavaScript passait les bornes
en acceptant à peu près n'importe quel programme,
même des programmes qui font des choses bizarres. Les expressions qui
suivent vous en donnent une illustration:

```
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("cinq" * 2)
// → NaN
console.log(false == 0)
// → true
```

{{index "+ operator", arithmetic, "* operator", "- operator"}}

Lorsqu'un opérateur est utilisé avec des valeurs du «mauvais» type,
JavaScript va les convertir sans avertissement dans le type dont il
a besoin, en utilisant pour cela un ensemble de règles qui produisent
quelque chose que vous ne vouliez pas ou n'attendiez pas. Cela s'appelle
une _((coercition de type))_. La valeur `null` de la première expression est devenue `0`,
et le `"5"` de la seconde a été transformé en `5` (chaîne vers nombre).
Pourtant, dans la troisième expression, `+` essai de se comporter comme
une concaténation avant d'essayer d'être une addition et donc le `1` est
converti en `"1"` (nombre vers chaîne).

{{index "type coercion", [number, "conversion to"]}}

Quand quelque chose ne correspond pas d'une façon évidente
à un nombre (comme `"cinq"` ou `undefined`) et que cette chose est tout de même
convertie en un nombre, vous obtenez la valeur `NaN`.
Mais si NaN figure dans une opération arithmétique celle-ci produit à nouveau NaN.
Ainsi, si vous ne vous attendiez pas à trouver NaN quelque part, il y a
fort à parier qu'une conversion de type accidentelle s'est produite en amont.

{{index null, undefined, [comparison, "of undefined values"], "== operator"}}

Lorsque vous comparez deux valeurs du même type avec `==`, le résultat est facile
à prévoir: vous obtenez `true` lorsque les deux valeurs sont les mêmes sauf dans le
cas de `NaN`. Mais si jamais les deux valeurs ont des types qui diffèrent,
Javascript utilise un jeu compliqué de règles pour déterminer quoi faire.
Dans la plupart des cas, il essaie juste de convertir une valeur dans le type
de l'autre. Mais si `null` ou `undefined` apparaît dans un membre, la comparaison
ne produira `true` que si `null` ou `undefined` apparaît dans l'autre. 

```
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

C'est un comportement utile. Lorsque vous avez besoin de vérifier
si une valeur n'est pas «vide», vous pouvez la comparer à `null`
avec l'opérateur `==` (ou `!=`).

{{index "type coercion", [Boolean, "conversion to"], "=== operator", "!== operator", comparison}}

Mais comment faire pour tester si quelque chose se réfère précisément à
la valeur `false`? Des expressions comme `0 == false` et `"" == false` donneront
`true` à cause des conversions automatiques de type. 
Si vous voulez empêcher que des conversions de type se produisent, JavaScript
fournit deux opérateurs supplémentaires: `===` et `!==`. Le premier teste
si une valeur est _précisément_ égale à une autre et le second si l'une est
_précisément_ différente de l'autre.

Je vous recommande d'utiliser l'opérateur à trois caractères par prudence et
pour empêcher que des conversions de type inattendues ne viennent vous titiller le cuir chevelu.
Néanmoins, si vous êtes certains que les valeurs comparées sont du même type,
il n'y a aucun problème à utiliser la forme courte de l'opérateur.

### Évaluation «court-circuit» des opérateurs logiques

{{index "type coercion", [Boolean, "conversion to"], operator}}

Les opérateurs logiques `&&` et `||` gèrent les valeurs de différents types
de façon un peu spéciale. Ils vont convertir la valeur située à leur gauche
dans le type booléen de façon à décider quoi faire, et cela va dépendre à la fois
de l'opérateur et de la valeur logique obtenue après conversion. Au final, on
obtient la valeur _originale_ (avant conversion) de gauche
ou celle de droite.

{{index "|| operator"}}

L'opérateur `||`, par exemple, va renvoyer sa valeur gauche si sa
conversion donne `true` et sa valeur droite sinon.
C'est l'effet attendu lorsque les valeurs sont des booléens et cela produit
quelque chose d'analogue si les valeurs ont d'autres types.

```
console.log(null || "utilisateur")
// → utilisateur
console.log("Agnès" || "utilisateur")
// → Agnès
```

{{index "default value"}}

Ce mécanisme peut être exploité afin de fournir une valeur par défaut.
Si vous avez une valeur potentiellement vide suivie par `||` et par une
valeur de remplacement, et que la valeur de gauche est convertie en `false`,
vous obtenez la valeur de remplacement. Les règles de conversion
des chaînes et des nombres en valeurs booléennes stipulent que `0`, `NaN` et la
chaîne vide (`""`) produiront `false`, toutes les autres `true`. Et donc,
`0 || -1` donne `-1` et `"" || "!?"` donne `"!?"`.

{{index "&& operator"}}

L'opérateur `&&` a un fonctionnement analogue mais dans l'autre sens.
Lorsque sa valeur gauche est convertie en `false`, c'est celle qu'il produit
au final et, sinon, il produit celle de droite.

Une autre importante propriété de ces deux opérateurs est de n'évaluer leur
partie droite que si c'est absolument nécessaire. Par exemple, dans l'expression
`true || X`, la valeur de `X` n'a aucune importance—même si c'est un bout de programme
qui fait des choses _horribles_—le résultat sera `true` et `X` n'est jamais
évaluée. La même chose vaut pour `false && X`, qui donnera `false` et ignorera
`X`. Ce comportement s'appelle _((évaluation court-circuit))_. 

{{index "ternary operator", "?: operator", "conditional operator"}}

L'opérateur conditionnel fonctionne aussi selon ce principe. Seule la valeur
sélectionnée parmi les deux possibles est effectivement évaluée.

## Résumé

Nous avons examiné quatre types de valeurs de JavaScript dans ce chapitre: les
nombres (_numbers_), les chaînes (_strings_), les booléens (_Booleans_) et les valeurs vides.

On crée ces valeurs en saisissant leur nom (`true`, `null`) ou directement
 (`13`, `"abc"`). On peut combiner et transformer des valeurs en
utilisant des opérateurs. Nous avons vu des opérateurs binaires pour l'arithmétique
(`+`, `-`, `*`, `/`, and `%`), la concaténation de chaînes (`+`),
les comparaisons (`==`, `!=`, `===`, `!==`, `<`, `>`, `<=`, `>=`),
et la logique (`&&`, `||`), mais aussi plusieurs opérateurs unaires (`-` la négation
numérique, `!` la négation logique et `typeof` pour connaître le type d'une valeur)
et enfin l'opérateur ternaire (`?:`) pour sélectionner
une valeur parmi deux sur la base d'une troisième.

À partir de là, vous disposez d'assez d'informations pour utiliser JavaScript
comme une calculatrice de poche mais guère plus. Le [prochain chapitre](programm_structure)
vous apprendra à les combiner de façon à produire des programmes basiques.
