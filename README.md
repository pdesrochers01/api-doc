# Lignes directrices des API du gouvernement du Québec

# Table des matières

1. [Préface](#préface)
    1. [Introduction](#introduction)
    1. [Audience](#audience)
    1. [Sémantique et formatage du document](#document)
    1. [Contacts](#contact)
1. [Pour bien démarrer](#démarrer)
    1. [Pourquoi une norme de conception des API?](#norme)
    1. [Comment appliquer cette norme de conception?](#appliquer)
    1. [Pourquoi choisir REST?](#rest)
    1. [La norme de spécification OpenAPI](#openapi)
1. [Définitions](#définition)
    1. [API](#api)
    1. [REST](#rest)
    1. [Ressources](#resssources)
    1. [Identifiants de ressource](#idresssources)
    1. [Représentations](#représentation)
    1. [Espace de noms](#namespace)
    1. [Opérations](#opérations)
1. [Exigences gouvernementale des API](#exigences)
    1. [Documentation des API](#documentation)
    1. [Développement des API](#développement)
    1. [L'expérience du développeur](#expérience)
    1. [Stabilité des API](#stabilité)
    1. [Maturité de la conception des API](#maturité)
1. [Sécurité des API](#sécuritéapi)
    1. [Conception des API](#conceptionsecurité)
    1. [Sécurité des transports](#sécuritétransports)
    1. [Authentification et autorisation](#authentificationautorisation)
    1. [Limitation du débit](#limitationdébit)
    1. [Gestion des erreurs](#gestionerreurs)
    1. [Journaux d'audit](#journauxaudit)
    1. [Validation des entrées](#validationentrées)
    1. [Validation du type de contenu](#validationtypecontenu)
    1. [Utiliser les fonctions de sécurité de la passerelle d'API](#fonctionspasserelle)
1. [Conventions de nommage](#nommage)
    1. [Format des messages](#formatmessages)
    1. [Noms des composants URI](#composantesuri)
    1. [Noms des champs](#nomschamps)
    1. [Noms des relations des liens](#nomsliens)
    1. [En-têtes des requêtes](#entetedemandes)
    1. [Gestion des dates](#dates)
    1. [Exemples](#exemples)
1. [Versionnage des API](#nommage)
    1. [Gestion sémantique de version](#sémantiqueversion)
    1. [Version majeure](#versionmajeure)
    1. [Version mineure](#versionmineure)
    1. [Rétrocompatibilité](#rétrocompatibilité)
    1. [Politique de fin de vie](#findevie)
    1. [Désuétude des API](#désuétude)
1. [Requête des API](#requête)
    1. [Entête des requêtes](#entêterequêtes)
    1. [Méthodes des requêtes HTTP](#méthodeshttp)
    1. [Formats du contenu des requêtes (Request Payload Formats)](#formatréponses)
    1. [Idempotence](#idempotence)
    1. [Paramètres de requête](#paramètresrequête)
    1. [Pagination](#paramètresrequête)
    1. [Filtrage et tri](#Filtragetri)
1. [Réponses des API](#réponses)
    1. [Entête des réponses](#entêteréponses)
    1. [Codes de réponse HTTP](#codesréponsehttp)
    1. [Contenu des réponses](#contenuréponses)
1. [Hypermédia](#hypermédia)
    1. [Hypermédia - Données liées](#hypermédiadonnées)
    1. [HATEOAS](#hateoas)
    1. [API compatible hypermédia](#compatiblehypermédia)
    1. [Link Description Object](#linkobject)
    1. [Type de relation de lien](#typelien)
1. [Outils de test](#outilstest)
1. [Références](#références)

# Préface <a name="préface"></a>

## Introduction <a name="introduction"></a>
Ce document décrit la norme de conception pour l'ensemble des interfaces de programmation d'application (API) du Québec. Ce guide s'adresse à toute personne œuvrant au développement de services numériques pour une fonction publique, que ce soit dans le cadre de la fonction publique du Québec, d'une agence gouvernementale ou au-delà.

## Audience <a name="audience"></a>
Le public visé par ce document est constitué des développeurs d'API, des architectes de solutions et des analystes commerciaux.

## Document Semantics, Formatting, and Naming <a name="document"></a>
Les mots clés `DOIT`, `NE DOIT PAS`, `DEVRAIT`, `NE DEVRAIT PAS`, `RECOMMANDÉ`, `PEUT` et `OPTIONNEL` dans ce document doivent être interprétés comme décrit dans le standard [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

Les mots `REST` et `RESTful` **DOIVENT** être écrits tels que présentés ici, représentant l'acronyme comme toutes les lettres majuscules. Cela est également vrai pour `JSON`, `XML` et d'autres acronymes.

Le texte lisible par traitement informatique (machine-readable), comme les URL, les verbes HTTP et le code source, est représenté à l'aide d'une police de caractères à largeur fixe.


## Contact <a name="contact"></a>
TBC

# Pour bien démarrer <a name="démarrer"></a>

## Pourquoi une norme de conception des API? <a name="norme"></a>
Ce document peut servir de référence dans la phase de conception du développement d'un nouvel API.

Les normes de conception définies dans ce document sont à la fois indépendantes des données (ne tiennent pas compte du type de données consommées ou produites) et indépendantes du langage de programmation (ne tiennent pas compte du langage de programmation utilisé).

Ces normes de conception présentent des modèles communs qui sont applicables à tous les scénarios d'API sur la manière dont ceux-ci peuvent être conçus.

En tant que tel, ce document n'a pas besoin d'être appliqué de façon intégrale. Il peut être utilisé comme point de référence lors du développement d'une nouvelle API ou d'un scénario d'intégration.

## Comment appliquer cette norme de conception? <a name="appliquer"></a>
Savoir comment et quand appliquer les normes de conception d'API influencera considérablement la conception de solution d'API qu'un concepteur d'API prendra.

La détermination du moment d'utilisation de la norme est basée sur la catégorie d'API et le niveau d'abstraction requis.

Une API appartiendra généralement à l'une des catégories suivantes:

- **API de niveau système**: il s'agit d'API de bas niveau exposées directement par une application.

- **API de niveau processus**: il s'agit d'API composées d'autres API système via l'orchestration et la chorégraphie.

- **API de niveau d'expérience**: il s'agit d'API destinées à faciliter l'adoption de l'intégration d'API entre une organisation et ses consommateurs externes.

Si votre API fait partie de l'API de niveau système et est développée sur mesure, il est **RECOMMANDÉ** d'utiliser la norme de conception car cela vous aidera à développer des API de niveau processus ou expérience si elles sont requises à l'avenir.

Si votre API est une API de niveau de processus, vous **DEVRIEZ** appliquer la norme de conception car le plus souvent, une API de niveau de processus sera adaptée pour la réutilisation.

Si votre API est une API de niveau d'expérience, les normes de conception **DOIVENT** être appliquées.

Cette norme de conception elle-même ne s'applique PAS aux API tierces au niveau du système telles que celles disponibles en tant que «prêtes à l'emploi» ou faisant partie de la plate-forme SaaS, par exemple. API Salesforce ou API ArcGIS. Cependant, la norme peut s'appliquer si vous cherchez à réexposer ces API en tant qu'API de niveau d'expérience pour une consommation plus large.

## Pourquoi choisir REST? <a name="rest"></a>

Cette norme de conception d'API se concentre largement sur l'utilisation des API HTTP REST (Representational State Transfer) comme base de conception.

Alors qu'il existe des normes et des modèles de conception émergents pour les API (y compris GraphQL et gRPC), les développeurs du monde entier ont largement accepté REST comme mécanisme de facto de représentation et de transfert de données vers et depuis des systèmes sur Internet.

REST fonctionne bien lors de la modélisation des systèmes et des données. Les principes peuvent être appliqués aux systèmes qui sont à la fois grands et petits et les outils disponibles pour les développeurs prennent largement en charge l'accès aux données prêt à l'emploi.

Les API REST ne sont généralement pas adaptées à la diffusion de données (websockets) et ne sont pas non plus la meilleure utilisation pour les API largement basées sur des fonctions (gRPC / JSON-RPC). GraphQL est une alternative qui gère ces aspects du développement d'une manière différente et a été considérée comme une option pour les standards WoG.

Étant donné que les outils pour les API REST sont plus largement disponibles et que les connaissances technologiques générales du gouvernement sont déjà raisonnablement familiarisées avec les API REST et les principes de conception, il a été déterminé que REST serait la base de la modélisation de cette norme de conception d'API pour une utilisation dans le gouvernement australien .

Il est prévu que le développement futur de ces normes de conception prendra également en compte GraphQL et gRPC / JSON-RPC.


## La norme de spécification OpenAPI <a name="openapi"></a>
[OpenAPI v3.0](https://swagger.io/specification/)

# Définitions <a name="définition"></a>



## API <a name="api"></a>
Dans le contexte de cette norme de conception d'API, une API (Application Programming Interface) est définie comme une API RESTful.

Une API RESTful est un style de communication entre les systèmes où les ressources sont définies par URI et ses opérations sont définies par l'utilisation de méthodes HTTP.

Une API RESTful adopte de nombreuses normes HTTP telles que les codes de réponse et les méthodes.

## REST <a name="rest"></a>
REST (representational state transfer) est un style d'architecture logicielle définissant un ensemble de contraintes à utiliser pour créer des services web. Les services web conformes au style d'architecture REST, aussi appelés services web RESTful, établissent une interopérabilité entre les ordinateurs sur Internet. Les services web REST permettent aux systèmes effectuant des requêtes de manipuler des ressources web via leurs représentations textuelles à travers un ensemble d'opérations uniformes et prédéfinies sans état.

## Ressources <a name="resssources"></a>
Afin de concevoir une API utile et propre, votre système doit être divisé en groupements logiques (souvent appelés modèles ou ressources). Dans la plupart des cas, les ressources sont les «noms» de votre système.

Dans l'exemple d'un système RH, les ressources sont les `salariés`, les `postes` et les `demandes de congé`.

En décomposant les systèmes en ces zones logiques, cela permet une séparation nette des préoccupations (par exemple, les fonctions des employés ne fonctionnent que sur les employés et seuls les employés peuvent demander des demandes de congé). Cela garantit également que chaque élément de données renvoyé par votre API est le plus petit nécessaire pour répondre aux exigences du client (par exemple, lorsque vous demandez des employés, vous ne recevez pas non plus les membres de la famille).

## Identifiants de ressource <a name="idresssources"></a>
Chaque ressource disponible dans votre système (par exemple, chaque `employé` ou chaque `demande de congé`) doit être identifiable de manière unique dans le système. Ceci est un élément clé du style RESTful des API; la possibilité d'adresser individuellement tout élément de votre système et de stocker ces identifiants pour une utilisation ultérieure.

Les identificateurs de ressources peuvent être l'un des suivants:
| Nom                       | Exemple                        |
| ------------------------- |--------------------------------|
| Numérique                 | /employes/12389                |
| Chaîne de caractères      | /employes/john-smith           |
| Date                      | /dates/2018-09-17              |
| GUID                      | 7d80-eb69-4lq5-9f95            |

Tant que l'identifiant est unique dans votre application, il peut s'agir de n'importe quelle chaîne de caractères ou de chiffres.

Lorsque des identifiants numériques sont utilisés, ils **NE DOIVENT** pas être séquentiels. Par exemple. il ne devrait pas être évident de deviner le prochain identificateur.

## Représentations <a name="représentation"></a>
Un concept clé dans la conception d'API RESTful est l'idée de la représentation d'une ressource à un moment donné.

Lorsque vous demandez au système des informations sur les employés, vous recevrez une représentation de cet employé, par exemple :

```
HTTP 1.1 GET /employees/john-smith
Accept: application/json

200 OK
Content-Type: application/json

{
  "name" : "John Smith",
  "employee_id" : "123456",
  "position" : "Manager",
  "on_leave" : false
}
```
L'intention est que cette représentation peut changer au fil du temps à mesure que le système et les données changent. Un futur appel à ce même point d'extrémité peut produire une représentation différente si l'employé est maintenant en congé ou si son poste au sein de l'organisation a changé.

Il est également possible de demander une représentation entièrement différente de cette même ressource si le système la prend en charge. Par exemple, il peut y avoir un cas où vous avez besoin d'une version PDF de cet employé et cela peut être facilité par une demande de représentation différente via l'en-tête `Accept`:

```
HTTP 1.1 GET /employees/john-smith
Accept: application/pdf

200 OK
Content-Type: application/pdf

...<BINARY CODE>...
```
## Espace de noms <a name="namespace"></a>
L'espace de noms d'un service définit le regroupement des fonctions associées dans. Les espaces de noms peuvent être de niveau assez élevé (par exemple, le nom d'une agence ou d'un service) ou assez bas (par exemple, un projet, une équipe ou un service exposé).

Les espaces de noms sont utiles pour fournir aux consommateurs l'accès aux fonctions associées via des technologies de passerelle. Une fois qu'un consommateur a un jeton d'accès à un espace de noms particulier, il est probable (mais pas toujours obligatoire) qu'il aura accès à toutes les fonctions fournies dans cet espace de noms.

Les espaces de noms sont pertinents lors de la conception de la structure de votre URL et sont décrits plus en détail dans la section Noms des composants URI.

## Opérations <a name="opérations"></a>

Pour utiliser l'un des espaces de noms, ressources et identificateurs de ressources, les développeurs doivent utiliser Operations.

Une opération est définie par l'utilisation de:

une méthode HTTP; et
un chemin de ressource.
Exemples:
```
GET /employes
GET /employes/john-smith
DELETE /employes/john-smith
```

# Exigences gouvernementale des API <a name="exigences"></a>


## Documentation des API <a name="documentation"></a>
Des API bien documentées sont un élément essentiel d'un programme d'API réussi. Cela facilite l'interopérabilité des services grâce à un document descriptif commun. Dans la mesure du possible, la structure, les méthodes, les conventions de dénomination et les réponses seront également normalisées pour garantir une expérience commune aux développeurs qui accèdent aux services d'une gamme d'éditeurs.

Toutes les API créées pour le gouvernement australien **DOIVENT** spécifier un document OpenAPI v2.0 valide car il a le support le plus large. Un document OpenAPI v3.0 **PEUT** également être fourni pour assurer la pérennité de l'API.

Le document de description de l'API est **RECOMMANDÉ** de contenir les sections suivantes:

- À propos de
- Mentions légales
- Conditions d'utilisation
- Licence
- Classification des données
- droits d'auteur
- Attribution
- Exigences d'authentification
- Modèle de données
- Nous contacter

## Développement des API <a name="développement"></a>
Il est **RECOMMANDÉ** de suivre les directives suivantes lors du développement d'API:

- Les documents de description de l'API **DEVRAIENT** contenir la documentation de l'API (informations et descriptions de haut niveau) et la version contrôlée en conséquence.

- Ils **DOIVENT** être considérés comme des contrats techniques entre concepteurs et développeurs et entre consommateurs et fournisseurs.

- Les API simulées **DEVRAIENT** être créées en utilisant la description de l'API pour permettre une intégration précoce du code pour le développement.

- Le comportement et l'intention de l'API **DEVRAIENT** être décrits avec autant d'informations que possible.

- La documentation **DEVRAIT** être publique dans la mesure du possible et facilement accessible à ceux qui en ont besoin.

- Les descriptions **DEVRAIENT** contenir un exemple de demande et de réponses.

- Des exemples de corps de demande et de réponse **DEVRAIENT** être fournis dans leur intégralité.

- Les codes de réponse et les messages d'erreur attendus **DEVRAIENT** être fournis dans leur intégralité.

- Des codes de réponse corrects **DEVRAIENT** être utilisés (voir Codes de réponse HTTP).

- Les problèmes ou limitations connus **DEVRAIENT** être clairement documentés.

- Les performances attendues, la disponibilité et le SLA / OLA **DEVRAIENT** être clairement documentés.

- S'il est connu, une chronologie dans laquelle les méthodes seront obsolètes **DEVRAIT** être fournie. (Voir la section Politique de fin de vie et dépréciation de l'API).

- Toute la documentation de l'API **DEVRAIT** être imprimable ou exportable.

Tous les documents OpenAPI **DEVRAIENT** être fournis au format JSON.

Afin de suivre les recommandations de versionnage de cette norme, il **DOIT** y avoir une description OpenAPI par version principale.

Par exemple; si votre produit API expose et gère 3 versions principales de son API REST, vous devez fournir 3 descriptions OpenAPI (une pour chaque version: v1, v2 et v3).

## L'expérience du développeur <a name="expérience"></a>
Une API difficile à utiliser réduit la probabilité que les consommateurs continuent à l'utiliser et cherchent donc des alternatives. Il est également peu probable qu'ils recommandent l'API à d'autres.

Les API en cours de conception sont **RECOMMANDÉES** pour être testées avec de vrais consommateurs. Tout commentaire fourni **DEVRAIT** être pris en compte pour être intégré à l'API afin d'assurer le meilleur résultat possible.

L'équipe API WoG fournit un processus d'examen des API pour garantir que les API répondent à un niveau de base d'utilisation avant qu'elles ne soient publiées aux consommateurs potentiels pour commentaires.


## Stabilité des API <a name="stabilité"></a>
Les API **DOIVENT** être conçues en gardant à l'esprit la compatibilité ascendante, car lorsque des modifications sont introduites, il est peu probable que les consommateurs les introduisent immédiatement dans leurs applications.

Dans la plupart des cas, l'introduction de nouveaux champs dans une API ou l'ajout de nouveaux points de terminaison est un changement sans rupture et peut être introduit avec une mise à jour de version de correctif.

Si le contrat API doit changer d'une manière qui rompt le contrat des consommateurs, cela **DEVRAIT** être communiqué clairement.

1. Les propriétaires de produits API **DEVRAIENT** documenter la durée de vie du support pour les services API (par exemple, combien de temps ils seront pris en charge).
1. Les nouvelles fonctionnalités **DOIVENT** être introduites de manière à ne pas affecter les consommateurs existants.
1. Toutes les activités de dépréciation **DOIVENT** être connues des consommateurs avant leur mise en œuvre.

## Maturité de la conception des API <a name="maturité"></a>
Lors de la conception d'une nouvelle API, l'une des principales considérations est l'expérience du développeur utilisant cette API. Les développeurs auront une bien meilleure expérience s'ils comprennent déjà les concepts de base de l'API.

Le concept le plus courant pour les développeurs d'API est le style architectural des API RESTful.

Afin d'aider les concepteurs d'API à s'assurer que leurs API sont conçues de manière REST, le modèle de maturité Richardson a été développé en tant qu'outil.

Ce modèle décompose toutes les API RESTful en l'un des 4 niveaux différents en fonction de leur utilisation des URI, des méthodes HTTP et des HATEOAS:

Niveau 0 - État de base pour toute nouvelle API.
Niveau 1 - L'API implémente différents URI, mais un seul verbe (par exemple POST)
Niveau 2 - L'API implémente différents URI et plusieurs verbes (par exemple CRUD via GET / POST / PUT / DELETE).
Niveau 3 - L'API implémente différents URI, plusieurs verbes et HATEOAS pour représenter les relations entre les objets.
Toutes les API adhérant à cette norme **DOIVENT** être conçues au niveau 2 du modèle de maturité Richardson.

Une API **PEUT** choisir de mettre en œuvre le niveau 3 du modèle, mais ce n'est pas obligatoire pour cette norme.

# Sécurité des API <a name="sécuritéapi"></a>

## Conception des API <a name="conceptionsecurité"></a>
TBD

## Sécurité des transports <a name="sécuritétransports"></a>
TBD

## Authentification et autorisation <a name="authentificationautorisation"></a>
TBD

## Limitation du débit <a name="limitationdébit"></a>
TBC
## Gestion des erreurs <a name="gestionerreurs"></a>
TBD

## Journaux d'audit <a name="journauxaudit"></a>
TBD

## Validation des entrées <a name="validationentrées"></a>
TBC

## Validation du type de contenu <a name="validationtypecontenu"></a>
TBD

## Utiliser les fonctions de sécurité de la passerelle d'API <a name="fonctionspasserelle"></a>
TBC

# Conventions de nommage <a name="nommage"></a>

## Format des messages <a name="formatmessages"></a>
Pour les noms de corps de requête et de réponse et de paramètre de requête, le format de message est **RECOMMANDÉ** d'utiliser snake-case:

Exemple:
```
// this_is_snake_case

{
  "employee_id" : "AB1837"
}
```
Si le cas de serpent n'est pas disponible, il est alors **RECOMMANDÉ** d'utiliser le cas de chameau.

Exemple:
```
// thisIsCamelCase

{
  "employeeId" : "AB1837"
}
```
Dans les deux cas, l'objet et la définition de champ doivent être identiques pour le corps de la demande et de la réponse et les paramètres de la requête.

## Noms des composants URI <a name="composantesuri"></a>
Les URI suivent la spécification RFC 3986. Cette spécification simplifie le développement et la consommation de services d'API REST.

Les directives de cette section régissent la structure et la sémantique de votre URI conformément aux contraintes de la RFC 3986.

**Composants URI**
La structure des URL utilisées dans les API **DEVRAIT** être significative pour les consommateurs. Les URL **DEVRAIENT** suivre une structure hiérarchique prévisible pour améliorer la compréhensibilité et donc la convivialité.

Les URL **DOIVENT** suivre la convention de dénomination standard décrite ci-dessous:
```
https://api.quebec.ca/namespace/project-name/v1/collection?attributes=first_name,last_name
\___/   \___________/\___________________________________/\______________________________/
  |           |                        |                                  |
schéma     autorité                 chemin                              requête
```
**Longueur maximale de l'URI**
L'URI total, y compris le chemin et la requête, **NE DOIT PAS** dépasser 2000 caractères, y compris les codes de mise en forme tels que les virgules, les traits de soulignement, les points d'interrogation, les tirets, les plus ou les barres obliques.

**Conventions de dénomination des URI**
Les URL **DOIVENT** suivre la convention de dénomination standard décrite ci-dessous:

- l'URI DOIT être spécifié en minuscules
- seuls les tirets «-» peuvent être utilisés pour séparer les mots ou les paramètres de chemin pour la lisibilité (pas d'espaces ni de traits de soulignement)
- seuls les traits de soulignement peuvent être utilisés pour séparer les mots dans les noms de paramètres de requête, mais pas dans le cadre de l'URI de base

Le tableau suivant explique comment construire l'URI de l'API.

| Élément de l'URI                        | Description                                                  | Exemple                                                      |
| --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Protocol                                | Toutes les API **DOIVENT** être exposées en utilisant HTTPS. | `https://`                                                   |
| Autorité > Environment                  | Le domaine dans lequel le point de terminaison de l'API sera exposé. Reportez-vous à la section standard «DNS» pour plus de détails. | `api.quebec.ca`                                              |
| Chemin > API                            | Le nom d'API dérivé du domaine métier.                       | e.g. `/namespace/project-name` Toute service peut spécifier le nom de l'API sur lequel elle souhaite exposer ses services. |
| Chemin > Version                        | La version de l'API à laquelle le consommateur souhaite accéder. | e.g. `/v1` Toutes les API doivent spécifier une version qui suit le schéma de gestion des versions tel que spécifié dans la section «gestion des versions» ci-dessous. |
| Chemin > Collection                     | La collection identifie une liste de ressources. La collection **DOIT** être nommée en utilisant la représentation plurielle d'un nom. | par exemple, dans le cadre de l'API de la main-d'œuvre - une ressource peut être une liste de `employees`. |
| Chemin > Resource                       | L'identifiant de ressource qui correspond à une instance de la ressource. | par exemple, dans le cadre de l'API project-name, s'il y avait un employé spécifique avec l'ID E13454. Ces détails peuvent être récupérés en utilisant `GET` `/project-name/v1/employees/E13454` |
| Chaîne de requête> Paramètres / Filtres | Les paramètres de requête **NE DOIVENT** PAS être utilisés pour transporter des données utiles ou des données réelles. Les paramètres de requête suivants **DEVRAIENT** être pris en charge par votre API là où ils seraient utiles: **attributs** - spécifiez ou restreignez les attributs à renvoyer **filtres** - conditions pour restreindre / filtrer la liste de collection **sort** - spécifiez l'exigence de tri **page** - spécifiez l'index de pagination à renvoyer dans un ensemble de collections | e.g. `attributes=first_name,last_name` retourne un élément de données ne contenant que `first_name` et `last_name` attributes`filters=creation_date => 2001-09-20T13:00:00 et creation_date <= 2001-09-21T13:00:00 et first_name like 'fred' et post_code=3000` - retourne une collection de ressources avec uen date de création entre 2001-09-20 1pm et 2001-09-21 1pm et first-name comme 'fred' et post_code is 3000. |

**Noms des ressources**
Les concepteurs d'API **DOIVENT** suivre ces principes lors de la création d'une API REST:

Les noms **DOIVENT** être utilisés - pas les verbes pour les noms.
Les noms de ressources **DOIVENT** être au pluriel. Lorsque le pluriel d'une ressource n'est pas standard, comme feuille ou poisson, choisissez un nom plus approprié ou utilisez le pluriel approprié - feuilles, poissons.
Les noms de ressources **DOIVENT** être en minuscules et n'utiliser que des caractères alphabétiques et des traits d'union.
Le caractère trait d'union, (-), **DOIT** être utilisé comme séparateur de mot dans les paramètres de chemin URI.
Notez que c'est le seul endroit où les tirets sont utilisés comme séparateur de mots. Dans presque toutes les autres situations, le caractère de soulignement DOIT être utilisé.

Bons exemples:
```
/employees
/customers
/products
```
Mauvais exemples :
```
/get-employee
/customer
/add-product
```
**Noms des paramètres de requête**
- Les littéraux / expressions dans les chaînes de requête **DEVRAIENT** être séparés en utilisant un trait de soulignement.
- Les valeurs des paramètres de requête **DOIVENT** être codées en pourcentage.
- Les paramètres de requête **DOIVENT** commencer par une lettre et **DEVRAIENT** tous être en minuscules. Seuls les caractères alphabétiques, les chiffres et le caractère de soulignement **DOIVENT** être utilisés.
- Les paramètres de requête **DEVRAIENT** être facultatifs.
- Les paramètres de requête **NE DEVRAIENT** pas contenir de caractères qui ne sont pas sûrs pour les URL.


## Noms des champs <a name="nomschamps"></a>
Le modèle de données pour la représentation *DOIT** être conforme à la spécification `JSON`.

Les valeurs peuvent elles-mêmes être des objets, des chaînes, des nombres, des booléens ou des tableaux d'objets.

- Les noms de clé DOIVENT être des mots en minuscules, séparés par un caractère de soulignement.
   - `foo`
   - `bar_baz`
- Un préfixe tel que `is_` ou` has_` **NE DEVRAIT PAS** être utilisé pour les clés de type booléen.
- Les champs qui représentent des tableaux **DEVRAIENT** être nommés en utilisant des noms au pluriel (par exemple authentificateurs - contient un ou plusieurs authentificateurs, produits - contient un ou plusieurs produits).

## Noms des relations des liens <a name="nomsliens"></a>
Pour aider à guider les utilisateurs à travers vos liens relationnels API **DOIVENT** être fournis. Ces liens agissent comme la navigation de votre API informant les utilisateurs de l'endroit où ils peuvent aller ensuite.

Un tableau `_links` **DOIT** être fourni pour les ressources. Il contient des objets lien qui peuvent faire référence à des ressources associées dans le système.

Une relation de lien **DOIT** contenir les éléments suivants:

- href - chaîne contenant l'URL absolue ou relative de la ressource
- rel - la chaîne textuelle décrivant ce qu'est cette entité
- méthode - la méthode HTTP qui **DEVRAIT** être utilisée lors de l'utilisation de cette ressource associée.

Exemple:
```
"_links": [
    {
        "href": "/v1/customer/partner-referrals/ALT-JFWXHGUV7VI",
        "rel": "_self",
        "method": "GET"
    }
]
```

## En-têtes des requêtes <a name="entetedemandes"></a>
Les en-têtes suivants **DEVRAIENT** être utilisés par défaut sur toutes les demandes.

| Header | Default Value      |
| ------ | ------------------ |
| Accept | `application/json` |



## Gestion des dates <a name="dates"></a>
### Dates formatées ISO8601

Toutes les implémentations utilisant des dates **DOIVENT** être conformes au format [ISO 8601] (https://en.wikipedia.org/wiki/ISO_8601).

Dans ISO 8601, les valeurs de date et d'heure sont classées de la plus grande à la plus petite unité de temps: année, mois (ou semaine), jour, heure, minute, seconde et fraction de seconde.

La manière internationalement reconnue de représenter un objet de date est:

```
YYYY-MM-DD
```

La manière internationalement reconnue de représenter un objet temporel est:

```
hh:mm:ss.fff
```

Les composants de ceux-ci sont décrits ci-dessous:

| Date Component | Description                                                  | Example |
| -------------- | ------------------------------------------------------------ | ------- |
| YYYY           | Année à quatre chiffres                                      | `2019`  |
| MM             | Mois à deux chiffres (avec zéro non significatif)            | `04`    |
| DD             | Jour à deux chiffres (avec zéro non significatif)            | `27`    |
| T              | Définir le caractère indiquant le début de l'élément temporel dans un format datetime combiné | `T`     |
| hh             | Deux chiffres d'une heure (00 à 23)                          | `18`    |
| mm             | Deux chiffres d'une minute                                   | `38`    |
| ss             | Deux chiffres de seconde                                     | `12`    |
| fff            | Trois chiffres d'une milliseconde (000 à 999)                | `123`   |

Lorsqu'il est combiné dans un datetime, l'objet peut être représenté comme suit:

```
2019-10-02T18:36:12.123
```

### Utilisation des fuseaux horaires

Lors de l'utilisation du format ISO 8601, le fuseau horaire est **RECOMMANDÉ** à fournir. Cela est dû aux complexités qui surviennent lors de la consommation de données et de la conversion à l'heure locale.

Le fuseau horaire peut être représenté dans une variété de mécanismes, mais il est le plus souvent représenté comme un décalage par rapport à GMT + 0 (ou à l'heure zouloue).

`2019-10-02T18:36:12.123Z` (avec «z» indiquant l'heure zoulou).

La convention ici est la suivante:

| Composant de date | Description                                                  | Exemple |
| ----------------- | ------------------------------------------------------------ | ------- |
| Z                 | Indique l'heure zouloue (facultatif)                         | `Z`     |
| + \| -            | + représente un décalage positif par rapport à Zulu (par exemple devant Zulu). - représente un décalage négatif par rapport à Zulu (par exemple, derrière Zulu) | `+      |
| hh                | Deux chiffres d'une heure (00 à 13)                          | `10`    |
| mm                | Deux chiffres d'une minute (généralement 00, 15, 30 ou 45)   | `30`    |

pour représenter l'heure normale de l'Est du Québec (-5), le format suivant serait utilisé:

```
2019-10-02T18:36:12.123+10:00
```

### Conventions de dénomination des champs de date

Lorsque vous utilisez des champs de date, les conventions de dénomination suivantes pour ces champs doivent être utilisées:

- Pour les propriétés nécessitant à la fois la date et l'heure, les services **DOIVENT** utiliser le suffixe `datetime`, par exemple `start_datetime`.
- Pour les propriétés ne nécessitant que des informations de date sans spécifier l'heure, les services **DOIVENT** utiliser le suffixe `date`, par exemple `date_naissance`.
- Pour les propriétés ne nécessitant que des informations d'heure sans spécifier de date, les services **DOIVENT** utiliser le suffixe `time`, par ex. `rendez-vous_start_time`.



## Exemples <a name="exemples"></a>
### Bons exemples d'URL

List des employés :
`GET https://gw.api.gov.au/e09284/v1/employees`

Requête avec filtre :
`GET https://gw.api.gov.au/e09284/v1/employees?year=2011&sort=desc`
`GET https://gw.api.gov.au/e09284/v1/employees?section=economy&year=2011`

Un seul employé au format JSON :
`GET https://gw.api.gov.au/e09284/v1/employees/1234`

Tous les endroits où cet employé travaille :
`GET https://gw.api.gov.au/e09284/v1/employees/1234/locations`

Spécifiez les champs facultatifs dans une liste séparée par des virgules :
`GET https://gw.api.gov.au/e09284/v1/employees/1234?fields=job_title,start_date`

Ajouter un nouvel emplacement à un employé en particulier :
`POST https://gw.api.gov.au/e09284/v1/employees/1234/locations`

### Exemples d'URL incorrectes

Point de terminaison non pluriel:

`GET` https://gw.api.vic.gov.au/e09284/v1/employee
`GET` https://gw.api.vic.gov.au/e09284/v1/employee/1234
`GET` https://gw.api.vic.gov.au/e09284/v1/employee/1234/location

`GET` https://gw.api.gov.au/e09284/v1/employee
`GET` https://gw.api.gov.au/e09284/v1/employee/1234
`GET` https://gw.api.gov.au/e09284/v1/employee/1234/location

Verbe dans l'URL:
`POST` https://gw.api.gov.au/e09284/v1/employee/1234/create

Filtrage à l'extérieur dans l'URL au lieu de la chaîne de requête
`GET` https://gw.api.gov.au/e09284/v1/employee/1234/desc


*******
# Titre1 <a name="titre1"></a>

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

*******
# Titre1 <a name="titre1"></a>

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

*******
# Titre1 <a name="titre1"></a>

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

*******
# Titre1 <a name="titre1"></a>

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

*******
# Titre1 <a name="titre1"></a>

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC

## Titre2 <a name="titre2"></a>
TBD

## Titre2 <a name="titre2"></a>
TBC
