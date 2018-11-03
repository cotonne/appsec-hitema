---
title: Quelques rappels
tags: [architecture]
keywords: architecture, rappels
last_updated: November 2, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_archi_rappels
---

# Définition

L'architecture logicielle décrit les composants logiciels constituant l’application. Il répond aux questions sur la manière de réaliser les exigences fonctionnelles et non-fonctionnelles.

# Modèles d'architecture logicielle

## Architecture n-tiers

Architecture 3-tiers:
 - Couche de présentation: interface utilisateur affichant les données de manière accessible à ce dernier
 - Couche de traitement: logique métier, fonction de l’architecture
 - Couche d’accès aux données: l’endroit où les données sont stockées

# Problèmes d'architecture logicielle

Application monolithique
 - Complexité à comprendre le comportement
 - Analyse statique compliquée
 - Correction compliquée
 - Propagation de la menace

Compartimentation
 - Plus facile à comprendre
 - Complique la vie de l’attaquant
 - Défense en profondeur: si un échoue, un autre prendra le relais

# Secure Design Patterns

Modèles existants pour résoudre des problèmes connus
Niveau
 - Architecture: haut niveau de responsabilités entre les composants
 - Conception: décrit les interactions entre les composant
Implémentation: bas-niveau/secure coding

Ressources:
 - [Secure Design Patterns](https://resources.sei.cmu.edu/asset_files/TechnicalReport/2009_005_001_15110.pdf)
 - 