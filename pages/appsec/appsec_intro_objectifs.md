---
title: Objectifs de la sécurité applicative
tags: [formatting]
keywords: appsec, objectifs
last_updated: July 3, 2016
summary: ""
sidebar: appsec_sidebar
permalink: appsec_intro_objectifs
---

## Confidentiality

La confidentialité est la capacité d'un système à protéger ses données de lecture non autorisées.
Le système s'assure que l'information n'est pas accéder par des utilisateurs non autorisés.

La confidentialité est proche de la notion de vie privée (privacy).
La "vie privée" réfère à la volonté d'une personne ou d'un groupe de personnes à vouloir protéger ses intérets.

La "vie privée" concerne les personnes tandis que la confidentialité s'intéresse à la donnée ou l'information en général.

Les menaces liées à la confidentialité peuvent devenir des problèmes de violations de vie privée, surtout si les données sont confidentielles ou sensible (PII).

On peut penser à différents cas de pertes de confidentialité:
 - Perte d'une clé USB contenant des informations importantes
 - Interception d'une communication (écoutes - en: eavesdropping)
 - Affichage d'une information (Information Disclosuer)

Pour protéger l'information, nous utilisons des techniques de chiffrement (en: to crypt).

### Chiffrement

Le chiffrement est un procédé qui consiste à transformer un message afin de s'assurer que seul le destinaire puisse le lire.

Le chiffrement est utilisé depuis des millénaires. Jules César chiffrait déjà ces messages via une décalage des lettres de l'alphabet.

Nous évoquerons les deux grandes familles de chiffrements: symétrique et asymétrique.

Les élèves sont invitées à lire les livres suivants pour compléter leur compréhension:
 * Un livre d'introduction à la cryptographie (référence à venir)
 * Exercices et problèmes de cryptographie de Damien Vergnaud

#### Principe de Kerckhoffs

Le principe de Kerckhoffs (1883) énonce six propriétés souhaitables d'un cryptosystème.

On s'intéressera sur la deuxième propriété : "**Il faut qu’il n’exige pas le secret, et qu’il puisse sans inconvénient tomber entre les mains de l’ennemi**".

Cette propriété est importante car elle indique que la sécurité du système ne doit pas reposer uniquement sur le fait que le fonctionnement de celui-ci n'est connu que de nous seul.
Un exemple: Enigma. Une fois la machine récupérée, l'algorithme a été cassée, ce qui donna un avantage aux Alliées lors de la Seconde Guerre Mondiale.
De plus, le fait que la sécurité du système repose uniquement sur le secret utilisé permet de faciliter son changement en cas de découverte.
Dans un système moderne, on peut imaginer la compléxité de changer un algorithme de chiffrement si la sécurité n'est plus assurée parce qu'il a été découverte.
Il faut toujours supposer que l'attaquant connait le système.

#### Chiffrement symétrique

Le chiffrement symétrique s'appuie sur une clé secrète partagée entre les deux parties.

{% include image.html file="20181015-introduction-chiffrement-symetrique.png" caption="Chiffrement symétrique" %}

Le principal défi dans un système à chiffrement symétrique est l'échange de manière sécurisée de la clé.

Quelques exemples d'algorithmes:
 * AES
 * Blowfish
 * DES (obsolète), 3DES

Quelques exemples d'attaques
 * Algorithme perso: **ne jamais écrire son propre algorithme!**
 * Fréquences
 * Brute-force
 * Texte chiffré seul
 * Texte clair connu
 * Texte clair choisi
 * Texte chiffré choisi
 * Injection d’erreurs
 * Canaux cachés

#### Chiffrement asymétrique

Le chiffrement asymétrique s'appuie sur une clé privée (qui ne doit surtout par être divulgée) et d'une clé publique partagée, utilisée pour la communication.

{% include image.html file="20181015-introduction-chiffrement-symetrique.png" caption="Chiffrement asymétrique" %}

L'intéret est qu'il n'y a pas de risques en soit à communiquer la clé publique utilisée pour chiffrer. Seul le destinataire, qui possède la clé privée, est en mesure de déchiffrer et donc de lire le message. La sécurité est assurée par la complexité calculatoire pour inverser la clé publique.

Le désavantage de ces algorithmes est qu'ils sont plus complexes et donc plus consommateurs en temps de calcul.

Quelques exemples d'algorithmes:
 * RSA
 * Courbes elliptiques (EC)

#### Quelques questions à se poser

Plusieurs questions sont à se poser. Même si les algorithmes sont sûrs, des erreurs dans la configuration affaiblissent la sécurité.

 * Chiffrement symétrique
 - Choix de l’algorithme? ROT13? Vigenère? DES? 3DES? AES?
 - Quelle taille de clé choisir?
 * Chiffrement asymétrique
 - Choix de l’algorithme? RSA? EC?
 - Taille de la clé? 128, 256, 512, 1024, …?
 - Génération des clés? (PRNG)
 * Choix des blocs? ECB? CBC? OFB?
 * Comment stocker les clés en sécurité? Comment les échanger?

