---
title: Architecture Technique
tags: [architecture]
keywords: architecture, technique
last_updated: November 2, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_archi_technical
---

# Définition

L'architecture technique décrit les composants matériel supportant l’application. Par exemple, l'architecte technique décrira:
 - Machines (physique ou virtuelle)
 - Bases de données (SQL, NoSQL, ...)
 - Flux
 - Produits Applicatifs (OS, Serveurs, Frameworks, …)
 - Réseaux & topologie
 - Disques (SAN, NAS, …)
 - Services (API)
 - ...

Cette description est généralement conservée dans un document d'architecture technique. Ce document est alors utilisé par les équipes d'exploitation pour commander (**provisionner**) les machines, installer les produits applicatifs, ouvrir les flux réseaux, construire les sous-réseaux, ...

{% include image.html file="20181015-architecture-technique-layers.png" caption="Composants techniques pour la sécurité par couche OSI" %}

# Exemples d'architecture

L'ANSSI propose un [GUIDE DE DEFINITION D’UNE ARCHITECTURE DE PASSERELLE D’INTERCONNEXION SECURISEE](https://www.ssi.gouv.fr/uploads/IMG/pdf/2011_12_08_-_Guide_3248_ANSSI_ACE_-_Definition_d_une_architecture_de_passerelle_d_interconnexion_securisee.pdf). Nous reprenons deux exemples d'architecture pour illustrer notre propos.

## Architecture “Accès via pare-feu”

{% include image.html file="20181015-architecture-simple-parefeu.png" caption="Architecture avec un pare-feu" %}

## Architecture basée sur deux pare-feux

{% include image.html file="20181015-architecture-double-parefeux.png" caption="Architecture avec deux pare-feux" %}

Plusieurs éléments sécurisent notre système:
 - Firewalls de différents constructeurs: cela réduit l’impact en cas de découverte d’une vulnérabilité. L'inconvénient: double compétence sur les produits. C'est à prendre en compte lors de la définition de l'architecture. Pour configurer sans erreur un outil, il faut un bon niveau d'expertise, qui est souvent dur à avoir. Donc, il faut maintenir deux équipes, avec leur propre expertise. On retrouve ici encore notre équilibre coût/risque.
 - Proxy: filtre les accès de l’intérieur vers l’extérieur. Cela peut bloquer des accès non voulus.
 - Reverse proxy: filtre de l’extérieur vers l’intérieur. On concentre généralement tous les flux en un point pour pouvoir les filtrer. Cela masque aussi les ressources internes.
 - Terminaison SSL : permet l’inspection des données
 - Répartition de charge: peut être assuré par un serveur dédié comme bluecoat
 - VPN
 - WAF: Web Application Firewall. Firewall de niveau applicatif, c'est à dire au-dessus du niveau 4 de l'OSI.
 - Base de données
 * Chiffrement du disque
 * Chiffrement des mots de passe en base

## Applications des principes

Nous allons maintenant revoir ces architectures par rapport aux principes vus précédemment.

### Architecture basée sur deux pare-feux

 - Defense in Depth : deux firewalls séparent les réseaux et se renforcent. Des systèmes à jour et renforcés (**Hardened systems**) peuvent être utilisées pour les machines de l'application (AppArmor, SELinux, services désactivés, ...)
 - Isolation & segregation of duties : plusieurs réseaux & VLAN par fonction (opération, exploitation, sauvegarde, supervision, ...)
 - Least privilege : les flux réseaux sont limités par les firewalls.
 - Psychological acceptability : quasi-transparent
 - Compromise recording: supervision
 - Least common mechanism: séparation des réseaux
