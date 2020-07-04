{{meta {load_files: ["code/intro.js"]}}}

# Introduction

{{quote {author: "Ellen Ullman", title: "Close to the Machine: Technophilia and its Discontents", chapter: true}

We think we are creating the system for our own purposes. We believe
we are making it in our own image... But the computer is not really
like us. It is a projection of a very slim part of ourselves: that
portion devoted to logic, order, rule, and clarity.

quote}}

{{figure {url: "img/chapter_picture_00.jpg", alt: "Picture of a screwdriver and a circuit board", chapter: "framed"}}}

Ceci est un livre sur l'art d'instruire les ordinateurs. Les ordinateurs sont à peu près
aussi courants que les tournevis de nos jours, mais ils sont un peu plus complexes et
il n'est pas toujours évident de leur faire faire ce qu'on veut.

Si la tâche que vous souhaitez faire accomplir à votre ordinateur est
courante et bien comprise, comme afficher votre courrier électronique ou une calculatrice,
vous pouvez ouvrir l'((application)) appropriée et vous mettre au travail.
Mais pour les tâches uniques ou ouvertes, il n’y a probablement pas d’application toute faite.

C’est là que la _programmation_ entre en jeu. La programmation consiste à
construire un _programme_—un ensemble d’instructions précises qui indiquent à un
ordinateur quoi faire. Mais comme les ordinateurs sont basiquement «idiots», leur programmation
est fondamentalement fastidieuse et frustrante.

{{index [programming, "joy of"], speed}}

Heureusement, si vous pouvez surmonter cela, et peut-être même apprécier la rigueur
d'une réflexion sur ce que peuvent faire ces machines sans cervelles, la programmation peut être
enrichissante. Cela vous permet de faire des choses en quelques secondes qui
prendraient une _éternité_ à la main. C'est un moyen de faire en sorte que votre machine
fasse des choses qu'elle ne pouvait pas faire auparavant. Et cela fournit un
excellent exercice d'abstraction de la pensée.

La plupart des programmes sont réalisés avec des ((langages de programmation)). Un
_langage de programmation_ est un langage construit artificiellement dans le but d'
instruire les machines. Il est intéressant de noter que le moyen le plus efficace que
nous ayons trouvé pour communiquer avec un ordinateur emprunte énormément à la
façon dont nous communiquons les uns avec les autres. A l'instar des langages
humains, les langages informatiques permettent de combiner des mots et des phrases
de manière nouvelle, permettant ainsi d'exprimer des concepts toujours nouveaux.

{{index [JavaScript, "availability of"], "casual computing"}}

À un moment donné, les interfaces basées sur le langage, telles que les invites BASIC
et DOS des années 1980 et 1990, constituaient la principale méthode d’interaction
avec les ordinateurs. Elles ont été largement remplacées par des interfaces graphiques,
plus faciles à apprendre mais offrant moins de liberté. Les langages informatiques
sont toujours là, si vous savez où chercher. Un de ces langages, JavaScript, est intégré
à tous les ((navigateur))s Web modernes et est donc disponible sur presque tous les
appareils.

{{indexsee "web browser", browser}}

Ce livre essaiera de vous familiariser suffisamment avec ce langage pour pouvoir
faire des choses utiles et amusantes.

## Sur la programmation

{{index [programming, "difficulty of"]}}

En plus d'expliquer JavaScript, je vais présenter les principes de base de la
programmation. Il s'avère que programmer est difficile. Les règles de base
sont simples et claires, mais les programmes construits sur ces règles ont tendance à
devenir suffisamment complexes pour introduire leurs propres règles et de nouvelles complexités.
D'une certaine manière, vous construisez votre propre labyrinthe et vous risquez de
vous y perdre.

{{index learning}}

Il y aura des moments où la lecture de ce livre risque de vous frustrez. Si vous débutez
en programmation, il y aura beaucoup de nouvelles notions à assimiler.
Une grande partie de ces notions seront ensuite réinvesties de façon à vous permettre d'apprendre
à les _combiner_ pour atteindre tel ou tel but.

