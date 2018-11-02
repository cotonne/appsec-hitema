---
title: Principes de conception
tags: [architecture]
keywords: architecture, principes
last_updated: November 2, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_archi_principles
---

Ce chapitre introduit des principes sur lesquels nous pouvons nous appuyer pour concevoir et construire une application qui soit sécurisée par "design". Ce sont des principes de haut-niveau, indépendant des choix techniques ou logicielles. Ils sont peu nombreux et simple à comprendre.

La conception doit être faite en gardant à l'esprit que nous avons un adversaire dont nous voulons nous protéger (**Adversary Principle**). Nous reverrons plus tard les activités de modélisation des menaces (**Threat Modeling**) qui nous aident dans ce travail. En effet, nous ne chercherons pas à adresser toutes les possibilités, mais plutôt celles qui sont réalistes ou très dangereuses (en fonction de notre contexte de travail).

Ressources:
 - "Secure Coding, Principles & Practices", chapitre 2, Graff & van Wyk
 - "Code Complete", Mc Connell

# Saltzer and Schroeder's design principles (1975)

Dans leur article de 1975, Saltzer et Schroeder ont proposé une liste de dix principes qui sont importants pour concevoir un système sécurisé. Il faut noter que ces principes sont utilisables à différents niveaux: pour un urbaniste (ou architecte du SI), un architecte, un développeur ou un exploitant.

Voici la liste des principes:

 - **Economy of mechanism** : Un conception simple est plus facile à tester et à valider. Moins il y en a, mieux c'est. Mais attention, simple ne veut par dire simpliste (le minimum est fait) ni petit. Simple s'oppose à compliqué  et inutile mais pas forcément à complexe (le domaine fait que c'est complexe, par exemple les Mathématiques). On pensera alors à l'**over-engineering**.
 - **Fail-safe defaults** : par défaut, en cas d'erreur, le système ne doit être dans un état moins protégé. Par défaut, aucun accès n'est autorisé puis, on ouvre progressivement les accès en fonction des droits.
 - **Complete mediation** : pour chaque opération, l'authenticité, l'intégrité et les droits doivent être vérifiés. Les droits d'accès doivent être vérifiés.
 - **Open design** : un système ne doit pas reposé uniquement sur l'obfuscation (le fait de vouloir perturber la compréhension de notre système par un adversaire). Notre conception doit pouvoir être revue par des pairs pour évaluer et détecter les problèmes. On retrouve ici un des principes de Kerchoffs. Il faut se mettre dans la situation où un ennemi connait le système.
 - **Separation of privilege** : il faut empêcher que les actions dépendent des choix d’un seul élément.
 - **Least privilege** : il faut toujours effectuer une action avec le moins de droits possibles. Si des droits supplémentaires sont nécessaires, ils doivent être abandonnées une fois l'action terminée. Une façon de voir est: "est-ce qu’un admin devrait avoir les droits de se connecter à une application?"
 - **Least common mechanism** : il faut faire attention avec tout ce qui est partagé. Les utilisateurs ne devraient pas partager les mêmes outils, sauf si c’est indispensable. Par exemple, doit-on utiliser le même câble pour le réseau, même PC, … (accès à des fichiers partagés, …)
 - **Psychological acceptability** : si une application est trop compliqué, les utilisateurs vont soit s'en lasser, soit trouver un contournement (création d'un **shadow IT**, ...). De plus, le système doit aider l'utilisateur et l'empêcher de commettre des erreurs.
 - **Work factor** : l'effort à fournir par l’attaquant pour rentrer dans le système doit être . Gros mots de passe, … Voici un extrait de "The Security Development Lifecycle" sur le sujet:
> The cost-benefit ratio for a criminal is defined by Clark and Davis (Clark and Davis 1995) as
> Mb + Pb > Ocp + Ocm * Pa * Pc
> where
> * Mb is the monetary benefit for the attacker.
> * Pb is the psychological benefit for the attacker.
> * Ocp is the cost of committing the crime.
> * Ocm is the monetary costs of conviction for the attacker (future lost opportunities and legal costs).
> * Pa is the probability of being apprehended and arrested.
> * Pc is the probability of conviction for the attacker.
Si *Ocp* ou *Ocm* sont importants, l'attaquant peut ne pas essayer d'attaquer.
 - **Compromise recording** : Le système devrait conserver une trace des différentes attaques, même si elles n'ont pas été bloquées. En cas d'analyse post-mortem (**forensic**), ces logs seront d'une aide précieuse. Ils sont aussi un point sensible qu'un attaquant cherchera à compromettre pour ne pas se faire détecter ou retrouver.

Ressources:

 - "[The Protection of Information in Computer Systems](http://web.mit.edu/Saltzer/www/publications/protection/)", JEROME H. SALTZER & MICHAEL D. SCHROEDER

# Saltzer and Kaashoek design principles (2009)

Dans leur livre "Principles of Computer System Design", Saltzer et Kaashoek s'intéressent aux principes de conception de système d'un point de vue plus général. On pourra retenir les règles suivantes:

 - **Minimize secrets** : Parce qu'ils ne vont pas rester secret bien longtemps. Il doit y en avoir peut et ils doivent être facilement changeable (rotation of secrets).
 - **Least astonishment** : ne réinventez pas la roue. Les logiciels sont utilisés par des personnes. Ils sont parti prenant lors de la conception de l'outil. La conception, le développement, l'utilisation du logiciel, ... doivent respecter l'état de l'art dans leur domaine.
 - **Design for iteration** : Vous n'arrivez pas à faire bien du premier coup. Le système doit être penser pour pouvoir être facilement évoluable. On est dans l'amélioration en continu du logiciel en tant que principe de conception. Le lecteur intéressé pourra voir des concepts comme **evolutionary architectures** ou **agile architecture**.

Ressources:
 - "Principles of Computer System Design", Saltzer et Kaashoek, 2009
 - [Design principles, extrait du livre précédent](https://ocw.mit.edu/resources/res-6-004-principles-of-computer-system-design-an-introduction-spring-2009/online-textbook/principles_open_5_0.pdf)

# Quelques autres principes

 - **Defense in depth** : il faut généralement construire son système en incluant plusieurs couches de défense. Si une couche échoue, une autre prend le relais.
 - **Fail fast** : en cas d'erreur, il faut arrêter l'exécution le plus tôt possible et ne pas la laisser se propager, au risque d'impacter un autre système.
 - **Deny by default**: comme évoquer précédemment, par défaut, tout est fermé
 - **Plan on failure**: lors de la conception, il est important de se demander: "que va-t'il se passer en cas d'échec?" ou "que se passera-t'il si je ne reçois pas ce que j'attends?"
 - **Isolation** : c'est l'idée de confinement. On va exécuter du code non sûr dans un système isolé (**sandbox**). On va s’assurer que du code non sûr ne peut pas abîmer le reste du système (utilisateurs, processus, vm, machine, chroot, …). L'attaquant va alors développer des techniques d'évasion ou de détection de ce genre d'environnement.
