---
title: Convertisseur de températures en HTML/JAVASCRIPT
papersize: a4
geometry: margin=2cm
fontsize: 10pt
toc-depth: 2
lang: fr
---

<!--
Compilé avec :
pandoc --pdf-engine=xelatex  --template my.latex --highlight-style tango -N readme.md -o wator.pdf
https://download.tuxfamily.org/linuxvillage/Informatique/Cours-Shell-prog/cours_shell_unix.pdf
<https://cache.media.eduscol.education.fr/file/NSI/76/6/RA_Lycee_G_NSI_algo_knn_1170766.pdf>
--->

![Héritage](640px-Russian-Matroshka.jpg)

# Introduction

Il s\'agit de réaliser une page web permettant de faire un convertisseur 
de températures entre différentes unités. Dans un premier temps on se 
limitera a la conversion
d\'une valeur en Kelvin (K) en degré Ceclius (°C). Par la suite on
rajoutera le degré Fahrenheit et le degré Rankine. Ceci sera réalisé à
 l'aide d'un navigateur internet. 
 
 Pour réaliser cette activité vous aurez besoin :
 
 - D'un navigateur internet pour afficher la page et utiliser les outils
 de développeurs web
 - D'un éditeur de texte avec de préférence la coloration syntaxique comme
 par exemple `geany`, `notepad++`. Une liste est disponible ici :
 <https://fr.wikipedia.org/wiki/%C3%89diteur_de_texte>
 
Pour finir cette introduction on peut préciser :
 
 - Le langage `HTML` est un langage de descriptions qui utilise des balises
 . Exemple la balise heading (titre) de niveau 1 s'écrit `<h1>TITRE</h1>`.
 C'est le fichier HTML qui contient le fond du document.
 - Le langage `CSS` est un langage de présentation. Il permet la mise en forme
 du document. (taille, placement, couleur etc...)
 - Le langage `JAVASCRIPT` est un langage de programmation interprété par 
 le navigateur internet permettant de rendre interactive des pages web.

Le document HTML sera principalement constitué :

- d'un titre
- d'une zone où l'utilisateur rentre une valeur numérique avec son unité.
- d'une zone de résultat affichant le résultat de la conversion dans les autres
    unités

Pour l'unité, on acceptera les conventions d'écritures suivantes :

- Pour les degrés Celcius : °C, °c, C ou c
- Pour les Kelvin: K, k
- Pour les degrés Fahrenheit : °F, °f, F ou f
- Pour les Rankine: °R , °r , R ou r

# Version 1

## Simplification des fonctionnalités

Dans un premier temps nous chercherons à réaliser une ébauche de programme
fonctionnelle. Le but étant de réaliser un convertisseur de
températures, il faut réaliser un convertisseur de Kelvin en degré
Celcius.

## Première version du `index.html`

> En général la page par défaut des serveurs HTTP est le fichier nommé
`index.html`

1. ![Q] Utiliser votre éditeur de texte préféré pour créer le fichier `index.html`
 avec le contenu suivant :
 
~~~html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Numérotation d'une plaque d'immatriculation Française</title>
    <style type="text/css">
        html {
            margin: 2px;
            background-color: yellow;
        }
        body {
            background-color: silver;
        }
        h1 {
            background-color: purple;
            border: 2px solid red;
            text-align : center;
        }
        div {
            border: 2px dashed green;
            margin: 2px;
            text-align: center;
        }
    </style>
</head>
<body>
<h1>Convertisueur de températures</h1>
<div id="page">
    <div id="saisie"><h2>Zone de saisie</h2>
        <input type="text" id="entree" size="5">
        <input type="button" id="bouton" value="Conversion">
    </div>
    <div id="resultats">
        <h2>Zone de résultats</h2>
        <div class="conversion">
            <h3>En Kelvin</h3>
            <span id="rep_K"></span>
        </div>
    </div>
</div>
<script>
    alert('Page chargée');
</script>
</body>
</html>
~~~

> Une balise HTML peut contenir un attribut `id`. Celui-ci permet de 
donner un identifiant unique à cette balise.
 
