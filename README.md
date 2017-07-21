Format réglementaire pour la publication des données essentielles des conventions de subventions françaises
===================================================================================================

### Version 0.1.0

Ce dépôt est destiné à accueillir :

- la documentation du format
- des exemples de données

Seront ajoutés courant août 2017 :

- un schéma [CSVW](https://www.w3.org/TR/2015/REC-tabular-data-model-20151217/) pour permettre la validation des données publiées
- des exemples de données
- le lien vers l'arrêté qui définit la sémantique des champs
- un sujet sur [le forum Etalab](https://forum.etalab.gouv.fr/) pour recueillir des questions et les commentaires sur ce format de données

# Cadre réglementaire

Ce format a été conçu parallèlement et conformément aux textes suivants :

- [Article 8 de la loi n° 2010-237 du 9 mars 2010 de finances rectificative pour 2010](https://www.legifrance.gouv.fr/affichTexteArticle.do?cidTexte=JORFTEXT000021943745&idArticle=JORFARTI000021943775&categorieLien=cid)
-  [Décret du 9 mai 2017 relatif aux possibilités de cumuler des aides à l'investissement pour la construction, l'acquisition et l'amélioration de logements locatifs aidés, avec les subventions versées au titre de certaines actions du programme d'investissements d'avenir](https://www.legifrance.gouv.fr/eli/decret/2017/5/9/LHAL1705272D/jo)
- L'arrêt est en cours de publication


L'objectif est de normaliser la publication des données des conventions de subventions afin de les regrouper, de les rendre publiques, et de permettre à tout un chacun d'appliquer des traitements automatiques :

- analyses
- visualisation de données

Depuis le 1er mars 2017, l'ensemble des attribuants de subventions, à l'exception des collectivités de moins de 3 500 habitants, sont tenus de publier ces données dans le format décrit sur ce dépôt. D'après l'article 2 du décret, ces données doivent être publiées :

- dans les 3 mois suivants la signature de la convention de subvention
- *soit* sur le site Internet de la collectivité attribuante
- *soit* sur le portail interministériel des données ouvertes [data.gouv.fr](http://data.gouv.fr)
- un lien vers le jeu de données doit être publié sur le site Internet de la collectivité attribuante

Veuillez vous référer à la lettre du décret pour vous assurer du bon respect des obligations réglementaires.

# Un format tabulaire

## Une structure des données simple

Le format CSV (*Comma Separated Values*, valeurs séparées par des virgules) a été choisi en raison de sa simplicité et de la facilité qu'ont les logiciels à le produire, y compris ceux qui datent du siècle dernier.

Ce format était en concurrence avec des formats plus modernes tels que XML ou JSON, mais la structure des données des subventions n'était pas suffisamment complexe pour justifier l'utilisation d'un format en arbre.

En effet, la structure décrite dans le décret se prête bien à la représentation sous forme de lignes et de colonnes. Voici la liste des colonnes, dans l'ordre :

- `nomAttribuant`
- `idAttribuant`
- `dateDecision`
- `referenceDecision`
- `nomBeneficiaire`
- `idBeneficiaire`
- `objet`
- `montant`
- `nature`
- `conditionsVersement`
- `datesPeriodeVersement`
- `idRAE`
- `notificationDeMinimisUE`
- `pourcentageSubvention`


La seule subtilité réside dans le besoin de créer **une ligne par bénéficiaire**. Dans ce cas, toutes les données de la subvention sont répétées, à l'exception des colonnes suivantes :

- `nomBeneficiaire`
- `idBeneficiaire`
- `pourcentageSubvention`

Dans certains cas, les colonnes `conditionsVersement` et `datesPeriodeVersement` peuvent également varier d'un bénéficiaire à un autre, pour une même subvention.

## Recommandations de formatage

Le format CSV, contrairement au XML et au JSON, n'a pas une syntaxe très stricte :

- on peut utiliser des virgules ou des point-virgules pour séparer les champs
- on peut entourer certains champs de guillemets, ou seulement en utiliser pour repérer les champs dont la valeur contient le séparateur (virgule ou point-virgule)
- on peut utiliser l'encodage UTF-8 (Unicode) ou un encodage restreint comme ASCII
- etc.

L’émergence de ce format a pour objectif de faciliter le regroupement des données des subventions et de faciliter leur traitement. À ce titre, voici donc les recommandations de formatage des CSV pour la publication des données de subventions :

- l'encodage est [UTF-8](https://fr.wikipedia.org/wiki/UTF-8)
- **le séparateur de colonne est la virgule**
- les champs qui contiennent une virgule doivent être entourés de guillemets (`"`)
- **le séparateur des nombres décimaux est le point** (1225 euros et 55 centimes => 1225.55), car c'est, de loin, la forme de nombre la mieux supportée par les outils de traitement des données
- si un champ contient des guillemets, celles-ci doivent
  - soit être doublées (`Redimensionnement d'une conduite (""pipeline"")`)
  - soit être échappées (`Redimensionnement d'une conduite (\"pipeline\")`)
- chaque ligne doivent avoir le même nombre de champs
- certains champs peuvent être laissés vides
- le type MIME ou *Content-Type* est `text/csv`

## Commentaires, suggestions

TODO Lien vers un sujet dédié sur le forum Etalab
