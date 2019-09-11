Format réglementaire pour la publication des données essentielles des conventions de subventions
===================================================================================================

### Version 1.1.1

Ce dépôt est destiné à accueillir :

- la documentation du format
- des exemples de données

Vous pouvez poser vos questions et transmettre vos commentaires via [le sujet dédié sur le forum d'Etalab](https://forum.etalab.gouv.fr/t/cadre-juridique-et-technique-de-louverture-des-donnees-de-subventions/4004).

# Cadre réglementaire

Ce format a été conçu parallèlement et conformément aux textes suivants :

- [Article 8 de la loi n° 2010-237 du 9 mars 2010 de finances rectificative pour 2010](https://www.legifrance.gouv.fr/affichTexteArticle.do?cidTexte=JORFTEXT000021943745&idArticle=JORFARTI000021943775&categorieLien=cid)
-  [Décret du 9 mai 2017 relatif aux possibilités de cumuler des aides à l'investissement pour la construction, l'acquisition et l'amélioration de logements locatifs aidés, avec les subventions versées au titre de certaines actions du programme d'investissements d'avenir](https://www.legifrance.gouv.fr/eli/decret/2017/5/9/LHAL1705272D/jo)
- [L'arrêté du 17 novembre 2017 relatif aux conditions de mises à disposition des données essentielles des conventions de subvention](https://www.legifrance.gouv.fr/affichTexte.do?cidTexte=JORFTEXT000036040528&categorieLien=id)


L'objectif est de normaliser la publication des données des conventions de subventions afin de les regrouper, de les rendre publiques, et de permettre à tout un chacun d'appliquer des traitements automatiques :

- analyses
- visualisation de données

Depuis le 1er mars 2017, l'ensemble des attribuants de subventions, à l'exception des collectivités de moins de 3 500 habitants, sont tenus de publier ces données dans le format décrit sur ce dépôt. D'après l'article 2 du décret, ces données doivent être publiées :

- dans les 3 mois suivants la signature de la convention de subvention
- *soit* sur le site Internet de la collectivité attribuante
- *soit* sur le portail interministériel des données ouvertes [data.gouv.fr](http://data.gouv.fr) (en y ajoutant le mot-clef "subvention")
    - dans ce cas, un lien vers le jeu de données doit être publié sur le site Internet de la collectivité attribuante

# Un format tabulaire

## Une structure des données simple

Le format CSV (*Comma Separated Values*, valeurs séparées par des virgules) a été choisi en raison de sa simplicité et de la facilité qu'ont les logiciels à le produire, y compris ceux qui datent du siècle dernier.

Ce format était en concurrence avec des formats plus modernes tels que XML ou JSON, mais la structure des données des subventions n'était pas suffisamment complexe pour justifier l'utilisation d'un format en arbre.

En effet, la structure décrite dans le décret se prête bien à la représentation sous forme de lignes et de colonnes. Voici la liste des colonnes, dans l'ordre :

- `nomAttribuant`
- `idAttribuant`
- `dateConvention`
- `referenceDecision`
- `nomBeneficiaire`
- `idBeneficiaire`
- `objet`
- `montant`
    - si c'est un nombre décimal, ne pas oublier d'utiliser le point comme signe de séparation : `77800.20`
- `nature`
    - si le versement est à la fois en numéraire et en nature, séparez les valeurs par un point-virgule : `aide en numéraire;aide en nature` (l'arrêté sera amendé prochainement)
- `conditionsVersement`
- `datesPeriodeVersement`
    - si versement unique et date du versement connu, date unique : `2017-09-20`
    - si versement échelonné ou date précise de versement unique inconnue, indiquer une période : `2017-12-14_2018-12-14`
- `idRAE`
- `notificationUE`
- `pourcentageSubvention`


La seule subtilité réside dans le besoin de créer **une ligne par bénéficiaire** et non une ligne par contrat. Ainsi, dans le cas d'une subvention attribuée à plusieurs bénéficiaires, toutes les données de la subvention sont répétées à l'identique sur autant de lignes qu'il y a de bénéficiaires, à l'exception des colonnes suivantes dont les valeurs sont adaptées :

- `nomBeneficiaire`
- `idBeneficiaire`
- `pourcentageSubvention`

Dans certains cas, les colonnes `conditionsVersement` et `datesPeriodeVersement` peuvent également varier d'un bénéficiaire à un autre, pour une même subvention.

## Recommandations de formatage

Le format CSV, contrairement au XML et au JSON, n'a pas une syntaxe très stricte. Afin de faciliter le regroupement et l'analyse des données, voici les règles de formatage imposées pour la publication des données essentielles des conventions de subventions :

- l'encodage est [UTF-8](https://fr.wikipedia.org/wiki/UTF-8)
- **le séparateur de colonne est la virgule**
- les champs qui contiennent une virgule doivent être entourés de guillemets doubles (`"`)
- **le séparateur des nombres décimaux est le point** (1225 euros et 55 centimes => 1225.55). C'est aujourd'hui contraire à la lettre actuelle de l'arrêté, mais celui-ci va être adapté en ce sens.
- si un champ contient des guillemets doubles, celles-ci doivent
  - soit être doublées (`Redimensionnement d'une conduite (""pipeline"")`)
  - soit être échappées (`Redimensionnement d'une conduite (\"pipeline\")`)
- chaque ligne doit avoir le même nombre de champs
- le type MIME ou *Content-Type* est `text/csv`

## Exemple

Le fichier [exemple.csv](https://github.com/etalab/format-subventions/blob/master/exemple.csv) ([contenu brut](https://raw.githubusercontent.com/etalab/format-subventions/master/exemple.csv)) est un exemple de bonne saisie des données dans le cas d'une subvention octroyée par la Région Bretagne à deux bénéficiaires, Rodriguez SA et Bellandier SAS.

## Commentaires, suggestions

Vous pouvez poser vos questions et transmettre vos commentaires via [le sujet dédié sur le forum d'Etalab](https://forum.etalab.gouv.fr/t/cadre-juridique-et-technique-de-louverture-des-donnees-de-subventions/4004).

## Notes de versions

### 1.1.1

- Correction dans exemple.csv du format des pourcentageSubvention, du format `80` (%) au format `0.8`, comme décrit dans l'annexe de [l'arrêté](https://www.legifrance.gouv.fr/affichTexte.do?cidTexte=JORFTEXT000036040528&dateTexte=&categorieLien=id) ([#3](https://github.com/etalab/format-subventions/issues/3))

### 1.1.0

- Pour des raisons de praticité, et contrairement à la lettre de l'arrêté qui va être amendée, la mission Etalab a décidé d'utiliser le point comme séparateur des décimales
- Pour les mêmes raisons, le séparateur du champ `nature` en cas de valeurs multiples est maintenant le point-virgule, et non plus la virgule

### 1.0.0

- Version initiale 26 février 2018
