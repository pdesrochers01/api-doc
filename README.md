# Lignes directrices des API du gouvernement du Québec

# Table des matières

1. [Préface](#préface)
    1. [Introduction](#introduction)
    1. [Audience](#audience)
    1. [Sémantique et formatage du document](#document)
    1. [Contacts](#contact)
    1. [Pourquoi une norme de conception des API?](#norme)
    1. [Comment appliquer cette norme de conception?](#appliquer)
    1. [Pourquoi le style d'architecture REST?](#rest)
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
1. [Versionnage des API](#versionnage)
    1. [Gestion sémantique de version](#sémantiqueversion)
    1. [Version majeure](#versionmajeure)
    1. [Version mineure](#versionmineure)
    1. [Rétrocompatibilité](#rétrocompatibilité)
    1. [Politique de fin de vie](#findevie)
    1. [Désuétude des API](#désuétude)
1. [Requête des API et paramètres](#requêteparamètres)
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

Ce document de normes API est un travail en cours d'élaboration (Work-in-progress). Pour y contribuer, veuillez faire une demande à l'adresse ci-dessous. L'équipe de gestion des API du gouvernement du Québec analysera la demande et décidera si ceux-ci peuvent être incorporer.

## Audience <a name="audience"></a>
Le public visé par ce document sont les développeurs d'API, les architectes d'entreprise et de solutions et les analystes organiques et d'affaire.

Ce document a été écrit dans le but d'être utilisé en interne au sein du gouvernement du Québec. Il est rendu public afin de permettre une adoption plus large par des individus ou des organisations partenaires qui souhaitent aussi créer ou consommer des API interopérables avec les standard du gouvernement du Québec.

## Document Semantics, Formatting, and Naming <a name="document"></a>
Les mots clés **DOIT**, **NE DOIT PAS**, **DEVRAIT**, **NE DEVRAIT PAS**, **RECOMMANDÉ**, **PEUT** et **OPTIONNEL** dans ce document doivent être interprétés tel que décrit dans le standard [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

Les acronymes sont écrits en majuscules. Par exemple,les mots **REST**, **JSON**, **XML** et tous les autres acronymes sont tous représentés de cette façon.

Les textes lisibles par traitement informatique (programme, script, etc.) tel que les **URL**, les verbes **HTTP** et les codes sources, sont représentés par la `mise en évidence (surbrillance) des blocs de code`.

## Contact <a name="contact"></a>
TBC (ex: apiteam@api.quebec.ca)

## Pourquoi une norme de conception des API? <a name="norme"></a>
La principale intention de ce document est de servir de référence dans la phase de conception du développement d'un nouvel API.

Les normes de conception définies dans ce document sont indépendantes des données (ne tiennent pas compte du type de données consommées ou produites) et des langages de programmation.

Ces normes de conception présentent des patrons communs de conception qui sont applicables à tous les scénarios d'API. Ces patrons de conceptions sont basées sur des standards reconnus par l'industrie.

## Comment appliquer ce cadre normatif? <a name="appliquer"></a>
Il est important de bien comprendre quand et comment appliquer ces normes de conception d'API. En effet, la catégorie à laquelle appartient l'API déterminera si vous devrez ou non utiliser ce cadre normatif.

Une API appartiendra généralement à l'une des catégories suivantes:

- **API de niveau système (application)**: il s'agit d'API de bas niveau exposées directement par une application.

- **API de niveau processus**: il s'agit d'API composées d'autres API systèmes soit via une orchestration et/ou une chorégraphie.

- **API de niveau d'intégration**: il s'agit d'API destinées à faciliter l'adoption de l'intégration d'API entre une organisation et ses consommateurs externes.

Si votre API fait partie de l'API de niveau système (application) et est développée sur mesure (maison), il est **RECOMMANDÉ** d'utiliser cette norme de conception car elle facilitera le développement futur des API de niveau processus ou d'intégration.

Si votre API est une API de niveau de processus, il est aussi **RECOMMANDÉ** d'appliquer cette norme de conception. En effet, les API de niveau de processus sont souvent réutilisées par différents consommateurs.

Si votre API est une API de niveau d'expérience, ces normes de conception **DOIVENT** être appliquées.

## Pourquoi le style d'architecture REST? <a name="rest"></a>

Cette norme de conception d'API du gouvernement du Québec est basée sur le style d'architecture **REST** (Representational State Transfer).

Bien qu'il existe d'autres modèles de conception pour les API (par exemple, **GraphQL** et **gRPC**), la grande majorité des développeurs de l'industrie des TI ont largement adopté **REST** comme mécanisme de représentation et de transfert de données entre les différentes applications visibles sur Internet. Les gouvernements de l'Ontarion, du Canada, du Royaume-Uni, de l'Australie et de nombreux autres ont déjà adoptés le style d'architecture REST. Ce standard a aussi été adopté par la grande majorité des fournisseurs de services infonuagiques (Azure, AWS, Google Cloud, DigitalOcean, etc.).

Le style d'architecture **REST** est très efficace lorsque nous devons modéliser des systèmes et des données. Les principes peuvent être aussi bien appliqués aux systèmes de petite ou de grande envergure et les outils disponibles (écosystème de développement) supportent facilement les accès aux données.

Il est important de prendre note que les **API REST** ne sont généralement pas adaptées à la diffusion (streaming) de données ou dans les cas où la performance des interaction est essentielle. **GraphQL** et **gRPC/JSON-RPC** sont des alternatives en émergence qui seront éventuellement considérées comme des options pour les standards du gouvernement du Québec.

Finalement, étant donné que les outils pour les **API REST** sont largement disponibles et que les développeurs sont déjà familiées avec ceux-ci (autant en Amérique du Nord qu'ailleurs dans le monde), il a été déterminé que le style d'architecture **REST** serait la base de la modélisation des API dans le gouvernement du Québec.

## La norme de spécification OpenAPI <a name="openapi"></a>
**TBC** [OpenAPI v3.0](https://swagger.io/specification/)

# Définitions <a name="définition"></a>

## API <a name="api"></a>
En informatique, une interface de programmation d’application (en anglais API pour *Application Programming Interface*) est un ensemble normalisé de classes, de méthodes, de fonctions et de constantes qui sert de façade par laquelle une application offre des services à d'autres applications. Elle est offerte par une bibliothèque logicielle ou un service web, le plus souvent accompagnée d'une description qui spécifie comment des programmes consommateurs peuvent se servir des fonctionnalités du programme fournisseur.

Dans le contexte de cette norme de conception d'API, une API est définie comme un service Web.
![alt text](https://github.com/pdesrochers01/api-doc/blob/main/API-Web.png "API de type service Web")

## REST <a name="rest"></a>
REST (*REpresentational State Transfer*) est un style d'architecture logicielle définissant un ensemble de contraintes à utiliser pour créer des services web.

Les services web conformes au style d'architecture REST, aussi appelés services web RESTful, établissent une interopérabilité entre les ordinateurs sur Internet.

Les services web REST permettent aux systèmes effectuant des requêtes de manipuler des ressources web via leurs représentations textuelles à travers un ensemble d'opérations uniformes et prédéfinies sans état.

Essentiellement, ce style d’architecture vise à utiliser tous les standards du Web afin de développer des applications distribuées. Il a été développé par Thomas Fielding dans sa thèse de doctorat (Université de Californie, 2000).

Les principaux bénéfices de l’architecture REST est d’exploiter les caractéristiques du Web et de son principale protocole (HTTP) sont :
- Réutilisation des verbes d’actions standards pour interagir avec des stockages persistants (GET pour lire, POST pour créer une nouvelle ressource, PUT pour mettre à jour, DELETE pour effacer, ETC.)
- Réutilisation des mécanismes de cache (fureteur, proxy, passerelle, serveur Web, etc.)
- Réutilisation des mécanismes de redirection et de transfert
- Réutilisation des mécanismes de sécurité (cryptage, authentification, etc.)

Toutes ces caractéristiques sont des facteurs essentiels afin de créer des services résilients. Un autre avantage important réside dans le fait que le Web est construit à partir de multitude de spécifications succinctes permettant une évolution plus facile et éviter ainsi d’interminables polémiques sur la sélection des normes.

## Ressources <a name="resssources"></a>
Afin de concevoir une API facilement utilisable, les applications doit être divisées en groupes logiques (souvent appelés modèles ou ressources).

Dans l'exemple d'un système de gestion de ressources humaines (RH), les **ressources** sont les `employés`, les `postes` et les `demandes de congés`.

La décomposition des systèmes en groupes logiques permet une séparation des préoccupations (*"separation of concerns"*). Cela garantit également que chaque élément de données retourné par l'API sera minimaliste afin répondre aux exigences du client.

## Identifiants de ressource <a name="idresssources"></a>
Toutes les ressources disponibles dans un système (par exemple, chaque `employé` ou chaque `demande de congé`) doit être identifiable de manière unique. Ceci est un élément clé d'un API RESTful, soit la possibilité d'adresser individuellement tout élément d'un système.

Voici quelques exemples d'identificateurs de ressources :
| Nom                       | Exemple                        |
| ------------------------- |--------------------------------|
| Numérique                 | /employes/12389                |
| Chaîne de caractères      | /employes/marie-tremblay       |
| Date                      | /dates/2018-09-17              |
| GUID                      | 7d80-eb69-4lq5-9f95            |

**Note** : Lorsque des identifiants numériques sont utilisés, ils **NE DOIVENT** pas être séquentiels afin d'éviter une facilité à deviner le prochain identificateur.

## Représentations <a name="représentation"></a>
Un concept clé dans la conception d'API RESTful réside dans l'idée de représenter une ressource à un moment donné dans le temps.

Dans notre exemple d'un système de gestion de ressources humaines, lorsque qu'une application cliente demande une informations sur un employé, le système de RH retournera cette représentation :

```
HTTP 1.1 GET /employees/marie-tremblay
Accept: application/json

200 OK
Content-Type: application/json

{
  "nom" : "Marie Tremblay",
  "no_employe" : "123456",
  "poste" : "Directrice",
  "en_conge" : false
}
```
L'état de cette représentation peut changer dans le temps. Un appel ultérieur à ce même *"endpoint"* peut potentiellement produire une représentation différente si l'employée est en congé ou si son poste au sein de l'organisation a changé.

Il est également possible de demander une représentation entièrement différente de cette même ressource (si le système le permet). Par exemple, une version PDF de cette employée peut être produite via une instruction spécifique dans l'entête `Accept`. L'exemple ci-dessous retournera une version PDF de la requête :

```
HTTP 1.1 GET /employes/marie-tremblay
Accept: application/pdf

200 OK
Content-Type: application/pdf

...<CODE BINAIRE>...
```
## Espace de noms (*namespace*) <a name="namespace"></a>
L'espace de noms (*namespace*) d'un service définit le regroupement d'un ensemble de fonctions associées. Les espaces de noms (*namespace*) peuvent être de niveau élevé (par exemple, le nom d'un OP ou d'un service) ou bas (par exemple un projet spécifique ou une équipe précise).

Les espaces de noms (*namespace*) sont utiles pour fournir aux consommateurs d'API des accès à des regroupements via une passerelle d'API. Une fois qu'un consommateur a un jeton d'accès (*access token*) à un espace de noms particulier, il peut alors avoir accès à toutes les fonctions fournies dans cet espace de noms (*namespace*).

Les espaces de noms (*namespace*) doivent être tenus en compte dans la conception de la structure d'un URL (voir la section Noms des composants URI).

## Opérations <a name="opérations"></a>

Les développeurs utilisent des **Operations** afin d'accéder aux espaces de noms (*namespace*), ressources et des identificateurs de ressources.

Une opération est définie par l'utilisation d'une méthode HTTP et d'un chemin de ressource.

Exemples:
```
GET /employes
GET /employes/123456
DELETE /employes/123456
```

# Exigences gouvernementale des API <a name="exigences"></a>

## Documentation des API <a name="documentation"></a>

Une bonne documentation d'un API est un élément essentiel. En effet, cela facilite l'interopérabilité des services grâce à un document descriptif commun. Idéalement, la structure, les méthodes, les conventions de dénomination (*naming conventions*) et les réponses seront normalisées afin de garantir une expérience commune aux développeurs qui accèdent aux services provenant des différents OP ou agence.

En conséquence, toutes les API créées pour le gouvernement québécois **DOIVENT** fournir un document OpenAPI v2.0. Un document OpenAPI v3.0 **PEUT** également être fourni pour assurer la pérennité de l'API.

Il est **RECOMMANDÉ** d'inclure les sections suivantes dans le document de description d'un l'API :

- À propos de
- Mentions légales
    - Conditions d'utilisation
    - Licence
    - Classification des données
    - Droits d'auteur
- Exigences d'authentification
- Modèle de données
- Nous contacter

## Développement des API <a name="développement"></a>
Il est **RECOMMANDÉ** de suivre les directives suivantes lors du développement d'une API:

- Le document de description de l'API **DEVRAIENT** contenir une description de haut niveau et **DEVRAIENT** être versionnés.

- Ils **DOIVENT** être considérés comme des contrats techniques entre les concepteurs (*designers*) et les développeurs et entre consommateurs et fournisseurs.

- Les API simulées (*mock*) **DEVRAIENT** être créées en utilisant la description de l'API afin de permettre aux consommateurs de s'intégrer rapidement dans la phase de réalisation.

- Le comportement et les réponses de l'API **DEVRAIENT** être décrits avec autant d'informations que possible dans la documentation de l'API.

- La documentation **DEVRAIT** facilement accessible aux consommateurs.

- Les descriptions **DEVRAIENT** contenir au moins un exemple de requête et de réponses.

- Des exemples de corps (*bodies*) de demande et de réponse **DEVRAIENT** être fournis en totalité.

- Les codes de réponse et les messages d'erreur attendus **DEVRAIENT** être fournis en totalité.

- Des codes de réponse HTTP précis **DEVRAIENT** être utilisés.

- Les problèmes connus ou les limitations (*known issues or limitations*) **DEVRAIENT** être clairement documentés.

- Les performances attendues, la disponibilité et le niveau de service (*SLA*) des l'API **DEVRAIENT** être documentés.

- La chronologie dans laquelle les méthodes seront obsolètes **DEVRAIT** être fournie. (Voir la section Politique de fin de vie et dépréciation de l'API).

- Toute la documentation de l'API **DEVRAIT** être imprimable ou exportable.

Tous les documents OpenAPI **DEVRAIENT** être fournis au format JSON.

Afin de suivre les recommandations de versionnage de cette norme, il **DOIT** y avoir une description OpenAPI par version principale. Par exemple; si votre produit API expose et gère 3 versions principales de son API REST, vous devez fournir 3 descriptions OpenAPI (une pour chaque version, soit v1, v2 et v3).

## L'expérience du développeur <a name="expérience"></a>

Une API difficile à utiliser réduit sa probabilité d'utilisation de la part des consommateurs. Il est également peu probable que ceux-ci la recommandent à d'autres consommateurs d'API.

Il est fortement **RECOMMANDÉES** que les API en cours de conception soient testées avec de véritables consommateurs. Toutes rétroactions données **DEVRAIENT** être pris en compte afin d'être intégré à l'API.

L'équipe API du gouvernement du Québec fournit un processus d'examen des API afin de garantir que les API répondent à un niveau minimaliste d'utilisabilité avant qu'elles ne soient publiées aux consommateurs.


## Stabilité des API <a name="stabilité"></a>

Les API **DOIVENT** être conçues en gardant à l'esprit la rétro-compatibilité lorsque des modifications sont apportées.

L'introduction de nouveaux champs dans une API ou l'ajout de nouveaux points de terminaison (*endpoints*) représente un changement sans rupture.

Si le contrat API doit changer d'une manière qui rompt celui-ci avec ses des consommateurs, cela **DEVRAIT** être communiqué clairement.

1. Les propriétaires de des API **DEVRAIENT** documenter la durée de vie du support de ceux-ci.
1. Les nouvelles fonctionnalités **DOIVENT** être introduites de manière à ne pas affecter les consommateurs existants.
1. Toutes les activités de dépréciation (*deprecation*) **DOIVENT** être connues des consommateurs avant leur mise en œuvre.

## Maturité de la conception des API <a name="maturité"></a>

Lors de la conception d'une nouvelle API, l'une des principales considérations est l'expérience du développeur qui utilisera celle-ci.

Afin d'aider les concepteurs d'API peuvent se référer au modèle de maturité de Leonard Richardson (2008) afin de s'assurer que leurs API sont conforme au style REST.

![alt text](https://github.com/pdesrochers01/api-doc/blob/main/niveaux-maturité-REST.png "Niveaux de maturité de Richardson")

Le modèle de maturité proposé par Richardson classe les API en fonction de leur adhésion et de leur conformité aux principes REST selon quatre niveaux (de 0 à 3).

- Niveau 0 - État de base pour toute nouvelle API. Le niveau 0 utilise un simple URI (typiquement des POST) et HTTP comme système de transport pour les interactions entre les applications, mais sans utiliser aucun autres des mécanismes du Web. Il s’agit essentiellement d’échanger des fichiers XML (« Plain Old XML » – POX). Bien que ce style d’architecture soit populaire (simplicité d’implémentation), il n’utilise pas les caractéristiques du Web, soit sa capacité de monter en charge (scalabilité), fiabilité et sa robustesse. De plus, celui-ci amène un problème de couplage fort entre les applications. Cette approche est fortement déconseillée.
- Niveau 1 - L'API implémente différents URI, mais un seul verbe (par exemple POST). Le niveau 1 introduit des « ressources » et permet de faire des requêtes à des URI individuels (généralement en utilisant des POST) pour des actions distinctes au lieu d'exposer un point de terminaison universel (de service unique). Une ressource représente une entité unique ou une collection de tout ce qu’on peut exposer sur le Web (ex. un document, une vidéo, une fiche client, une commande, etc.).
- Niveau 2 - L'API implémente différents URI et plusieurs verbes (par exemple CRUD via GET / POST / PUT / DELETE). Le niveau 2 utilise les verbes HTTP, soit GET (obtenir des données), POST (pour créer de nouvelles données), DELETE (pour effacer des données), PUT (mettre à jour des données), etc. Cette technique permet de spécialiser davantage la ressource et d'affiner ainsi la fonctionnalité de chaque opération individuelle avec le service. Ce niveau est celui utilisé par la majorité des fournisseurs infonuagique (ex S3 d’AWS) et il permet d’accéder aux niveau 3.
- Niveau 3 - L'API implémente différents URI, plusieurs verbes et HATEOAS (« Hypermedia As The Engine of Application State ») pour représenter les relations entre les objets. Essentiellement, il s’agit d’ajouter les actions permises dans les messages de réponse. Ces actions peuvent être déterminer selon le cas d’utilisation et/ou le niveau d’autorisation du consommateur et représentent l’état de la ressource. C’est ce niveau de maturité qui permet le couplage le plus faible entre les applications.

Toutes les API adhérant à cette norme **DOIVENT** être conçues au niveau 2 du modèle de maturité Richardson.

Il est **RECOMMANDÉ** de mettre en œuvre le niveau 3 du modèle, mais ce n'est pas obligatoire pour cette norme.

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

# Conventions de nommage (*Naming Conventions*) <a name="nommage"></a>

## Format des messages <a name="formatmessages"></a>

Pour les noms de corps de requête et de réponse (*request and response body*) et des paramètres des requêtes, Il est **RECOMMANDÉ** d'utiliser le format de message  *snake-case* :

Exemple:
```
// ceci_est_un_snake_case

{
  "employe_id" : "123456"
}
```
S'il n'est pas possible d'utiliser le *snake-case* , le formet *camel case* est alors **RECOMMANDÉ** .

Exemple:
```
// CeciEstUnCamelCase

{
  "employeId" : "123456"
}
```

## Noms des composants URI <a name="composantesuri"></a>

Les URI de cette norme suivent la spécification RFC 3986 afin de simplifier le développement et la consommation de services d'API REST.

**Composants de l'URI**

La structure des URL utilisées dans les API **DEVRAIT** être significative pour les consommateurs. Les URL **DEVRAIENT** suivre une structure hiérarchique prévisible afin d'assurer sa compréhension et sa convivialité.

Les URL **DOIVENT** suivre la convention de dénomination standard tel que décrite ci-dessous:
```
https://api.quebec.ca/namespace/v1/collection?attributes=prenom,nom
\___/   \___________/\______________________/\____________________/
  |           |                   |                     |
schéma     autorité            chemin                requête
```
**Longueur maximale de l'URI**

L'URI total, y compris le chemin et la requête, **NE DOIT PAS** dépasser 2000 caractères, incluant les codes de mise en forme tels que les virgules, les traits de soulignement, les points d'interrogation, les tirets, les barres obliques, etc.

**Conventions de dénomination des URI**

Les URL **DOIVENT** suivre la convention de dénomination standard décrite ci-dessous:

- l'URI DOIT être spécifié en minuscules
- seuls les tirets «-» peuvent être utilisés pour séparer les mots ou les paramètres pour des fins de lisibilité (pas d'espaces ni de traits de soulignement)
- seuls les traits de soulignement peuvent être utilisés pour séparer les mots dans les noms de paramètres de requête, mais pas dans le cadre de l'URI de base.

Le tableau suivant explique comment construire l'URI de l'API.

| Élément de l'URI                        | Description                                                  | Exemple                                                      |
| --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Protocol                                | Toutes les API **DOIVENT** être exposées en utilisant HTTPS. | `https://`                                                   |
| Autorité > Environment                  | Le domaine dans lequel le point de terminaison (*endpoint*) de l'API sera exposé.| `api.quebec.ca`                                              |
| Chemin > API                            | Le nom d'API dérivé du domaine métier.                       | Exemple : `/namespace/nom-du-projet`. |
| Chemin > Version                        | La version de l'API à laquelle le consommateur souhaite accéder. | Exemple : `/v1`. |
| Chemin > Collection                     | La collection identifie une liste de ressources. La collection **DOIT** être nommée en utilisant la représentation plurielle d'un nom. |  |
| Chemin > Resource                       | L'identifiant de ressource qui correspond à une instance de la ressource. | Par exemple, l'API nom-du-projet, un employé spécifique avec l'ID 123456 serait récupérés en utilisant `GET` `/project-name/v1/employes/E13454` |
| Chaîne de requête> Paramètres/Filtres   | Les paramètres de requête **NE DOIVENT PAS** être utilisés pour transporter des données réelles. Les paramètres de requête suivants **DEVRAIENT** être pris en charge par votre API là où ils seraient utiles: **attributs** - spécifiez ou restreignez les attributs à renvoyer **filtres** - conditions pour restreindre / filtrer la liste de collection **sort** - spécifiez l'exigence de tri **page** - spécifiez l'index de pagination à renvoyer dans un ensemble de collections | Par exemple, `attributes=prenom,nom` retourne un élément de données ne contenant que `prenom` et `nom` attributes`filters=date_de_creation => 2001-09-20T13:00:00 et date_de_creation <= 2001-09-21T13:00:00 et prenom like 'Marie' et post_code=3000` - retourne une collection de ressources avec uen date de création entre 2001-09-20 1pm et 2001-09-21 1pm et prénom 'fred' et post_code est 3000. |

**Noms des ressources**

Les concepteurs d'API **DOIVENT** suivre ces principes lors de la création d'une API REST:

- Les noms **DOIVENT** être utilisés et non des verbes dans l'URI.
- Les noms de ressources **DOIVENT** être au pluriel.
- Les noms de ressources **DOIVENT** être en minuscules et n'utiliser que des caractères alphabétiques et des traits d'union (-).
- Le caractère trait d'union (-) **DOIT** être utilisé comme séparateur de mot dans les paramètres de chemin des URI.

Notez que c'est le seul endroit où les tirets sont utilisés comme séparateur de mots. Dans presque toutes les autres situations, le caractère de soulignement **DOIT** être utilisé.

Bons exemples:
```
/employes
/clients
/produits
```
Mauvais exemples :
```
/obtenir-employe
/client
/ajouter-produit
```
**Noms des paramètres de requête**

- Les expressions dans les chaînes de requête **DEVRAIENT** être séparés en utilisant un trait de soulignement.
- Les valeurs des paramètres de requête **DOIVENT** être codées en pourcentage.
- Les paramètres de requête **DOIVENT** commencer par une lettre et **DEVRAIENT** tous être en minuscules. Seuls les caractères alphabétiques, les chiffres et le caractère de soulignement **DOIVENT** être utilisés.
- Les paramètres de requête **DEVRAIENT** être facultatifs.
- Les paramètres de requête **NE DEVRAIENT** pas contenir de caractères non-sécure.


## Noms des champs <a name="nomschamps"></a>

Le modèle de données pour la représentation *DOIT** être conforme à la spécification `JSON`.

Les valeurs peuvent elles-mêmes être des objets, des chaînes, des nombres, des booléens ou des tableaux d'objets.

- Les noms de clé DOIVENT être des mots en minuscules, séparés par un caractère de soulignement.
   - `foo`
   - `bar_baz`
- Un préfixe tel que `is_` ou` has_` **NE DEVRAIT PAS** être utilisé pour les clés de type booléen.
- Les champs qui représentent des tableaux **DEVRAIENT** être nommés en utilisant des noms au pluriel (par exemple `produits` contient un ou plusieurs produits).

## Noms des relations des liens <a name="nomsliens"></a>

Afin de faciliter la navigation des utilisateurs dans l'API, des liens relationnels **DOIVENT** être fournis.

Un tableau `_links` **DOIT** être fourni pour les ressources. Il contient des objets lien qui peuvent référer à des ressources associées dans le système.

Une relation de lien (*link relation*) **DOIT** contenir les éléments suivants:

- href - chaîne contenant l'URL absolue ou relative de la ressource
- rel - la chaîne textuelle décrivant ce qu'est l'entité
- méthode - la méthode HTTP utilisée lors de l'utilisation de la ressource associée.

Exemple:
```
"_links": [
    {
        "href": "/v1/clients/reference-partenaire/ABC-123456",
        "rel": "_self",
        "method": "GET"
    }
]
```

## En-têtes des requêtes <a name="entetedemandes"></a>

Les en-têtes suivants **DEVRAIENT** être utilisés par défaut sur toutes les demandes.

| En-tête | Valeur par défaut   |
| ------- | ------------------- |
| Accept  | `application/json`  |

## Gestion des dates <a name="dates"></a>

### Dates formatées ISO8601

Toutes les API utilisant des dates **DOIVENT** être conformes au format [ISO 8601] (https://en.wikipedia.org/wiki/ISO_8601).

Dans ce standard, les valeurs de date et d'heure sont classées de la plus grande à la plus petite unité de temps: année, mois (ou semaine), jour, heure, minute, seconde et fraction de seconde.

La manière internationalement reconnue de représenter un objet date est :

```
AAAA-MM-JJ
```

La manière internationalement reconnue de représenter un objet temporel est:

```
hh:mm:ss.fff
```

Voici la description de ces composantes :

| Composantes    | Description                                                  | Example |
| -------------- | ------------------------------------------------------------ | ------- |
| AAAA           | Année à quatre chiffres                                      | `2021`  |
| MM             | Mois à deux chiffres (avec zéro non significatif)            | `07`    |
| JJ             | Jour à deux chiffres (avec zéro non significatif)            | `14`    |
| hh             | Deux chiffres(00 à 23)                                       | `13`    |
| mm             | Deux chiffres                                                | `48`    |
| ss             | Deux chiffres                                                | `12`    |
| fff            | Trois chiffres (milliseconde de 000 à 999)                   | `123`   |

Lorsqu'il est combiné dans un `datetime`, l'objet peut être représenté comme suit:

```
2021-07-14T13:48:12.123
```

### Utilisation des fuseaux horaires (*Timezones*)

Lors de l'utilisation du format ISO 8601, il est **RECOMMANDÉ** de fournir le fuseau horaire. Cette pratique peut permettre d'éviter une certaine complexité lors de conversion à l'heure locale.

Le fuseau horaire peut être représenté dans une variété de mécanismes, mais il est le plus souvent représenté comme un décalage par rapport à GMT + 0.

`2021-07-14T13:48:12.123DST`.

pour représenter l'heure normale de l'Est du Québec (-5), le format suivant serait utilisé:

```
2019-10-02T18:36:12.123-05:00
```

### Conventions de dénomination des champs de date

Lorsque vous utilisez des champs de date, les conventions de dénomination suivantes pour ces champs doivent être utilisées :

- Pour les propriétés nécessitant à la fois la date et l'heure, les services **DOIVENT** utiliser le suffixe `datetime`, par exemple `start_datetime`.
- Pour les propriétés ne nécessitant que des informations de date sans spécifier l'heure, les services **DOIVENT** utiliser le suffixe `date`, par exemple `date_naissance`.
- Pour les propriétés ne nécessitant que des informations d'heure sans spécifier de date, les services **DOIVENT** utiliser le suffixe `time`, par ex. `rendez-vous_start_time`.

## Exemples <a name="exemples"></a>

### Bons exemples d'URL

Liste des employés :

`GET https://api.quebec.ca/v1/employes`

Requête avec filtre :

`GET https://api.quebec.ca/v1/employes?annee=2012&sort=desc`

`GET https://api.quebec.ca/v1/employes?section=juridique&annee=2012`

Un seul employé au format JSON :

`GET https://api.quebec.ca/v1/employes/123456`

Tous les endroits où cet employé travaille :

`GET https://api.quebec.ca/v1/employes/123456/locations`

Spécifiez les champs facultatifs dans une liste séparée par des virgules :

`GET https://api.quebec.ca/v1/employes/1234?fields=titre_poste,date_debut`

Ajouter un nouvel emplacement à un employé en particulier :

`POST https://api.quebec.ca/v1/employes/123456/locations`

### Exemples d'URL incorrectes

Point de terminaison non pluriel:

`GET https://api.quebec.ca/v1/employe`

`GET https://api.quebec.ca/v1/employe/123456`

Verbe dans l'URL:

`POST https://api.quebec.ca/v1/employe/123456/ajouter`

Filtrage à l'extérieur dans l'URL au lieu de la chaîne de requête :

`GET https://api.quebec.ca/v1/employe/123456/desc`

# Versionnage des API <a name="versionnage"></a>

## Gestion sémantique de version <a name="sémantiqueversion"></a>
Toutes les API **DOIVENT** adhérer à la [gestion des versions sémantique] (https://semver.org/):

  {MAJEUR}. {MINEUR}. {PATCH}

  La première version d'une API **DOIT** toujours commencer par une version MAJEURE de 1.

  Utilisez les instructions suivantes lors de l'incrémentation du numéro de version de l'API:

  - Version **MAJEURE** lorsque vous apportez des modifications **incompatibles** ou **cassantes** d'API,
  - Version **MINEURE** lorsque vous ajoutez des fonctionnalités de manière rétrocompatible, et
  - Version **PATCH** lorsque vous effectuez des corrections de bogues rétrocompatibles.


## Version majeure <a name="versionmajeure"></a>
Toutes les API **DOIVENT** inclure uniquement la version MAJEUR dans le cadre de l'URI au format 'v {MAJEUR}', par exemple https://api.gov.au/namespace/v1/employees

Les versions mineure et corrective ne sont PAS requises dans l'URI car la modification est rétrocompatible.



### Réduire les nouvelles versions majeures

Avec chaque nouvelle version majeure de l'API, les anciennes versions sont nécessaires jusqu'à ce que tous les consommateurs soient incités à utiliser les nouvelles versions. Cela ajoute aux besoins globaux de maintenance et de support.

Utilisez des clés API pour identifier les consommateurs et désactiver les anciennes versions de l'API avec une notification suffisante.

Lorsque de nouvelles versions majeures sont publiées, l'ancienne version doit être obsolète suite au processus de dépréciation.

## Version mineure <a name="versionmineure"></a>
Les numéros de version mineure sont affichés sur la page de documentation de l'API ou font partie d'un appel de gestion spécial à l'URI de l'API lui-même. Pour prendre en charge cela, votre API **DOIT** implémenter une réponse à une requête GET à l'URI de base de l'API et renvoyer les métadonnées suivantes dans la réponse:

- **api_name:** Le nom de l'API
- **api_version:** La version de l'API avec les versions majeures et mineures
- **api_released:** La date à laquelle l'API a été publiée
- **api_documentation:** Liens vers la documentation de l'API
- **api_status:** Pour indiquer si une API est toujours active ou est obsolète.

Des métadonnées supplémentaires peuvent être ajoutées à la réponse si nécessaire.

Exemple:
```
GET /namespace/v1/

//HTTP 200 OK

{
  "api_name": "namespace",
  "api_version": "1.0.3"
  "api_released": "2018-08-10"
  "api_documentation": "https://api.quebec.ca/namespace/v1/docs"
  "api_status": "active"
}

```
##Documentation mineure et patch

La définition Swagger **DEVRAIT** contenir également la version mineure et le correctif.

La version du produit de l'API et la version de la mise en œuvre de l'API ne sont **PAS** identiques.

Une version de produit est la version logique appliquée à l'API à des fins de documentation et de référence. La version d'implémentation est la version de build physique qui a été créée.

Par exemple:

| Version de produit | Version d'implémentation de l'API | Type de changement    | Changement de version                                        |
| ------------------ | --------------------------------- | --------------------- | ------------------------------------------------------------ |
| 17.04.xx           | 1.29.xx                           | Corrections de bogues | La version du produit sera modifiée en **17.04.02** si les modifications sont liées au produit. La version d'implémentation de l'API sera modifiée en **1.29.02** si les modifications sont liées à l'API |
| 17.04.xx           | 1.29.xx                           | Rétrocompatible       | La version du produit sera modifiée en **17.05.xx** si les modifications sont liées au produit. La version d'implémentation de l'API sera modifiée en **1.30.x** si les modifications sont liées à l'API |
| 17.04.xx           | 1.29.xx                           | Non rétrocompatible   | La version du produit sera remplacée par **18.01.x** si les modifications sont liées au produit. La version d'implémentation de l'API sera modifiée en **2.00.x** si les modifications sont liées à l'API |

Une mise à jour de correctif (Patch ou Service Pack) **DOIT** avoir une compatibilité descendante.


## Rétrocompatibilité <a name="rétrocompatibilité"></a>
Il est essentiel que les API soient développées avec un couplage lâche à l'esprit pour assurer la compatibilité descendante pour les consommateurs.

Les modifications suivantes sont réputées rétrocompatibles:

- Ajout d'un nouveau champ optionnel à une représentation
- Ajout d'un nouveau lien au tableau `_links` d'une représentation
- Ajout d'un nouveau point de terminaison à une API
- Prise en charge supplémentaire d'un nouveau type de média (par exemple, `Accept: application / pdf`)

Les modifications suivantes ne sont **PAS** considérées comme rétrocompatibles:

- Suppression des champs des représentations
- Changements des types de données sur les champs (par exemple chaîne en booléen)
- Suppression des terminaux ou des fonctions
- Suppression du support du type de support

Toutes ces modifications **DOIVENT** nécessiter une mise à jour majeure de la version, elles doivent donc être gérées avec prudence.

## Politique de fin de vie <a name="findevie"></a>
Lors de la conception de nouvelles API, l'une des dates les plus importantes à prendre en compte est celle du retrait de l'API.

Les API ne sont pas destinées à durer éternellement. Certaines API sont retirées après une courte période car elles peuvent prouver un cas d'utilisation, d'autres peuvent être supprimées lorsque de meilleures options sont disponibles pour les utilisateurs.

La politique de fin de vie (EOL) détermine le processus par lequel les API passent pour passer à travers leur flux de travail de l'état «LIVE» à l'état «RETIRED».

La politique EOL est conçue pour garantir une période de transition cohérente et raisonnable pour les clients API qui doivent migrer de l'ancienne version d'API vers la nouvelle version d'API tout en permettant à un processus sain de retirer sa dette technique.

### Version mineure de l'API EOL

Selon la politique de gestion des versions, les versions mineures de l'API **DOIVENT** être rétrocompatibles avec les versions mineures précédentes dans la même version majeure. Par conséquent, les versions mineures de l'API sont «RETIRED» immédiatement après qu'une version mineure plus récente de la même version majeure devient «LIVE». Cette modification ne devrait avoir aucun impact sur les abonnés existants, il n'est donc pas nécessaire de passer par un état `DEPRECATED` pour faciliter la migration du client.

### Version principale de l'API EOL

Les versions majeures de l'API **PEUVENT** être rétrocompatibles avec les versions majeures précédentes. Les règles suivantes s'appliquent lors du retrait d'une version majeure de l'API.

1. Une API majeure **NE DOIT PAS** être à l'état "DEPRECATED" jusqu'à ce qu'un service de remplacement soit "LIVE" qui fournit un chemin de migration clair pour toutes les fonctionnalités qui seront conservées à l'avenir. Ceci **DEVRAIT** inclure de la documentation et des outils / exemples de code de migration qui fournissent aux utilisateurs ce dont ils ont besoin pour effectuer une migration propre.
2. La version obsolète de l'API **DOIT** être à l'état «DEPRECATED» pendant une période minimale de 60 jours pour donner aux utilisateurs un préavis suffisant pour migrer.
3. L'abandon des versions d'API avec des utilisateurs externes **DEVRAIT** être envisagé au cas par cas et peut nécessiter des délais et / ou des contraintes supplémentaires pour minimiser l'impact sur les utilisateurs.
4. Si une API versionnée à l'état «LIVE» ou «DEPRECATED» n'a pas d'utilisateurs enregistrés, elle **PEUT** passer immédiatement à l'état «RETIRED».

### Version principale de l'API de remplacement

Étant donné qu'une nouvelle version majeure de l'API qui entraîne la dépréciation d'une version d'API préexistante est un investissement produit important, les propriétaires d'API ** DOIVENT ** justifier la nouvelle version majeure avant de commencer un travail de conception et de développement important.

Les propriétaires d'API ** DEVRAIENT ** explorer toutes les alternatives possibles à l'introduction d'une nouvelle version majeure de l'API dans le but de minimiser l'impact sur les clients avant de décider d'introduire une nouvelle version.

La justification **DEVRAIT** inclure les éléments suivants:



####  Business case

   1. Valeur client fournie par une nouvelle version qui n'est pas possible avec la version existante.
   2. Analyse coûts-avantages de la version obsolète par rapport à la nouvelle version.
   3. Explication des alternatives à l'introduction d'une nouvelle version majeure et pourquoi celles-ci n'ont pas été choisies.
   4. Si une modification rétrocompatible est requise pour résoudre un problème de sécurité critique, les éléments 1 et 2 (ci-dessus) ne sont pas requis.

#### Conception d'API

   1. Un modèle de domaine de toutes les ressources dans la nouvelle version d'API et comment elles se comparent au modèle de domaine de la version d'API principale précédente.
   2. Description des opérations des API et des cas d'utilisation qu'elles implémentent.
   3. Définition d'objectifs de niveau de service pour les performances et la disponibilité qui sont égaux ou supérieurs à la version principale de l'API à déconseiller.

#### Stratégie de migration

   1. Nombre de clients existants touchés; internes, externes et partenaires.
   2. Plan de communication et de support pour informer les clients existants de la nouvelle version, de la valeur et du chemin de migration.


## Désuétude des API (Deprecated) <a name="désuétude"></a>
Lorsqu'une API plus récente est disponible, les propriétaires d'API sont **RECOMMANDÉS** de fournir deux en-têtes dans la réponse lorsque d'anciennes versions sont utilisées:

- X-API-Deprecated - champ booléen indiquant que cela est obsolète
- X-API-Retire-Time - Date ISO8601 indiquant quand il sera obsolète

Par exemple:

```
GET `/ namespace / v1`

// 200 OK
Content-Type: application / json; jeu de caractères = utf-8
X-API-obsolète: true
X-API-Retire-Time: 2018-11-17T13: 00: 00Z
```

Cela fournit aux consommateurs un rappel constant que l'API est marquée comme obsolète et qu'il existe probablement une autre version disponible pour qu'ils migrent.

# Requête des API et paramètres <a name="requêteparamètres"></a>

## Entête des requêtes <a name="entêterequêtes"></a>
Toutes les API **DOIVENT** prendre en charge les en-têtes de requête suivants:



| Entête                        | Valeur                                                       |
| ----------------------------- | ------------------------------------------------------------ |
| Autorisation / Identification | Peut être l'un des éléments suivants : clé API, authentification de base (clé API + secret), nom d'utilisateur + mot de passe, Jeton (token) |

Les en-têtes des requêtes suivantes sont facultatives.

| Entête        | Valeur                                                       |
| ------------- | ------------------------------------------------------------ |
| Content-Type  | Peut être l'un des valeurs suivantes :`application/json` (obligatoire), `application/xml` (optionel pour xml), `multipart/form-data` (optionel pour les fichiers), `application/x-www-form-urlencoded` (optionel pour les form data) |
| Accept        | Content-Types qui sont acceptable pour la réponse. Peut être un choix de :`application/json` (obligatoire), `application/xml` (optionel pour xml) |
| Connection    | Options de contrôle pour la connexion actuelle. Par exemple : `keep-alive`. |
| Date          | La date et l'heure à laquelle le message a été émis, au format "HTTP-date" tel que défini par [RFC 7231 Date / Time Formats] (http://tools.ietf.org/html/rfc7231#section-7.1.1.1) . Par exemple. «Mar 15 novembre 1994 08:12:31 GMT». |
| Cookie        | Un cookie HTTP précédemment envoyé par le serveur.           |
| Cache-Control | Utilisé pour spécifier des directives qui doivent être respectées par tous les mécanismes de mise en cache, par exemple pas de cache. |
| ETag          | Utilisé pour identifier la version particulière d'une ressource en cours de mise à jour pour empêcher plusieurs mises à jour utilisateur. Cela doit correspondre à ce qui est actuellement stocké sur le serveur. |


## Méthodes des requêtes HTTP <a name="méthodeshttp"></a>
Les opérations de l'API RESTful sont basées sur la norme HTTP Request Method telle que définie par la [RFC 7231] (https://tools.ietf.org/html/rfc7231#section-4.3).



### Méthodes de requête HTTP supportées



| Méthode HTTP | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `GET`        | Pour *récupérer* une ressource.                              |
| `POST`       | Pour *créer* une nouvelle ressource, ou pour * exécuter * une opération sur une ressource qui change l'état du système, par ex. envoyer un message. |
| `PUT`        | Pour *remplacer* une ressource par une autre fournie dans la demande. |
| `PATCH`      | Pour effectuer une *mise à jour partielle* d'une ressource.  |
| `DELETE`     | Pour *supprimer* une ressource.                              |
| `HEAD`       | Pour récupérer des métadonnées sur la demande, par ex. combien de résultats retournera la requête? (sans  exécuter la requête). |
| `OPTIONS`    | Utilisé pour déterminer si une demande CORS (partage de ressources inter-origines, en anglais *cross-origin resource sharing*) peut être effectuée. Ceci est principalement utilisé dans les applications Web *front-end* pour déterminer si elles peuvent utiliser directement les API. |



Une demande de récupération de ressources peut être effectuée pour une seule ressource ou une collection de ressources.

Prenons l'exemple suivant:

```
https://api.quebec.ca/agence/v1/clients/{id}
```

Pour récupérer une collection de clients, une requête est envoyée à l'URN `/customers`.

Pour récupérer un seul "client", une requête est envoyée à l'URN `/customers/{id}`.

### Collection de ressources

Les opérations suivantes s'appliquent à une collection de ressources:

| Méthode HTTP | Chemin des ressources | Opération                                       | Exemples                                             |
| ------------ | --------------------- | ----------------------------------------------- | ---------------------------------------------------- |
| GET          | `/ressources`         | Obtenez une collection de la ressource          | GET `/employés` ou GET<br />` /employés?Status=open` |
| POST         | `/ressources`         | Créez une nouvelle instance de cette ressource. |                                                      |

**Note :**

La création ou la mise à jour de plusieurs instances de ressources dans la même demande n'est actuellement pas standardisée. Il existe des facteurs tels que l'accusé de réception et la manière de gérer le succès partiel d'un ensemble de lots qui doivent être considérés au cas par cas.

Les futures versions de la spécification pourraient aborder le traitement par lots à l'aide d'API.



### Ressource unique

Les opérations suivantes sont applicables pour une seule ressource:



| Méthode HTTP | Chemin des ressources | Opération                                                    |
| ------------ | --------------------- | ------------------------------------------------------------ |
| GET          | `/resources/{id}`     | Obtenez l'instance correspondant à l'ID de ressource         |
| PUT          | `/resources/{id}`     | Pour mettre à jour une instance de ressource en la remplaçant |
| SUPPRIMER    | `/resources/{id}`     | Pour supprimer l'instance de ressource en fonction de la ressource |
| PATCH        | `/resources/{id}`     | Effectuez des modifications telles que l'ajout, la mise à jour et la suppression des attributs spécifiés. Est souvent utilisé pour effectuer des mises à jour partielles sur une ressource |

## Formats du contenu des requêtes (Request Payload Formats) <a name="formatréponses"></a>
Au minimum, l'API **DOIT** prendre en charge une charge utile au format `JSON` lorsqu'elle est fournie.

D'autres formats de données utiles tels que `XML`, `CSV` et `YAML` peuvent être pris en charge si nécessaire.

La prise en charge du format supplémentaire doit être documentée dans la conception de votre API (définition Swagger).

## Idempotence <a name="idempotence"></a>
Une méthode HTTP idempotente est une méthode HTTP qui peut être appelée plusieurs fois sans résultats différents. Dans certains cas, les appels secondaires entraîneront un code de réponse différent, mais il n'y aura pas de changement d'état de la ressource.
Par exemple, lorsque vous invoquez N demandes DELETE similaires, la première demande supprimera la ressource et la réponse sera 200 (OK) ou 204 (Aucun contenu). Les demandes supplémentaires renverront 404 (non trouvé). De toute évidence, la réponse est différente de la première requête, mais il n'y a aucun changement d'état pour aucune ressource côté serveur car la ressource d'origine est déjà supprimée.

| Méthode HTTP | Est idempotent |
| ------------ | -------------- |
| `GET`        | Vrai           |
| `POST`       | Faux           |
| `PUT`        | Vrai           |
| `PATCH`      | Faux           |
| `DELETE`     | Vrai           |
| `HEAD`       | Vrai           |
| `OPTIONS`    | Vrai           |

Les méthodes API RESTful **DOIVENT** adhérer à l'idempotence spécifiée dans le tableau ci-dessus



## Pagination <a name="paramètresrequête"></a>
La pagination est le processus qui consiste à renvoyer un grand ensemble de résultats sous forme de blocs (ou de pages) afin de réduire la quantité d'informations envoyées avec chaque requête.

La pagination nécessite plusieurs paramètres de requête à fournir, et des informations supplémentaires sur la façon de configurer cela sont fournies dans la section pagination de ce document.

## Filtrage et tri <a name="Filtragetri"></a>
Offrir la possibilité de filtrer et de trier les collections dans votre API offre à vos consommateurs une plus grande flexibilité et contrôle la façon dont ils choisissent de consommer votre API.

Il existe un certain nombre de techniques sur la façon de procéder et d'autres explications sont fournies ci-dessous, cependant **NE PAS** définir les paramètres de filtrage et de tri dans le cadre de l'URI de l'API (par exemple, `/employés/age/de/20/à/30` ).

Les paramètres de filtre ne font pas partie de la définition de la ressource. Utilisez plutôt des paramètres de requête pour spécifier les exigences de filtrage et de tri.

Exemple:

```
?date_of_birth=\>1999-12-31 and \<=2001-09-21T13:00:00
```

# Réponses des API <a name="réponses"></a>

## Entête des réponses <a name="entêteréponses"></a>
Le type de contenu recommandé est `JSON` (` application/json`).

Les en-têtes de réponse suivants **PEUVENT** être inclus dans toutes les réponses:

| En-tête                      | Valeur                                                       |
| ---------------------------- | ------------------------------------------------------------ |
| Access-Control-Allow-Origin  | URL qui est autorisée à accéder à ce service directement à partir de javascript et de clients basés sur un navigateur. <br />**Remarque:** N'utilisez jamais d'URL génériques (*) à moins que la ressource REST ne soit vraiment publique. |
| Access-Control-Allow-Methods | Les méthodes auxquelles il est possible d'accéder directement à partir de javascript et de clients basés sur un navigateur. |
| Access-Control-Allow-Headers | Les en-têtes auxquels il est possible d'accéder directement à partir de javascript et de clients basés sur un navigateur. |
| Content-Type                 | Choix de: <br />`application / json` (obligatoire)<br />` application / xml` (facultatif pour `xml`)<br />` multipart / form-data` (facultatif pour les fichiers)<br />`text / html` (facultatif pour` html`) |
| Cache-Control                | Informe les mécanismes de mise en cache.                     |
| Date                         | La date et l'heure à laquelle le message a été émis Date par ex. Mar 15 novembre 1994 08:12:31 GMT |
| Expire                       | Donne la date / heure après laquelle la réponse est considérée comme périmée, par ex. Jeu. 01 décembre 1994 16:00:00 GMT |
| ETag                         | Utilisé pour identifier la version particulière d'une ressource. Le client doit l'inclure dans toutes les demandes de mise à jour pour s'assurer qu'il est inchangé. |



## Codes de réponse HTTP <a name="codesréponsehttp"></a>
TBC : https://restfulapi.net/http-status-codes/


## Contenu des réponses <a name="contenuréponses"></a>
La charge utile de réponse pour une API peut concerner une seule ressource ou un ensemble de ressources.

Lorsque le format de réponse est «JSON», les normes de réponse suivantes s'appliquent:

### Ressource unique

Les codes d'état suivants représentent les réponses appropriées aux différentes opérations qui peuvent être effectuées sur une seule ressource dans le système.

| Request Method | Resource Path     | Status                 | Code |
| -------------- | ----------------- | ---------------------- | ---- |
| POST           | `/resources/{id}` | Created                | 201  |
|                |                   | Accepted               | 202  |
|                |                   | Bad Request            | 400  |
|                |                   | Unauthorised           | 401  |
|                |                   | Forbidden              | 403  |
|                |                   | Not found              | 404  |
|                |                   | Not Allowed            | 405  |
|                |                   | Unsupported Media Type | 415  |
|                |                   | Unprocessable Entity   | 422  |
|                |                   | Internal Server Error  | 500  |
| PUT            | `/resources/{id}` | OK                     | 200  |
|                |                   | Accepted               | 202  |
|                |                   | No content             | 204  |
|                |                   | Bad Request            | 400  |
|                |                   | Unauthorised           | 401  |
|                |                   | Forbidden              | 403  |
|                |                   | Not found              | 404  |
|                |                   | Not Allowed            | 405  |
|                |                   | Unsupported Media Type | 415  |
|                |                   | Unprocessable Entity   | 422  |
|                |                   | Internal Server error  | 500  |
| DELETE         | `/resources/{id}` | Accepted               | 202  |
|                |                   | No Content             | 204  |
|                |                   | Bad Request            | 400  |
|                |                   | Unauthorised           | 401  |
|                |                   | Forbidden              | 403  |
|                |                   | Not found              | 404  |
|                |                   | Not Allowed            | 405  |
|                |                   | Internal Server error  | 500  |
| PATCH          | `/resources/{id}` | Accepted               | 202  |
|                |                   | No content             | 204  |
|                |                   | Bad Request            | 400  |
|                |                   | Unauthorised           | 401  |
|                |                   | Forbidden              | 403  |
|                |                   | Not found              | 404  |
|                |                   | Not Allowed            | 405  |
|                |                   | Unsupported Media Type | 415  |
|                |                   | Unprocessable Entity   | 422  |
|                |                   | Internal Server error  | 500  |

### Collection of Resources

The following status codes represent appropriate responses to the different operations that can be performed on a collection resource within the system.

| Request Method | Resource Path | Status                 | Code |
| -------------- | ------------- | ---------------------- | ---- |
| GET            | `/resources/` | OK                     | 200  |
|                |               | Bad Request            | 400  |
|                |               | Unauthorised           | 401  |
|                |               | Forbidden              | 403  |
|                |               | Not found              | 404  |
|                |               | Not Allowed            | 405  |
|                |               | Unsupported Media Type | 415  |
|                |               | Internal Server error  | 500  |




*******
1. [Hypermédia](#hypermédia)
    1. [Hypermédia - Données liées](#hypermédiadonnées)
    1. [HATEOAS](#hateoas)
    1. [API compatible hypermédia](#compatiblehypermédia)
    1. [Link Description Object](#linkobject)
    1. [Type de relation de lien](#typelien)
# Hypermédia <a name="titre1"></a>

## HATEOAS <a name="hateoas"></a>
TBC https://restcookbook.com/Basics/hateoas/
«Hypermédia en tant que moteur de l'état de l'application» est le concept de représentation des actions autorisées sous forme d'hyperliens associés à la ressource. Similaire au concept de données liées hypermédia, les liens définis dans les données de réponse représentent des transitions d'états disponibles de cet état actuel vers des états adjacents.

```
{
   "numéro_compte": "12345",
   "solde": 100,00,
   "_liens":[
     {"rel": "** deposit **", "href": "/ accounts / 12345 / deposit"},
     {"rel": "** retirer **", "href": "/ accounts / 12345 / retirer"},
     {"rel": "** transfer **", "href": "/ accounts / 12345 / transfer"}
   ]
}
```

Mais si le même compte est à découvert de 25, la seule action autorisée est le dépôt:

```
{
   "numéro_compte": "12345",
   "solde": -25,00,
   "_liens":[{
     "rel": "dépôt",
     "href": "/ comptes / 12345 / dépôt"
   }]
}
```

La décision d'implémenter les conceptions HATEOAS dans votre API dépend de la volonté des clients de consommer HATEOAS et de l'effort de conception et de mise en œuvre.

## API compatible hypermédia <a name="compatiblehypermédia"></a>
TBC

Une API compatible hypermédia fournit aux consommateurs une machine à états accessible des transitions qu'ils peuvent utiliser.

Dans les API, les méthodes de requête telles que «DELETE», «PATCH», «POST» et «PUT» lancent une transition dans l'état d'une ressource. Une requête «GET» ne **DOIT** jamais changer l'état de la ressource qui est récupérée.

Afin de fournir une meilleure expérience aux consommateurs d'API, les concepteurs d'API **DEVRAIENT** fournir une liste des transitions d'état disponibles pour chaque ressource dans le tableau `_links`.

*********
Les API **DEVRAIENT** suivre une politique de «pagination d'abord».

La pagination est **RECOMMANDÉE** pour:

1. répondre aux demandes en temps opportun (par exemple, plus long que 2 secondes)
2. assurez-vous que la quantité de données renvoyées reste dans une charge utile gérable (par exemple, plus gros que 500 Ko)
3. Assurez-vous que les données de la réponse sont facilement gérables pour améliorer l'expérience utilisateur.


# Outils de test <a name="outilstest"></a>

Il existe une grande variété d'outils de test gratuits (open source) disponibles pour les tests d'API:

- **SoapUI** - SoapUI est l'outil de test fonctionnel Open Source pour les tests d'API. Il prend en charge plusieurs protocoles tels que SOAP, REST, HTTP, JMS, etc. Vous pouvez télécharger SoapUI à partir de https://www.soapui.org/downloads/soapui.html
- **Postman** - Postman est une application pour interagir avec les API HTTP. Il vous présente une interface graphique conviviale pour la construction de requêtes et la lecture des réponses. Vous pouvez télécharger Postman sur https://www.getpostman.com/
- **Curl** - cURL est un outil pour travailler avec des URL. cURL nous permet d'interroger une URL à partir de la ligne de commande. Cela nous permet de publier des données de formulaire. Avec cURL, nous pouvons facilement essayer de nouvelles API. Vous pouvez télécharger le curl à partir de la ligne de commande sous Linux en tapant sudo yum install curl
- **Apache benchmark** - L'outil ApacheBench (ab) peut charger des serveurs de test en envoyant un nombre arbitraire de requêtes simultanées. Vous pouvez télécharger le benchmark Apache à partir de la ligne de commande sous Linux en tapant yum install httpd-tools
- **Swagger** - L'éditeur Swagger peut être utilisé pour valider votre définition Swagger avant de l'importer dans API Manager.


## Références <a name="références"></a>

[Fielding's Dissertation on REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

[xxx](yyy)

[Government of Canada Web API Standard](https://github.com/wet-boew/wet-boew-api-standards#style-guide)

[18F API Standards](https://github.com/18F/api-standards)

[Gov.UK API Service Manual](https://www.gov.uk/service-manual/technology/application-programming-interfaces-apis)

[PayPal API Design Guidelines](https://github.com/paypal/api-standards/blob/master/api-style-guide.md)

[Refinery29 API Standards](https://github.com/refinery29/api-standards#url-structure-and-versioning)

[LeaseWeb API Design Standards](https://github.com/LeaseWeb/api-standards)

[OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)

[HTTP Status Codes](https://restfulapi.net/http-status-codes/)

[JSON:API, a specification for building APIs in JSON](https://jsonapi.org/format/)


*********template*********
# Titre1 <a name="titre1"></a>

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
