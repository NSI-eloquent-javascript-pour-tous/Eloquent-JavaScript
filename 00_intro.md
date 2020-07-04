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
d'une réflexion sur ce que ces stupides machines peuvent faire, la programmation peut être
enrichissante. Cela vous permet de faire des choses en quelques secondes qui
prendraient une _éternité_ à la main. C'est un moyen de faire en sorte que votre machine
fasse des choses qu'elle ne pouvait pas faire auparavant. Et cela fournit un
excellent exercice d'abstraction de la pensée.

La plupart des programmes sont réalisés avec des ((langages de programmation)). Un
_langage de programmation_ est un langage construit artificiellement dans le but d'
instruire les machines. Il est intéressant de noter que le moyen le plus efficace que
nous ayons trouvé de communiquer avec un ordinateur emprunte énormément à la
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
imaginaire- formée de nombreux rouages entremélés dont les relations mutuelles
doivent être maîtrisées pour réussir à animer le tout harmonieusement.

Un ((ordinateur)) est une machine physique conçu pour héberger ce genre de machines imaginaires.
Par lui-même, un ordinateur ne peut faire que des choses simplissimes. La raison
pour laquelle ils sont si utiles est qu’ils les font à une ((vitesse)) incroyablement
élevée. Un programme peut ingénieusement articuler un nombre énorme de ces
actions simples pour faire des choses très complexes.

{{index [programming, "joy of"]}}

Un programme est une construction de l'esprit. Son élaboration ne coûte rien, 
ne pèse rien et le programme pousse facilement sous nos mains qui pianotent.

Mais si on n'y prend garde, sa taille et sa ((complexité)) peuvent devenir incontrôlables,
et même son auteur risque de s'y perdre. Garder les programmes sous contrôle, c'est le
principal problème de la programmation. Quand un programme fonctionne, c'est
beau. L'art de la programmation est l'habileté à contrôler la complexité. Un
programme de grande qualité reste en retrait, sa complexité s'efface sous une apparente simplicité.

{{index "programming style", "best practices"}}

Certains programmeurs croient qu'il suffit de se limiter à quelques techniques
bien comprises pour maîtriser cette complexité. Ils élaborent des règles strictes
(«meilleures pratiques») sur la forme qu'un programme devrait avoir et se retranche
derrière leur petite zone de sécurité.

{{index experiment}}

En plus d'être à mourir d'ennui, cette approche est inefficace. À problèmes nouveaux,
solutions nouvelles. L'art de programmer, même s'il se développe rapidement,
est encore relativement jeune et la grande variété de ses sujets laisse beaucoup de place
à des approches ouvertes et audacieuses. Certes, de nombreuses et affreuses erreurs
de conception attendent le programmeur inexpérimenté, mais il vaut mieux aller de 
l'avant, tomber dans ces pièges et en tirer la leçon. Le jugement sur les qualités
d'un bon programme s'acquiert par la pratique et non en suivant aveuglément un corpus
de règles toutes faites. 

## Pourquoi le langage est important