## Integrity

L'intégrité est la "**capacité à empêcher qu’une donnée soit modifiée par une personne non autorisée ou d’une façon non voulue**".

En plus Non seulement empêcher les modifications mais aussi être capable de revenir dans un état précédent.

Une autre propriété qui en découle est l'**auditabilité** (en : accountability). On doit pouvoir comprendre comment le système a pu arriver dans cet état, qui a fait la modification, quand et pourquoi.

Derrière l'intégrité se cache la capacité d'un système à prendre des décisions. En effet, comment valider un achat si on n'est pas sûr des informations concernant l'état d'un compte en banque par exemple?

Quelques exemples de perte d'intégrité
 - Accès/Introduction dans un système
 - Injection d’erreurs lors d’une transaction bancaire

### Authentification et autorisation

Pour assurer l'intégrité, le système doit donc être capable de reconnaitre un utilisateur et doit être aussi capable d'associer les droits à cet utilisateur.

Dans le premier cas, on parle d'authentification et dans le deuxième, d'autorisation.
 * Authentification : confirmer l’identité de l’utilisateur ou de l’entité
 * Autorisation : donner (to grant) les droits d’accès aux ressources d’un système (fichiers, processus, …)

L'authentification d'un utilisateur repose sur un des axes suivants:
 * Quelque chose que l’on sait : mot de passe, question secrète, ...
 * Quelque chose que  l’on a : token d’authentification, téléphone, ...
 * Quelque chose que l’on est : biométrie

Il est aussi possible de combiner ces différents modes. On parle alors de **Multi-Factor Authentication** (ou MFA).

Différents modèles d'autorisations existent:
 - Mandatory Access Control
 - Discretionary Access Control
 - Role-Based Access Control
 - ...

### Fonction de hachage et signature

Dans le cas d'un échange chiffré avec un système asymétrique,
rien ne garantit que l'émetteur est bien celui qui a envoyé le message.
Un attaquant pourrait intercepter le message et créer son message chiffré avec la clé publique du destinataire.

Un moyen de s'assurer de l'intégrité de l'échange est de signer le message transmis.

Pour ce faire, deux couples de clés entrent en jeu:
 - celui du destinataire. Clé publique : pk_d, clé privée: sk_d
 - celui de l'émetteur. Clé publique : pk_e, clé privée: sk_e

 L'émetteur du message peut chiffrer le message envoyé avec la clé publique pk_d du destinataire. Il peut ensuite chiffrer une version condensée du message avec sa clé privée sk_e.
 Le destinataire reçoit le message et peut le déchiffrer avec sa clé privée sk_d. Il peut ensuite condenser le message reçu et déchiffrer le condensé reçu avec la clé publique pk_e de l'émetteur. Comme seul l'émetteur a pu chiffrer le condensé émis avec sa clé privée, si les deux condensés correspondent, alors le message a bien été émis par l'émetteur et n'a pas été modifié.

 On nomme "**signature**", le condensé chiffré par l'émetteur.

{% include image.html file="20181015-introduction-signature.png" caption="Signature" %}

Un point important est qu'il faut toujours assurer la confidentialité du message. En effet, le condensé peut être déchiffrer puisqu'il est déchiffrable avec la clé publique de l'émetteur (connu par l'attaquant).
Il faut donc que le condensé ne fournisse pas d'informations utiles à l'attaquant.

Pour ce faire, les fonctions à sens unique sont utilisées, comme des fonctions de hachage. Le message donné une fonction de hachage donne un **hash**.

Ce sont des fonctions théoriquement non inversibles. L'espace de départ est généralement plus petit que l'espace d'arrivée, ce qui peut entrainer des collisions (d'où un risque potentielle de pouvoir réutiliser un signature pour un message différent).

Une "bonne" fonction de hachage doit avoir les propriétés suivantes:
* Résistance à la pré-image: Fonction non réversible ⇒ h-1 n’existe pas, pour un h donné, il est difficile de trouver h = H(m)
 * Résistance à la seconde pré-image: pour un m donné, difficile de trouver un m’ tel que H(m) = H(m’)
 * Résistance aux collisions: difficile de trouver m et m’ tel que H(m) = H(m’)

Attaques connues:
 * Brute-force
 * Rainbow table. Il s'agit de tables contenant des hashs pour de nombreux mots de passe. Une manière de rendre inutile ce type de table est d'ajouter un sel au condensé.

## Availability

La dernière propriété, la disponibilité, est la capacité d'avoir accès aux données.

En effet, même si l'information est bien confidentielle et intègre, à quoi sert un système s'il n'est pas possible d'accéder à ces informations?

Quelques exemples d'indisponibilité :
 * DoS : Denial of Service
 * DDoS: Distributed Denial of Service

