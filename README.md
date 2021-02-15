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
    1. [REST](#rest)
    1. [API](#api)
    1. [Ressources](#resssources)
    1. [Identifiants de ressource](#resssources)
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
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus purus urna, elementum vitae blandit vitae, accumsan nec sem. Sed non urna placerat, aliquet leo at, eleifend nisi. Nunc gravida, metus at tempus tristique, nunc mauris placerat ipsum, ac aliquet purus erat id ex. Aenean at nunc mauris. Nullam posuere dui ligula, quis tempor dolor sollicitudin eu. Cras dapibus vel nisl eu placerat. Donec ut vehicula nibh, non convallis metus. Aenean vel consequat ante, nec convallis nisi. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Fusce pretium feugiat leo, at dictum purus. Aenean sem lectus, ornare a mollis id, consequat a nisl. Integer ut turpis in neque ornare aliquet nec quis lacus. Curabitur volutpat magna nisl.

## Contact <a name="contact"></a>
In gravida, sapien congue pharetra convallis, sem diam consequat nisi, ut ullamcorper magna eros ut ante. Nunc mattis placerat metus, id malesuada leo rutrum sed. Interdum et malesuada fames ac ante ipsum primis in faucibus. Praesent et enim ac nisl rutrum efficitur. Duis ultrices eros quis orci scelerisque, et mollis tellus euismod. Donec facilisis scelerisque sem, eu elementum metus pretium et. Nulla placerat nisl consectetur tortor accumsan vehicula. Donec sit amet risus eget urna viverra aliquam. Morbi pharetra lacinia tempus.

# Pour bien démarrer <a name="démarrer"></a>

## Pourquoi une norme de conception des API? <a name="norme"></a>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus purus urna, elementum vitae blandit vitae, accumsan nec sem. Sed non urna placerat, aliquet leo at, eleifend nisi. Nunc gravida, metus at tempus tristique, nunc mauris placerat ipsum, ac aliquet purus erat id ex. Aenean at nunc mauris. Nullam posuere dui ligula, quis tempor dolor sollicitudin eu.

## Comment appliquer cette norme de conception? <a name="appliquer"></a>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus purus urna, elementum vitae blandit vitae, accumsan nec sem. Sed non urna placerat, aliquet leo at, eleifend nisi. Nunc gravida, metus at tempus tristique, nunc mauris placerat ipsum, ac aliquet purus erat id ex. Aenean at nunc mauris. Nullam posuere dui ligula, quis tempor dolor sollicitudin eu.

## Pourquoi choisir REST? <a name="rest"></a>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus purus urna, elementum vitae blandit vitae, accumsan nec sem. Sed non urna placerat, aliquet leo at, eleifend nisi. Nunc gravida, metus at tempus tristique, nunc mauris placerat ipsum, ac aliquet purus erat id ex. Aenean at nunc mauris. Nullam posuere dui ligula, quis tempor dolor sollicitudin eu.

## La norme de spécification OpenAPI <a name="openapi"></a>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus purus urna, elementum vitae blandit vitae, accumsan nec sem. Sed non urna placerat, aliquet leo at, eleifend nisi. Nunc gravida, metus at tempus tristique, nunc mauris placerat ipsum, ac aliquet purus erat id ex. Aenean at nunc mauris. Nullam posuere dui ligula, quis tempor dolor sollicitudin eu.