## Why language matters

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
Il pourrait tourner sur l’ordinateur le plus simple. Pour programmer
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
while (count <= 10) {
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

## What is JavaScript?

{{index history, Netscape, browser, "web application", JavaScript, [JavaScript, "history of"], "World Wide Web"}}

{{indexsee WWW, "World Wide Web"}}

{{indexsee Web, "World Wide Web"}}

JavaScript a été introduit en 1995 afin d’ajouter des programmes aux pages Web dans
le navigateur Netscape Navigator. Le langage a depuis été adopté par tous les autres
principaux navigateurs Web graphiques. Il a rendu possible les applications Web
modernes - des applications avec lesquelles vous pouvez interagir directement sans
recharger la page pour chaque action. JavaScript est également utilisé dans des sites
Web plus traditionnels pour fournir diverses formes d'interactivité et d'intelligence.

JavaScript was introduced in 1995 as a way to add programs to web
pages in the Netscape Navigator browser. The language has since been
adopted by all other major graphical web browsers. It has made modern
web applications possible—applications with which you can interact
directly without doing a page reload for every action. JavaScript is also
used in more traditional websites to provide various forms of
interactivity and cleverness.

{{index Java, naming}}

Il est important de noter que JavaScript n’a presque rien à voir avec le langage de
programmation nommé Java. Le nom similaire a été inspiré par des considérations
marketing plutôt que par un bon jugement. Lors de l'introduction de JavaScript, le
langage Java était fortement commercialisé et gagnait en popularité. Quelqu'un a
pensé que c'était une bonne idée d'essayer de continuer sur cette lancée. Maintenant,
nous sommes coincés avec le nom.

It is important to note that JavaScript has almost nothing to do with
the programming language named Java. The similar name was inspired by
marketing considerations rather than good judgment. When JavaScript
was being introduced, the Java language was being heavily marketed and
was gaining popularity. Someone thought it was a good idea to try to
ride along on this success. Now we are stuck with the name.

{{index ECMAScript, compatibility}}

Après son adoption en dehors de Netscape, un document standard a été rédigé pour
décrire la manière dont le langage JavaScript devrait fonctionner de sorte que les
différents logiciels prétendant prendre en charge JavaScript parlent en réalité du
même langage. C'est ce qu'on appelle le standard ECMAScript, d'après l'organisation
Ecma International qui a effectué la normalisation. En pratique, les termesECMAScript et JavaScript peuvent être utilisés indifféremment: ce sont deux noms
pour le même langage.

After its adoption outside of Netscape, a ((standard)) document was
written to describe the way the JavaScript language should work so
that the various pieces of software that claimed to support JavaScript
were actually talking about the same language. This is called the
ECMAScript standard, after the Ecma International organization that
did the standardization. In practice, the terms ECMAScript and
JavaScript can be used interchangeably—they are two names for the same
language.

{{index [JavaScript, "weaknesses of"], debugging}}

Il y a ceux qui vont dire des choses terribles sur JavaScript. Beaucoup de ces choses
sont vraies. Quand on m'a demandé d'écrire quelque chose en JavaScript pour la
première fois, j'ai rapidement fini par le mépriser. Il accepterait presque tout ce que je
taperais, mais l'interpréterait d'une manière complètement différente de ce que je
voulais dire. Cela tenait beaucoup au fait que je n'avais aucune idée de ce que je
faisais, bien sûr, mais il y a un réel problème ici: JavaScript est ridiculement libéral
dans ce qu'il permet. L'idée derrière cette conception était de faciliter la
programmation en JavaScript pour les débutants. En réalité, il est généralement plus
difficile de trouver des problèmes dans vos programmes car le système ne vous les
signalera pas.

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

There are those who will say _terrible_ things about JavaScript. Many
of these things are true. When I was required to write something in
JavaScript for the first time, I quickly came to despise it. It would
accept almost anything I typed but interpret it in a way that was
completely different from what I meant. This had a lot to do with the
fact that I did not have a clue what I was doing, of course, but there
is a real issue here: JavaScript is ridiculously liberal in what it
allows. The idea behind this design was that it would make programming
in JavaScript easier for beginners. In actuality, it mostly makes
finding problems in your programs harder because the system will not
point them out to you.

{{index [JavaScript, "flexibility of"], flexibility}}

Cette flexibilité a aussi ses avantages. Cela laisse place à de nombreuses techniques
impossibles dans des langages plus rigides et, comme vous le verrez (par exemple au
chapitre 10 ), il peut être utilisé pour surmonter certaines des faiblesses de JavaScript.
Après avoir appris la langue correctement et travaillé avec elle pendant un certain
temps, j'ai appris à aimer réellement JavaScript.

Toutefois, la souplesse de ce langage est aussi un avantage. Elle laisse
la place à de nombreuses techniques que les langages de programmation
plus rigides ne permettent pas. Et, comme vous le verrez (par exemple
dans le [chapitre ?](modules), on peut l’utiliser pour compenser
certains défauts de JavaScript. Après l’avoir étudié correctement et
avoir travaillé avec un certain temps, j’ai vraiment appris à _apprécier_ JavaScript.

This flexibility also has its advantages, though. It leaves space for
a lot of techniques that are impossible in more rigid languages, and
as you will see (for example in [Chapter ?](modules)), it can be used
to overcome some of JavaScript's shortcomings. After ((learning)) the
language properly and working with it for a while, I have learned to
actually _like_ JavaScript.

{{index future, [JavaScript, "versions of"], ECMAScript, "ECMAScript 6"}}

Il y a eu plusieurs versions de JavaScript. ECMAScript version 3 était la version
largement prise en charge à l’époque de la domination de JavaScript, entre 2000 et
2010. À l’époque, une ambitieuse version 4 était en cours d’élaboration, qui
prévoyait un certain nombre d’améliorations et d’extensions du langage. Changer une
langue vivante et largement utilisée de manière aussi radicale s’est avéré
politiquement difficile, et les travaux sur la version 4 ont été abandonnés en 2008, ce
qui a conduit à une version 5 beaucoup moins ambitieuse, qui n’apportait que
quelques améliorations non controversées, à paraître en 2009 Puis, en 2015, la
version 6 est sortie, une mise à jour majeure incluant certaines des idées prévues pour
la version 4. Depuis lors, nous avons eu de nouvelles petites mises à jour chaque
année.

There have been several versions of JavaScript. ECMAScript version 3
was the widely supported version in the time of JavaScript's ascent to
dominance, roughly between 2000 and 2010. During this time, work was
underway on an ambitious version 4, which planned a number of radical
improvements and extensions to the language. Changing a living, widely
used language in such a radical way turned out to be politically
difficult, and work on the version 4 was abandoned in 2008, leading to
a much less ambitious version 5, which made only some uncontroversial
improvements, coming out in 2009. Then in 2015 version 6 came out, a
major update that included some of the ideas planned for version 4.
Since then we've had new, small updates every year.

Le fait que la langue évolue oblige les navigateurs à suivre en permanence. Si vous
utilisez un navigateur plus ancien, il peut ne pas prendre en charge toutes les
fonctionnalités. Les concepteurs de langues veillent à ne pas modifier les
programmes existants, de sorte que les nouveaux navigateurs peuvent toujoursexécuter les anciens programmes. Dans ce livre, j'utilise la version 2017 de
JavaScript.

The fact that the language is evolving means that browsers have to
constantly keep up, and if you're using an older browser, it may not
support every feature. The language designers are careful to not make
any changes that could break existing programs, so new browsers can
still run old programs. In this book, I'm using the 2017 version of
JavaScript.

{{index [JavaScript, "uses of"]}}

Les navigateurs Web ne sont pas les seules plateformes sur lesquelles JavaScript est
utilisé. Certaines bases de données, telles que MongoDB et CouchDB, utilisent
JavaScript comme langage de script et de requête. Plusieurs plates-formes de
programmation de postes de travail et de serveurs, notamment le projet Node.js (objet
du chapitre 20 ), fournissent un environnement permettant de programmer JavaScript
en dehors du navigateur.

Web browsers are not the only platforms on which JavaScript is used.
Some databases, such as MongoDB and CouchDB, use JavaScript as their
scripting and query language. Several platforms for desktop and server
programming, most notably the ((Node.js)) project (the subject of
[Chapter ?](node)), provide an environment for programming JavaScript
outside of the browser.

## Code, and what to do with it

## Code, and what to do with it

{{index "reading code", "writing code"}}

Le code est le texte qui compose les programmes. La plupart des chapitres de ce livre
contiennent beaucoup de code. Je crois que lire du code et écrire du code sont des
éléments indispensables pour apprendre à programmer. Essayez de ne pas
simplement regarder les exemples, lisez-les attentivement et comprenez-les. Cela
peut paraître lent et déroutant au début, mais je vous promets que vous allez vite
comprendre. La même chose vaut pour les exercices. Ne supposez pas que vous les
comprenez jusqu'à ce que vous ayez réellement écrit une solution efficace.

_Code_ is the text that makes up programs. Most chapters in this book
contain quite a lot of code. I believe reading code and writing ((code))
are indispensable parts of ((learning)) to program. Try to not just
glance over the examples—read them attentively and understand them.
This may be slow and confusing at first, but I promise that you'll
quickly get the hang of it. The same goes for the ((exercises)). Don't
assume you understand them until you've actually written a working
solution.

{{index interpretation}}

Je vous recommande d'essayer vos solutions aux exercices dans un interpréteur
JavaScript réel. De cette façon, vous obtiendrez un retour immédiat sur le résultat de
vos travaux et j'espère que vous serez tenté d'expérimenter et d'aller au-delà des
exercices.

I recommend you try your solutions to exercises in an actual
JavaScript interpreter. That way, you'll get immediate feedback on
whether what you are doing is working, and, I hope, you'll be tempted
to ((experiment)) and go beyond the exercises.

{{if interactive

Lorsque vous lisez ce livre dans votre navigateur, vous pouvez modifier (et exécuter)
tous les exemples de programmes en cliquant dessus.

When reading this book in your browser, you can edit (and run) all
example programs by clicking them.

if}}

{{if book

{{index download, sandbox, "running code"}}

The easiest way to run the example code in the book, and to experiment
with it, is to look it up in the online version of the book at
[_https://eloquentjavascript.net_](https://eloquentjavascript.net/). There,
you can click any code example to edit and run it and to see the
output it produces. To work on the exercises, go to
[_https://eloquentjavascript.net/code_](https://eloquentjavascript.net/code),
which provides starting code for each coding exercise and allows you
to look at the solutions.

if}}

{{index "developer tools", "JavaScript console"}}

Si vous souhaitez exécuter les programmes définis dans ce livre en dehors du site
Web du livre, vous devrez prendre certaines précautions. De nombreux exemples sont
indépendants et devraient fonctionner dans n'importe quel environnement JavaScript.
Toutefois, le code des chapitres ultérieurs est souvent écrit pour un environnement
spécifique (le navigateur ou Node.js) et ne peut être exécuté qu’à cet endroit. En
outre, de nombreux chapitres définissent des programmes plus volumineux et les
éléments de code qui y figurent dépendent les uns des autres ou de fichiers externes.
Le bac à sable du site Web fournit des liens vers des fichiers Zip contenant tous les
scripts et fichiers de données nécessaires à l'exécution du code d'un chapitre donné.

If you want to run the programs defined in this book outside of the
book's website, some care will be required. Many examples stand on their
own and should work in any JavaScript environment. But code in later
chapters is often written for a specific environment (the browser or
Node.js) and can run only there. In addition, many chapters define
bigger programs, and the pieces of code that appear in them depend on
each other or on external files. The
[sandbox](https://eloquentjavascript.net/code) on the website provides
links to Zip files containing all the scripts and data files
necessary to run the code for a given chapter.

## Vue d'ensemble du livre

## Overview of this book

Ce livre contient environ trois parties. Les 12 premiers chapitres traitent du langage
JavaScript. Les sept chapitres suivants traitent des navigateurs Web et de la manière
dont JavaScript est utilisé pour les programmer. Enfin, deux chapitres sont consacrés
à Node.js, un autre environnement dans lequel programmer JavaScript.

This book contains roughly three parts. The first 12 chapters discuss
the JavaScript language. The next seven chapters are about web
((browsers)) and the way JavaScript is used to program them. Finally,
two chapters are devoted to ((Node.js)), another environment to
program JavaScript in.

Tout au long du livre, il y a cinq chapitres de projet , décrivant des exemples de
programmes plus vastes pour vous donner un aperçu de la programmation réelle. Par
ordre d'apparition, nous allons construire un robot de distribution , un langage de
programmation , un jeu de plate - forme , un programme de peinture au pixel et un
site Web dynamique .

Throughout the book, there are five _project chapters_, which describe
larger example programs to give you a taste of actual programming. In
order of appearance, we will work through building a [delivery
robot](robot), a [programming language](language), a [platform
game](game), a [pixel paint program](paint), and a [dynamic
website](skillsharing).

La partie linguistique du livre commence par quatre chapitres qui présentent la
structure de base du langage JavaScript. Ils introduisent des structures de contrôle
(comme le while mot que vous avez vu dans cette introduction), des fonctions
(écriture de vos propres blocs de construction) et des structures de données . Après
cela, vous pourrez écrire des programmes de base. Les chapitres 5 et 6 présentent
ensuite des techniques permettant d’utiliser des fonctions et des objets pour écrire un
code plus abstrait et garder la complexité sous contrôle.

The language part of the book starts with four chapters that introduce
the basic structure of the JavaScript language. They introduce
[control structures](program_structure) (such as the `while` word you
saw in this introduction), [functions](functions) (writing your own
building blocks), and [data structures](data). After these, you will
be able to write basic programs. Next, Chapters [?](higher_order) and
[?](object) introduce techniques to use functions and objects to write
more _abstract_ code and keep complexity under control.

Après un premier chapitre de projet , la partie linguistique du livre se poursuit avec
des chapitres sur la gestion des erreurs et la correction des bugs , les expressions
régulières (un outil essentiel pour travailler avec du texte), la modularité (un autre
moyen de défense contre la complexité) et la programmation asynchrone (traitement
d'événements prendre du temps). Le deuxième chapitre du projet conclut la première
partie du livre.

After a [first project chapter](robot), the language part of the book
continues with chapters on [error handling and bug fixing](error),
[regular expressions](regexp) (an important tool for working with
text), [modularity](modules) (another defense against complexity), and
[asynchronous programming](async) (dealing with events that take
time). The [second project chapter](language) concludes the first part
of the book.

La deuxième partie, les chapitres 13 à 19 , décrit les outils auxquels le navigateur
JavaScript a accès. Vous apprendrez à afficher des éléments à l'écran (chapitres 14 et
17 ), à réagir aux entrées de l'utilisateur ( chapitre 15 ) et à communiquer via le
réseau ( chapitre 18 ). Il y a encore deux chapitres de projet dans cette partie.

The second part, Chapters [?](browser) to [?](paint), describes the
tools that browser JavaScript has access to. You'll learn to display
things on the screen (Chapters [?](dom) and [?](canvas)), respond to
user input ([Chapter ?](event)), and communicate over the network
([Chapter ?](http)). There are again two project chapters in this
part.

Après cela, le chapitre 20 décrit Node.js et le chapitre 21 construit un petit site Web à
l'aide de cet outil.

After that, [Chapter ?](node) describes Node.js, and [Chapter
?](skillsharing) builds a small website using that tool.

{{if commercial

Finally, [Chapter ?](fast) describes some of the considerations that
come up when optimizing JavaScript programs for speed.

if}}

## Conventions typographiques

## Typographic conventions

{{index "factorial function"}}

Dans ce livre, les textes écrits dans une monospaced police de caractères
représenteront des éléments de programmes. Ce sont parfois des fragments
autosuffisants, et ils ne font parfois que référence à une partie d'un programme à
proximité. Les programmes (dont vous en avez déjà vu quelques-uns) s’écrit comme
suit:

In this book, text written in a `monospaced` font will represent
elements of programs—sometimes they are self-sufficient fragments, and
sometimes they just refer to part of a nearby program. Programs (of
which you have already seen a few) are written as follows:

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

Parfois, pour afficher la sortie produite par un programme, la sortie attendue est écrite
après celle-ci, avec deux barres obliques et une flèche devant.

Sometimes, to show the output that a program produces, the
expected output is written after it, with two slashes and an arrow in
front.

```
console.log(factoriel(8));
// → 40320
```

Bonne lecture!

Good luck!