C'est à vous de faire l'effort nécessaire. Lorsque vous aurez du mal
à suivre le livre, ne vous blâmez pas, ne vous découragez pas. Tout va bien,
vous avez juste besoin de reprendre un peu les choses.
Faites une pause puis relisez tranquillement la partie qui vous a donnée du mal
en vous assurant de bien comprendre les exemples de programmes et les exercices
correspondants. Apprendre est une chose difficile, mais tout ce qu'on apprend
nous appartient et facilite nos apprentissages ultérieurs.

{{quote {author: "Ursula K. Le Guin", title: "The Left Hand of Darkness"}

{{index "Le Guin, Ursula K."}}

When action grows unprofitable, gather information; when information
grows unprofitable, sleep.

quote}}

{{index [program, "nature of"], data}}

Un programme, c'est beaucoup de choses. C'est un bout de texte saisi par un
programmeur, c'est la force directrice qui indique à l'ordinateur ce qu'il doit faire, ce
sont des données dans sa mémoire ainsi que les actions qu'il réalise sur celles-ci.
Les analogies qui tendent à comparer les programmes à des objets qui nous sont familiers
ne tiennent pas longtemps. Un rapprochement rapide peut toutefois être fait avec une machine
imaginaire—formée de nombreux rouages entremélés dont les relations mutuelles
doivent être maîtrisées pour réussir à animer le tout harmonieusement.

Un ((ordinateur)) est une machine physique conçu pour héberger ce genre de machines imaginaires.
Par lui-même, un ordinateur ne peut faire que des choses simplissimes. La raison
pour laquelle ils sont si utiles est qu’ils les font à une ((vitesse)) incroyablement
élevée. Un programme peut ingénieusement articuler un nombre énorme de ces
actions simples pour faire des choses très complexes.

{{index [programming, "joy of"]}}

Un programme est une construction de l'esprit. Son élaboration ne coûte rien, 
ne pèse rien et le programme pousse facilement sous nos mains qui pianotent.

Mais si on n'y prend garde, sa taille et sa ((complexité)) peuvent devenir ingérables,
et même son auteur risque de s'y perdre. Garder les programmes sous contrôle, c'est le
principal problème de la programmation. Quand un programme fonctionne, c'est
beau. L'art de la programmation est l'habileté à contrôler la complexité. Un
programme de grande qualité reste en retrait, sa complexité s'efface sous une apparente simplicité.

{{index "programming style", "best practices"}}

Certains programmeurs croient qu'il suffit de se limiter à quelques techniques
bien comprises pour maîtriser cette complexité. Ils élaborent des règles strictes
(«meilleures pratiques») sur la forme qu'un programme devrait avoir et se retranche
dans leur petite zone de sécurité.

{{index experiment}}

En plus d'être à mourir d'ennui, cette approche est inefficace. À problèmes nouveaux,
solutions nouvelles. L'art de programmer, même s'il se développe rapidement,
est encore relativement jeune et la grande variété de ses sujets laisse beaucoup de place
à des approches ouvertes et audacieuses. Certes, de nombreuses et affreuses erreurs
de conception attendent le programmeur inexpérimenté, mais il vaut mieux aller de 
l'avant, tomber dans ces pièges et en tirer une leçon. Le jugement sur les qualités
d'un bon programme s'acquiert par la pratique et non en suivant aveuglément un corpus
de règles toutes faites. 

## L'importance du langage

{{index "programming language", "machine code", "binary data"}}

Au début de l’informatique, à sa naissance, il n’existait pas de langage de programmation.
Les programmes ressemblaient à ceci :

```{lang: null}
00110001 00000000 00000000
00110001 00000001 00000001
00110011 00000001 00000010
01010001 00001011 00000010
00100010 00000010 00001000
01000011 00000001 00000000
01000001 00000001 00000001
00010000 00000010 00000000
01100010 00000000 00000000
```

{{index [programming, "history of"], "punch card", complexity}}