1. ![Q] Corriger la faute d’orthographe dans le titre de niveau 1 `h1`
1. ![Q] Corriger la faute de sémantique dans le titre du doucement `title`
1. ![Q] Ouvrir le fichier à l'aide d'un navigateur internet.
1. ![Q] En utilisant le `CSS`, modifier la couleur de fond de la balise `body`.
(suivre le lien une liste de couleurs : <https://developer.mozilla.org/fr/docs/Web/CSS/Type_color)

Quelques remarques:

- la balise `div` permet de faire une boite (un conteneur).
- le code `CSS` entoure en pointillé vert les balises `div`.
- noter que le texte "En Kelvin" est bien entouré dans trois conteneurs `div`.
- la boite de dialogue affichant "Page chargée" est du au code javascript :
`alert('Page chargée');` (sans oublier le `;` à la fin)

# Utilisation des navigateurs internet

Depuis quelques années les navigateurs internet modernes intègrent des outils
pour faciliter le travail des développeurs web. On dispose alors principalement
 de trois outils :
 
 - Une console pour examiner/modifier le code `HTML`
 - Une console pour interpréter le code `JAVASCRIPT`
 - Une console pour examiner/modifier le code `CSS`
 
Pour accéder à ces outils il suffit de presser la touche `F12` du clavier.

## Console `JAVASCRIPT`

Le message d'alerte à l'ouverture de la page est assez gênant.

1. ![Q] Changer la ligne `alert('Page chargée');` par `console.log('Page chargée');`
1. ![Q] Ouvrir la console `JAVASCRIPT` pour vérifier la présence du message.

## Code `JAVASCRIPT`

Python et JAVASCRIPT étant tous les deux des langages interprétés vous pouvez utiliser
 la console pour faire des "tests". Une fois les tests
concluants vous pouvez écrire les instructions dans le fichier source.

Ici le code JavaScript doit :

1.  Récupérer le texte (valeur+unité) rentré par l'utilisateur.
2.  Séparer le texte saisi en valeur et unité.
3.  Réaliser la (les) conversions.
4.  Générer pour chaque conversion le code HTML pour son affichage.
5.  Insérer dans la page HTML l'affichage du résultat.

## Récupérer le texte saisi

Dans un premier temps, nous utiliserons un bouton pour lancer la conversion. 
Lorsque l'utilisateur click sur le bouton un événement se produit.
C'est cet événement qui produira l’exécution de la conversion d'unité.
Dans ce cas on parle de programmation événementielle.

Il faut donc :

-   Sélectionner l'élément sur lequel l'événement doit se produire.
-   Associer à l'élément le type d'événement et le nom de la fonction
    a lancer lorsque l'événement se produit.

Ici l'élément qui nous intéresse est bien entendu le champ input. Comme
celui-ci possède un `id` ayant la valeur `entree`, on utilisera la méthode
`document.getElementById("entree")`'.

Dans la console `JAVASCRIPT` essayer :

- `alert('ok')`
- `document.getElementById('ENTREE')`
- `document.getElementById('entree')`
- `saisie = document.getElementById('entree')`
- `saisie.value`

Utiliser le navigateur pour mettre une valeur dans la zone de saisie et
reprendre la dernière instruction.

## Programmation événementielle

La programmation événementielle réagit aux événements. On aurait pu
utiliser un bouton et réagir lorsque l'utilisateur click sur le bouton.
Il faut donc mettre en place un "écouteur" d'événement. Celui-ci lancera
une fonction lorsque l'événement se produira.

## Fonction en JAVASCRIPT

Pour déclarer une fonction en `JAVASCRIPT` on utilise le mot clé `function`.
Ci-dessous le code de la fonction `click_bouton` permettant de lancer une 
boite de dialogue avec le message 'Vous venez de clicker sur le bouton!' :

~~~javascript
function click_bouton() {
    alert('Vous venez de clicker sur le bouton!');
}
~~~

1. ![Q] Modifier le contenu de la page HTML pour y insérer le code JAVASCRIPT
précédent. Ne pas oublier d'enregistrer le fichier et actualiser le navigateur
 internet (F5)
1. ![Q] Dans la console, lancer "à la main" la fonction en saisissant : `click_bouton()`
(la boite de dialogue doit apparaître).


## Écouteur d'événement

Pour mettre en place un écouteur d'événement il faut trois éléments :

1. Sur quel élément l'écouteur doit-il écouter ?
1. A quel événement doit-il réagir ? (<https://developer.mozilla.org/fr/docs/Web/Events>)
1. Quelle action doit produire l'événement ?

Réponses :

1. Sur la zone de saisie d'`id` = "entree"
1. Sur l'événement `click`
1. Doit lancer la fonction `click_bouton`


Quelques instructions JAVASCRIPT :

~~~javascript
bouton = document.getElementById('bouton');
bouton.addEventListener("click",click_bouton);
~~~

1. ![Q] Ouvrir la console JAVASCRIPT et essayer les instructions ci-dessus:
1. ![Q] Modifier alors le contenu de la zone de saisie et valider par
la touche ENTREE. Le message d'alerte doit apparaitre.
1. ![Q] Modifier le contenu de la page HTML pour y insérer le code JAVASCRIPT
précédent. Ne pas oublier d'enregistrer le fichier et actualiser le navigateur
 internet (F5). Vérifier.

## Séparer le texte saisi en valeur et unité

Ici en vue des simplifications il n'y a rien à faire puisque que l'on
suppose que la valeur saisie est en Kelvin.

# Réaliser la conversion

Il nous faut convertir des K en °C. Ci-dessous la structure de la
fonction de conversion.

~~~javascript
function conv_K2C(t) { 
    return "?????";
    }
~~~

1. ![Q] Remplacer les ?????. 
1. ![Q] Faire des essais dans la console. 

## Affichage du résultat.

### Affichage "quasi" statique

La page HTML contient un élément d'id `rep_K` pour la réponse en Kelvin.
Ci-dessous quelques instructions JAVASCRIPT à essayer dans la console:

~~~javascript
reponse = document.getElementById('rep_K')
reponse.innerHTML = 123
~~~

1. ![Q] Modifier le fichier HTML pour terminer cette première version de
programme

### Affichage dynamique

Pour la suite des conversions nous pourrions écrire en "dur" les éléments 
de réponse pour les différentes unités. Je vous propose plutôt
d'écrire à la volée le code HTML correspondant. C’est-à-dire que c'est le
programme JAVASCRIPT qui "écrit" le code HTML. Avant cela il nous faut introduire
 l'arbre DOM.

# Arbre DOM

> Le Document Object Model (DOM) est une interface de programmation 
normalisée par le W3C, qui permet à des scripts d'examiner et de modifier 
le contenu du navigateur web.

Ci-dessous une représentation partielle de l'arbre DOM du document:

![Arbre DOM](arbre_dom.png)\

Quelques mots de vocabulaire

- Le rectangle (ou nœud) du haut (`HTML`) est la racine de l'arbre.
- Les rectangles en dessous sont les nœuds fils. `head` est un enfant ou 
fils de `HTML`
- Les rectangles du "dessus" sont les pères. `title` a pour père `head`

> A noter que le `h3` en bas à droite, celui de la valeur en Kelvin, 
est bien "emboîté" dans trois `div` comme illustré par les pointillés verts
 crées en `CSS`.
 
1. ![Q] Compléter la figure en y indiquant les différents `id`.

## Création dynamique de contenu HTML

La zone de résultat pour la température en Kelvin est constituée :

1. D'un conteneur `div`
1. D'un titre de niveau 3 `h3` avec le nom de l'unité
1. Une balise `span` pour le résultat

Le conteneur est lui contenu dans le conteneur `resultats`

Pour créer une balise HTML en JAVASCRIPT il suffit d'appeler l'instruction
 `document.createElement(NOM_DE_LA_BALISE)`.

Quelques instructions:
 
- crée une variable `nv_div` qui contient la nouvelle
  balise `div`
  
~~~javascript
var nv_div = document.createElement("div");
~~~

 - crée une variable `texte_unite` qui contient la nouvelle
  balise `h3`

~~~javascript
var texte_unite = document.createElement("h3");
~~~

- Modifie le contenu HTML de la balise `h3`

~~~javascript
var texte_unite.innerHTML = "°F";
~~~

- Déclare la balise `h3` enfant du `div`
 
~~~javascript
nv_div.appendChild(texte_unite);
~~~

## Automatisation

Il nous faut donc créer une fonction qui :

- Prends en arguments la valeur à afficher et son unité.
- Génère le code HTML
- Déclare le code HTML enfant du conteneur ayant pour id="resultats"

~~~javascript
function affiche_resultat(valeur,unite) {
  var nv_div = document.createElement("div");
  
  var texte_unite = document.createElement("h3");
  texte_unite.innerHTML = unite;
  
  var texte_valeur = document.createElement("span");
  texte_valeur.innerHTML = valeur.toFixed(2);
  
  nv_div.appendChild(texte_unite);
  nv_div.appendChild(texte_valeur);
  
  var div_pere = document.getElementById("resultats");
  div_pere.appendChild(nv_div);
}
~~~

Vous pouvez dès à présent essayer depuis la console la fonction. 
Par exemple : affiche_resultat(273.15,"K") , affiche_resultat(32,"°F") etc...


> Vous remarquez que l'aspect graphique n'est pas optimal, mais il sera
modifié par la suite

Vous avez tous les éléments nécessaires pour finaliser cette version. 
Je vous laisse le soin de rendre se programme fonctionnel.


# En route vers la version finale

## Degré Fahrenheit

On supposera toujours que la valeur entrée est en K.

1. ![Q] Réaliser la fonction `conv_K2F`
2. ![Q] Ajouter l'affichage de la température en `°F`

Par défaut l'élément div est de type bloc. C’est-à-dire qu'il prendra
toute la largeur disponible (ici la page). Si l'on souhaite mettre les résultats les
uns à coté des autres il faut modifier en `CSS` la propriété `display`. Il est à
noter qu'il y a plusieurs conversions. Il faudra alors utiliser une
classe `CSS` nommée par exemple "conversion".

Il nous faut donc:

1.  Créer la classe `CSS` nommée "conversion" et modifier la propriété
    `display` en \"inline-block\". On pourra aussi fixer la largeur avec
    la propriété `width`.
2.  Modifier la création dynamique des résultats pour mettre l'attribut
    class à "conversion". Ceci sera réalisé en ajoutant la ligne
    suivante :
    `nv_div.setAttribute("class","conversion");`

![Affichage du résultat de conversion](affichage_resultat.png)\


1. ![Q] Dans le même état d'esprit je vous laisse le soin de créer une classe
`CSS` pour l'affichage de la valeur numérique de la conversion

### Détection de l'unité de base

Une fois de plus plusieurs solutions s'offrent à nous. Je vous propose
d\'utiliser les expressions régulières pour traiter ce problème.

#### (Trop) Petite introduction aux expressions régulières

On peut utiliser des regex (expressions régulières) dans de nombreux
cas. C\'est très utile pour chercher quelque chose de précis dans un
texte. D\'ailleurs de nombreux éditeurs de texte proposent les regex
dans leur outil de recherche. 

Tout de suite quelques exemples tirés de wikipédia :
<https://fr.wikipedia.org/w/index.php?title=Expression_rationnelle>

-   `chat|chien` : correspond aux chaînes de caractères « chat » ou
    « chien » (et seulement à celles-ci), n\'importe où dans le texte
    (exemple : « chaton »).
-   `[cC]hat|[cC]hien` : correspond aux chaînes « chat », « Chat »,
    « chien » ou « Chien » (et seulement à celles-ci), n\'importe où
    dans le texte (exemple : « Chat » dans « Chats et chiens »).
-   `chu+t` : correspond à « chut », « chuut », « chuuut », etc.,
    n'importe où dans le texte.
-   `a[ou]+` : correspond à « aou », « ao », « auuu »,
    « aououuuoou », etc., n'importe où dans le texte.
-   `peu[xt]?` : correspond à « peu », « peux » et « peut » (et
    seulement à ces chaînes), n'importe où dans le texte. La recherche
    retourne le texte le plus long possible en cas d'occurrences
    multiples à la même position.
-   `^[st]ac` : représente les chaînes « sac » et « tac » en début de
    ligne.
-   `[st]ac$` : représente les chaînes « sac » et « tac » en fin de
    ligne ou de texte (par exemple à l\'intérieur de « ressac »).
-   `^trax$` : représente la chaîne « trax » seule sur une ligne.

On remarque que:

-   \^ : signifie début de ligne
-   \$ : signifie fin de ligne
-   ? : qui existe zéro ou une fois
-   \+ : qui existe une ou plusieurs fois
-   \* : qui existe zéro, une ou plusieurs fois
-   \| : (pipe) opérateur entre plusieurs alternatives (ou logique)
-   \[\] : définit un ensemble de caractères possibles

> Ceci n\'est qu\'une très courte introduction aux expressions régulières.
Il existe des livres entiers consacrés aux expressions régulières. Il
est possible d\'utiliser des regex dans de nombreux langages. Je vous
recommande de connaître les bases, et d\'approfondir au cas par cas
selon les besoins.


#### Regex en JavaScript

L\'objet RegExp de JavaScript permet d\'utiliser des expressions
rationnelles. Pour tous les détails voir
<https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/Expressions_r%C3%A9guli%C3%A8res>

On y apprend :

-   qu\'un ensemble de caractères se note entre \[\] : \[abc\] pour 
    les caractères \"a\",\"b\" et \"c\".
-   Un intervalle est délimité par un \"-\" : \[a-z\] pour tous les caractères de \"a\" à \"z\"
-   \\d est un raccourci pour \[0-9\]
-   \\D est le complément à \\d (tout sauf \\d)
-   \\w est un raccourci pour \[a-z\]
-   \\W est le complément à \\w (tout sauf \\w)
-   () permet de \"sélectionner\" une zone

Ici notre chaîne à traiter est constituée de chiffres avec ou sans
séparateur décimal et éventuellement du texte pour l\'unité.
Dans notre cas la méthode `match` de l\'objet `RegExp` est la plus
appropriée. Je vous conseille fortement d\'essayer les lignes suivantes
dans la console.

~~~javascript
'Bonjour, il fait 22.3 °C dans la salle'.match(/\w/)
'Bonjour, il fait 22.3 °C dans la salle'.match(/(\w)/)
'Bonjour, il fait 22.3 °C dans la salle'.match(/(\w+)/)
'Bonjour, il fait 22.3 °C dans la salle'.match(/(\w+)$/)
'Bonjour, il fait 22.3 °C dans la salle'.match(/(\d)/)
'Bonjour, il fait 22.3 °C dans la salle'.match(/(\d+)/)
'Bonjour, il fait 22.3 °C dans la salle'.match(/(\d+\.\d)/)
~~~

Je vous laisse le soin de trouver l\'expression régulière que l\'on
cherche. Il faut remarquer ici que la méthode match retourne un tableau
les éléments d\'indices 1 et 2 devraient fortement nous intéresser.

### Fonctions de conversions

A ce stade nous pourrions presque dire que l\'on a fini. C\'est à dire
que le plus \"difficile\" est fait. Il nous reste a réaliser l\'ensemble
des conversions possibles.


![Ensemble des conversions](conversions.png)\


Toute valeur sera convertie dans l\'unité de référence, ici, le Kelvin.
Si bien que la conversion F2R = K2R(F2K). Je vous laisse le soin
d\'écrire les six fonctions de conversion.

Version finale
--------------

Réaliser la version finale que l\'on pourra agrémenter d\'un zeste de
CSS. Attention toutefois à ne pas perdre son temps avec du CSS.

[Q]: gears.png
