# Lignes directrices sur les API du gouvernement du Québec

# Table of contents
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
    1. [Opérations](#opérations)
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


# Préface <a name="préface"></a>

## Introduction <a name="introduction"></a>
Ce document décrit la norme de conception pour l'ensemble des interfaces de programmation d'application (API) du Québec. Ce guide s'adresse à toute personne œuvrant au développement de services numériques pour une fonction publique, que ce soit dans le cadre de la fonction publique du Québec, d'une agence gouvernementale ou au-delà.

## Audience <a name="audience"></a>
Le public visé par ce document est constitué des développeurs d'API, des architectes de solutions et des analystes commerciaux.

## Document Semantics, Formatting, and Naming <a name="document"></a>
Les mots clés `DOIT`, `NE DOIT PAS`, `DEVRAIT`, `NE DEVRAIT PAS`, `RECOMMANDÉ`, `PEUT` et `OPTIONNEL` dans ce document doivent être interprétés comme décrit dans le standard [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

Les mots REST et RESTful DOIVENT être écrits tels que présentés ici, représentant l'acronyme comme toutes les lettres majuscules. Cela est également vrai pour JSON, XML et d'autres acronymes.

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

API de niveau système: il s'agit d'API de bas niveau exposées directement par une application.

API de niveau processus: il s'agit d'API composées d'autres API système via l'orchestration et la chorégraphie.

API de niveau d'expérience: il s'agit d'API destinées à faciliter l'adoption de l'intégration d'API entre une organisation et ses consommateurs externes.

Si votre API fait partie de l'API de niveau système et est développée sur mesure, il est RECOMMANDÉ d'utiliser la norme de conception car cela vous aidera à développer des API de niveau processus ou expérience si elles sont requises à l'avenir.

Si votre API est une API de niveau de processus, vous DEVRIEZ appliquer la norme de conception car le plus souvent, une API de niveau de processus sera adaptée pour la réutilisation.

Si votre API est une API de niveau d'expérience, les normes de conception DOIVENT être appliquées.

Cette norme de conception elle-même ne s'applique PAS aux API tierces au niveau du système telles que celles disponibles en tant que «prêtes à l'emploi» ou faisant partie de la plate-forme SaaS, par exemple. API Salesforce ou API ArcGIS. Cependant, la norme peut s'appliquer si vous cherchez à réexposer ces API en tant qu'API de niveau d'expérience pour une consommation plus large.

## Pourquoi choisir REST? <a name="rest"></a>

Cette norme de conception d'API se concentre largement sur l'utilisation des API HTTP REST (Representational State Transfer) comme base de conception.

Alors qu'il existe des normes et des modèles de conception émergents pour les API (y compris GraphQL et gRPC), les développeurs du monde entier ont largement accepté REST comme mécanisme de facto de représentation et de transfert de données vers et depuis des systèmes sur Internet.

REST fonctionne bien lors de la modélisation des systèmes et des données. Les principes peuvent être appliqués aux systèmes qui sont à la fois grands et petits et les outils disponibles pour les développeurs prennent largement en charge l'accès aux données prêt à l'emploi.

Les API REST ne sont généralement pas adaptées à la diffusion de données (websockets) et ne sont pas non plus la meilleure utilisation pour les API largement basées sur des fonctions (gRPC / JSON-RPC). GraphQL est une alternative qui gère ces aspects du développement d'une manière différente et a été considérée comme une option pour les standards WoG.

Étant donné que les outils pour les API REST sont plus largement disponibles et que les connaissances technologiques générales du gouvernement sont déjà raisonnablement familiarisées avec les API REST et les principes de conception, il a été déterminé que REST serait la base de la modélisation de cette norme de conception d'API pour une utilisation dans le gouvernement australien .

Il est prévu que le développement futur de ces normes de conception prendra également en compte GraphQL et gRPC / JSON-RPC.


## La norme de spécification OpenAPI <a name="openapi"></a>

Des exemples de modèles OpenAPI (anciennement appelés Swagger) qui exposent les méthodes d'API et sont conformes au Guide de conception de services sont fournis ici pour aider les concepteurs d'API à démarrer. Les concepteurs d'API peuvent utiliser ces modèles comme base pour démarrer leur définition d'API à partir d'un point de départ conforme aux normes.

Les modèles disponibles sont:

OpenAPI v3.0 Template (JSON format)
OpenAPI v3.0 Template (YAML format)

Une fois la définition OpenAPI copiée, les concepteurs peuvent effectuer les tâches suivantes:

Fournissez une description pertinente de l'API - Reportez-vous à la ligne numéro 5.

Revoir et mettre à jour la description

Pour chaque méthode:

Mettre à jour la définition du champ
Mettre à jour la description
Donnez des exemples
Passez en revue les codes d'état et les messages d'erreur et mettez à jour si nécessaire
Voir la section Documentation API pour plus de détails sur les informations à inclure dans le fichier de documentation OpenAPI.

NB. Ces exemples OpenAPI montrent comment une API peut être définie. Vérifiez toujours le référentiel pour la dernière copie du modèle et des exemples.

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
TBC
## Représentations <a name="représentation"></a>
TBD

## Espace de noms <a name="namespace"></a>
TBD

## Opérations <a name="opérations"></a>
TBC
********
# Définitions <a name="définition"></a>

## REST <a name="rest"></a>
TBD

## API <a name="api"></a>
TBD

## Ressources <a name="resssources"></a>
TBD

## Identifiants de ressource <a name="idresssources"></a>
TBC
## Représentations <a name="représentation"></a>
TBD

## Espace de noms <a name="namespace"></a>
TBD

## Opérations <a name="opérations"></a>
TBC