Il s’agit d’un programme qui fait la somme des nombres de 1 à 10 et
donne le résultat: `1 + 2 + … + 10 = 55`.
Il pourrait tourner sur l’ordinateur le plus rudimentaire. Pour programmer
les premiers ordinateurs, il était nécessaire de disposer de grandes
séries d’interrupteurs dans la bonne position, ou de perforer des trous
dans des cartes que l’on faisait avaler à la machine.
Vous imaginez facilement à quel point la procédure était fastidieuse et sujette à erreurs.
Écrire ne serait-ce qu’un programme simple exigeait beaucoup d’intelligence
et de discipline, les programmes complexes étaient carrément hors d’atteinte.

{{index bit, "wizard (mighty)"}}

Bien entendu, saisir à la main des motifs de bits (les suites de 1 et de 0 ci-dessus)
donnait au programmeur l’impression d’être un puissant sorcier.
Cela ne devait pas compter pour du beurre en terme de satisfaction professionnelle.

{{index memory, instruction}}

Chaque ligne de programme contient une instruction unique.
On pourrait l’exprimer en français sous cette forme :

 1. Stocker le nombre 0 à l’adresse mémoire 0
 2. Stocker le nombre 1 à l’adresse mémoire 1
 3. Stocker la valeur de l’adresse mémoire 1 dans l’adresse mémoire 2
 4. Soustraire 11 de la valeur stockée à l’adresse mémoire 2
 5. Si la valeur à l’adresse mémoire 2 est le nombre 0, continuer à l’instruction 9
 6. Ajouter la valeur de l’adresse mémoire 1 à la valeur de l’adresse mémoire 0
 7. Ajouter le nombre 1 à la valeur de l’adresse mémoire 1
 8. Continuer avec l’instruction 3
 9. Donner la valeur de l’adresse mémoire 0

{{index readability, naming, binding}}

Bien que ce soit déjà plus lisible que la soupe binaire, cela reste assez désagréable.
Il pourrait être utile d’utiliser des noms au lieu de nombres
pour les instructions et les adresses mémoire:

```{lang: "text/plain"}
 Mettre 0 à 'total'
 Mettre 1 à 'compteur'
[boucle]
 Mettre 'compteur' à 'comparaison'
 Soustraire 11 à 'comparaison'
 Si 'comparaison' est zéro, continuer à [fin]
 Ajouter 'compteur' à 'total'
 Ajouter 1 à 'compteur'
 Continuer à [boucle]
[fin]
 Afficher 'total'
```

{{index loop, jump, "summing example"}}

À partir de là il n’est pas trop difficile de deviner
comment fonctionne le programme. Qu’en dites-vous ?
Les deux premières lignes donnent leur valeur initiale aux adresses mémoire:
`total` sera utilisé pour calculer le résultat du programme
et `compteur` conserve le nombre courant. Les lignes qui utilisent
`comparaison` sont probablement les plus bizarres. Ce que le programme
cherche à savoir c’est si `compteur` est égal à 11, de manière à savoir
s’il doit interrompre le calcul. Comme la machine est très rudimentaire,
il peut seulement tester si un nombre est zéro, et prendre une décision (sauter)
basée sur ce critère. Il utilise donc l’adresse mémoire nommée `comparaison`
pour calculer la valeur de `compteur` - 11, et décide suivant la valeur obtenue.
Les deux lignes suivantes ajoutent la valeur de `compteur` au résultat,
et incrémentent `compteur` d’une unité à chaque fois que le programme a décidé
qu’il n’avait pas encore atteint 11.

Voici maintenant le même programme en JavaScript:

```
let total = 0, compteur = 1;
while (compteur <= 10) {
  total += compteur;
  compteur += 1;
}
console.log(total);
// → 55
```

{{index "while loop", loop, [braces, block]}}

C’est encore un peu mieux pour nous. Le plus important c’est qu’il n’est
plus nécessaire de préciser la façon dont nous voulons que le programme
fasse un bond par-ci par-là. Le mot magique `while` (_tant que_) s’en occupe.
Le programme continue à exécuter les lignes ci-dessous tant que la condition
qu’on lui a donnée est satisfaite: `compteur <= 10`, ce qui veut dire
tant que «_compteur_ est inférieur ou égal à 10».
Apparemment, il n’y a plus besoin de créer une valeur temporaire et
de la comparer à zéro. C’était un petit détail stupide, et le pouvoir
des langages de programmation est justement qu’ils règlent
pour nous ce genre de choses accessoires.

