---
title: Quelques rappels
tags: [architecture]
keywords: architecture, rappels
last_updated: November 2, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_archi_rappels
---

# Le modèle OSI

Le modèle OSI est une architecture en sept couches. Chaque couche communique avec la couche du dessus et celle du dessous. Il s'agit d'un modèle: les coupures ne sont pas si claires, mais aident à comprendre certains concepts.

Ce modèle permet de simplifier et réfléchir sur les échanges entre applications.

Dans ce cours, nous nous intéresserons aux trois dernières couches (Application/Présentation/Session).

{% include image.html file="20181015-architecture-modele-osi.png" caption="Modèle OSI (Source: https://fr.wikipedia.org/wiki/Mod%C3%A8le_OSI)" %}

Exemples:
 - Couche physique : cable, bluetooth, …. ⇒ Interception
 - Couche liaison (data layer) : ethernet, MAC address ⇒ ARP spoofing
 - Couche réseau : IPv4, IPv6, … => syn flood, ...
 - Couche transports: TCP, UDP, … => usurpation, ...
 - Couche Session : HTTP, ...
 - Couche Présentation : ASN.1, XML, ...
 - Couche Application: DNS, FTP, ...

# L'importance de la conception

La sécurité se retrouve à tous les niveaux d'une application. Il ne faut pas attendre le dernier moment pour se poser la question de comment inclure de la sécurité dans un application.

Nous le verrons dans la partie sur les cycles de vie sécurisées: la sécurité se retrouve à toutes les étapes.
La conception, une activité qui arrive en amont des développements, est une activité sensible. Si une faille est introduite à ce moment, il est souvent coûteux et long de pouvoir corriger.

Dans ce chapitre, nous nous intéresserons à plusieurs éléments :
 - L'architecture technique, logicielle et fonctionnelle
 - Les outils d'analyse des menaces : DFD, Threat Trees

Le graphique suivant donne une idée du coût pour fixer une faille (généralement admis) en fonction de l'étape de découverte dans un projet.

{% include image.html file="20181015-architecture-cout-anomalie.png" caption="Coût d'une anomalie" %}

Comme on peut voir, plus elle est découverte tard, plus le coût est important.

# Définitions

L'étymologie d'architecture est la contraction de **principe** et **couvrir/clore**. On peut voir l'architecture comme l'art de définir des règles de construction d’un ouvrage. Dans le cas de l'informatique, notre ouvrage est un logiciel.

Martin Fowler [donne la définition suivante de l'architecture](https://youtu.be/DngAZyWMGR0) :
> Software architecture is those decisions which are both important and hard to change.

Dans "Secure Coding, principles & practices", Graff et van Wyk parlent d'architecture sécurisée:
> We think of security architecture as a body of high-level design principles and decisions that allow a programmer to say "Yes" with confidence and "No" with certainty.

Par la suite, nous verrons un ensemble de principes pour répondre à cette définition.

Nous nous intéresserons à l'**Architecture Technique**, l'**Architecture Logicielle** et l'**Architecture Fonctionnelle**.