{{index "console.log"}}

À la fin du programme, une fois que la «construction» `while` se termine, l'opération
`console.log` est utilisée pour écrire le résultat.

{{index "sum function", "range function", abstraction, function}}

Finalement, voici à quoi pourrait ressembler le programme si nous nous
étions arrangés pour que les opérations `serie` et `somme` soient disponibles,
la première produit une série de nombres, la seconde calcule la somme d’une série de nombres:

```{startCode: true}
console.log(somme(serie(1, 10)));
// → 55
```

{{index readability}}

La morale de cette histoire est donc que le même programme peut être
exprimé de façon brève ou longue, lisible ou illisible.
La première version du programme était extrêmement obscure,
tandis que la dernière est quasiment du français : `console.log` (afficher) la somme de la série des nombres de 1 à 10.
(Nous verrons [par la suite](data) dans d’autres chapitres comment construire des opérations telles que `somme` et `serie`.)

{{index ["programming language", "power of"], composability}}

Un bon langage de programmation aide le programmeur en lui fournissant
une manière plus abstraite de s’exprimer.
Il masque les détails inintéressants, procure des composants de base pratiques
(comme `while` et `console.log`) et, la plupart du temps, permet au programmeur
d’ajouter lui-même de telles briques (comme les opérations `somme` et `serie`)
tout en facilitant leur composition.

## Qu'est-ce que JavaScript ?

{{index history, Netscape, browser, "web application", JavaScript, [JavaScript, "history of"], "World Wide Web"}}

{{indexsee WWW, "World Wide Web"}}

{{indexsee Web, "World Wide Web"}}

JavaScript a été introduit en 1995 afin d’ajouter des programmes aux pages Web dans
le navigateur Netscape. Le langage a depuis été adopté par tous les autres
principaux navigateurs Web graphiques. Il a rendu possible les applications Web
modernes—des applications avec lesquelles vous pouvez interagir directement sans
recharger la page pour chaque action. JavaScript est également utilisé dans des sites
Web plus traditionnels pour fournir diverses formes d'interactivité et d'intelligence.

{{index Java, naming}}

Contrairement à ce que son nom suggère, JavaScript a très peu à voir avec
le langage de programmation Java. Le choix d'un nom similaire a été inspiré
par des considérations commerciales plutôt que rationnelles.
Lors de l'introduction de JavaScript, le langage Java était promu partout et gagnait en popularité. Apparemment, quelqu’un a dû penser que c’était une bonne idée d’essayer de surfer sur la mode du moment. Nous voilà aujourd’hui coincés avec ce nom.

{{index ECMAScript, compatibility}}

Après son adoption en dehors de Netscape, un effort de standardisation a été entrepris pour
décrire la manière dont le langage JavaScript devrait fonctionner et cela de manière à harmoniser les
différents logiciels qui prétendaient prendre en charge ce langage. On l'a appelé
ECMAScript, du nom de l'organisation qui l'a standardisé. En pratique, les termes ECMAScript et JavaScript sont interchangeables: ce sont deux noms pour le même langage.

{{index [JavaScript, "weaknesses of"], debugging}}

Certains vous diront des choses terribles à propos de JavaScript.
Beaucoup de ces reproches sont fondés. La première fois que j’ai dû écrire
quelque chose en JavaScript, j’ai rapidement méprisé ce langage.
Il acceptait à peu près tout ce que je tapais, mais l’interprétait
d’une façon complètement différente de celle que je voulais.
Cela venait surtout du fait que je n’avais aucune idée de ce que je faisais,
mais il y a aussi un véritable problème ici: JavaScript est absurdement laxiste
dans ce qu’il permet. L’idée derrière cette conception était de rendre
la programmation en JavaScript plus facile pour les débutants.
En réalité, il rend surtout plus difficile la recherche des problèmes
dans vos programmes, parce que le système ne vous les montrera pas.

{{index [JavaScript, "flexibility of"], flexibility}}

Toutefois, la souplesse de ce langage est aussi un avantage. Elle laisse
la place à de nombreuses techniques que les langages de programmation
plus rigides ne permettent pas. Et, comme vous le verrez (par exemple
dans le [chapitre ?](modules), on peut l’utiliser pour compenser
certains défauts de JavaScript. Après l’avoir étudié correctement et
avoir travaillé avec un certain temps, j’ai vraiment appris à _apprécier_ JavaScript.

{{index future, [JavaScript, "versions of"], ECMAScript, "ECMAScript 6"}}

Il y a eu plusieurs versions de JavaScript. La version 3 d'ECMAScript a marqué
l’époque de l'ascention de JavaScript, entre 2000 et 2010 à peu près.
À cette époque, une ambitieuse version 4 était en cours d’élaboration et
prévoyait un grand nombre d'avancées radicales et d’extensions du langage.
Mais modifier un langage si largement utilisée et de façon aussi radicale était
politiquement incorrect à tel point que la version 4 fut finalement abandonnée en 2008
au profit d'une version 5 - apparut en 2009 - beaucoup moins ambitieuse et qui n'apportait
que quelques améliorations non controversées.
Il fallut attendre 2015 pour que s'installe la version 6,
une mise à jour majeure incluant certaines des idées prévues pour la version 4.
Depuis lors, le langage est régulièrement mis à jour, chaque année, par petites touches.

L'évolution continuelle du langage oblige les navigateurs à suivre en permanence
et l'utilisation d'un navigateur plus ancien ne vous permettra pas d'utiliser ses
dernières fonctionnalités. Les concepteurs du langage veillent à ne pas introduire
des changements qui empêcheraient des programmes plus anciens de fonctionner correctement
sur des navigateurs récents. Dans ce livre, j'utiliserai la version 2017 de
JavaScript.

{{index [JavaScript, "uses of"]}}

Les navigateurs Web ne sont pas les seules plateformes sur lesquelles JavaScript est
utilisé. Certaines bases de données, telles que MongoDB et CouchDB, utilisent
JavaScript comme langage de script et de requête. Plusieurs plateformes de
programmation pour postes de travail et serveurs,
notamment le projet Node.js (sujet du [Chapitre ?](node),
fournissent un environnement permettant de programmer en JavaScript en dehors du navigateur.

## Interagir et expérimenter avec le code

{{index "reading code", "writing code"}}

Le code est le texte qui compose les programmes. La plupart des chapitres de ce livre
contiennent une quantité non négligeable d'exemples de code.
D'après mon expérience, lire et écrire du ((code)) sont des
activités indispensables pour ((apprendre)) à programmer. Ne vous contentez pas
de jeter un oeil sur ces exemples, mais lisez-les vraiment attentivement de manière
à en développer une bonne compréhension.
Cela peut être long et déroutant au début, mais vous prendrez rapidement le coup.
Il en va de même pour les ((exercices)). N'estimez pas les comprendre avant d'avoir
effectivement écrit une solution qui fonctionne.

{{index interpretation}}

Je vous recommande d'essayer vos solutions aux exercices dans un interpréteur
JavaScript. De cette façon, vous pouvez vérifier interactivement le résultat de
vos travaux, ((expérimenter)) de nouvelles idées et même aller au-delà des
exercices proposés.

{{if interactive

En lisant ce livre dans votre navigateur, vous pouvez modifier (et exécuter)
tous les exemples de programmes en cliquant dessus.

if}}

{{if book

{{index download, sandbox, "running code"}}

La manière la plus simple pour exécuter les exemples de codes du livre, et
d'expérimenter, est d'utiliser la version en ligne de ce livre
[_https://eloquentjavascript.net_](https://eloquentjavascript.net/). 
Vous pouvez cliquer sur chaque exemple de façon à le modifier et/ou l'exécuter
ce qui vous permettra de voir le résultat qu'il produit. Pour travailler sur les exercices,
aller à [_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code),
où vous trouverez un point de départ pour chaque exercice ainsi que la possibilité
de voir leur solution.

if}}

{{index "developer tools", "JavaScript console"}}

Si vous souhaitez exécuter les programmes donnés dans ce livre en dehors de son site
Web, vous devrez prendre certaines précautions. De nombreux exemples sont
indépendants et devraient fonctionner dans n'importe quel environnement JavaScript.
Toutefois, le code des derniers chapitres est souvent écrit pour un environnement
spécifique (le navigateur ou Node.js) et il ne peut être exécuté qu’à cet endroit. En
outre, de nombreux chapitres définissent des programmes plus volumineux et les
éléments de code qui y figurent dépendent les uns des autres ou de fichiers externes.
Le [bac à sable](https://eloquentjavascript.net/code) du site Web fournit des liens vers des fichiers Zip contenant tous les scripts et fichiers de données nécessaires
à l'exécution du code d'un chapitre donné.

## Vue d'ensemble du livre

Ce livre contient trois parties. Les 12 premiers chapitres traitent du langage
JavaScript. Les sept chapitres suivants traitent des ((navigateurs)) Web et de la manière
dont JavaScript est utilisé pour les programmer. Enfin, deux chapitres sont consacrés
à ((Node.js)), un autre environnement qui permet de programmer en JavaScript.

Le livre contient cinq chapitres consacrés à des projets.
Cela vous permettra d'avoir un avant goût de la programmation appliquée à des situations concrêtes.
Nous y construirons successivement un [robot de distribution](robot), un [langage de
programmation](language), un [jeu de plateforme](game), un programme de [dessin pixellisé](paint) et un [site Web dynamique](skillsharing).

La partie du livre consacrée au langage proprement dit commence par quatre chapitres
qui présentent les constructions de base de JavaScript.
Ils introduisent les [structures de contrôle](program_structure)
(comme le mot `while` déjà rencontré plus tôt),
les [fonctions](functions) (pour l'écriture de vos propres blocs de construction) et
des [structures de données](data). Après cela, vous serrez en mesure
d'écrire des programmes de base.
Les chapitres [?](higher_order) et [?](object) présentent ensuite des techniques permettant
d’utiliser les fonctions et les objets pour écrire un
code de _plus haut niveau_ et garder la complexité sous contrôle.

Après un [premier chapitre de projet](robot), la découverte du langage se poursuit avec
des chapitres sur la [gestion des erreurs et la correction des bugs](error) , les [expressions
rationnelles](regexp) (un outil essentiel pour travailler avec du texte), la [modularité](modules)
(un autre moyen pour maîtriser la complexité) et la [programmation asynchrone](async) (traitement
d'événements en différé). 
Le [deuxième chapitre du projet](language) conclut la première
partie du livre.

La deuxième partie, chapitres [?](browser) à [?](paint), décrit les outils auxquels le navigateur
JavaScript a accès. Vous apprendrez à afficher des éléments à l'écran (chapitres [?](dom) et
[?](canvas)), à réagir aux actions de l'utilisateur ([chapitre ?](event)) et à communiquer en utilisant les réseaux ([chapitre 18](http)). Deux chapitres de projet accompagnent cette partie.

Après cela, le [chapitre ?](node) décrit Node.js et le [chapitre ?](skillsharing) construit un petit site Web avec cet outil.

{{if commercial

Pour finir, le [chapitre ?](fast) décrit des considérations qu'il faut avoir à l'esprit
pour optimiser la vitesse d'exécution des programmes JavaScript.

if}}

## Conventions typographiques

{{index "factorial function"}}

Dans ce livre, les portions de texte écrits dans une police à chasse fixe - comme `monospace` -
représenteront des éléments de programmes—Ce seront tantôt des fragments
autosuffisants, tantôt de simple référence à une partie d'un programme situé à 
proximité.
Les programmes (vous en avez déjà vu quelques-uns) ont l'allure suivante:

```
function factoriel(n) {
  if (n == 0) {
    return 1;
  } else {
    return factoriel(n - 1) * n;
  }
}
```

{{index "console.log"}}

Parfois, pour montrer ce que fait un programme, sa sortie attendue est écrite
juste à la fin de celui-ci avec deux barres obliques et une flèche devant.

```
console.log(factoriel(8));
// → 40320
```

En avant et bonne lecture!
