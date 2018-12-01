---
title: "Securité Applicative - HiTeMa"
keywords: sample homepage
tags: [getting_started]
sidebar: appsec_sidebar
permalink: index.html
summary: Ce site reprend les principales informations du cours "Sécurité Applicative" à HiTeMa ainsi que les instructions pour les cours
---

{% include note.html content="A l'attention des étudiants HiTeMa : Merci de consulter ce site avant le cours pour avoir les différentes instructions" %}

## Modalités d'évaluation

L'évaluation est constituée de deux parties:
 - Un travail en groupe
 - Un travail individuel

Pour le travail en groupe, il faudra:
 - Rendre un rapport avant le 25 février dernier délai (sur 8 points)
 - Présenter votre travail pendant de 45 minutes (sur 8 points) le 4 février

Pour le travail en individuel, il faudra:
 - Rendre un rapport avant le 25 février dernier délai (sur 8 points)
 - Présenter votre travail pendant de 45 minutes (sur 8 points) le 4 février

### Travail en groupe

Le sujet sera présenté le mardi 8 janvier dans l'après-midi. L'après-midi sera consacré à l'étude et à la préparation du sujet.
Les groupes seront défini le mardi.

#### Rapport

Le rapport présentera:
 - le problème et l'analyse associée
 - la méthodologie suivie
 - les solutions envisagées

Dans ce rapport, vous étudierez le sujet proposé. Vous mettrez en oeuvre les connaissances acquises lors du cours.
Vous proposerez des solutions en mettant en avant les avantages et les inconvénients.

Le rapport utilisera au mieux les connaissances acquises lors du cours "Sécurité Applicative".

Le rapport pourra être fait à un ou plusieurs.

Taille estimée: 15-20 pages (hors introduction, conclusion, page de garde, bibliographie, ...).

La qualité de l'écrit étant importante, un rapport contenant trop de fautes (orthographe, grammaire) sera pénalisé.

Date limite : **25 février 2019**

### Présentation

Vous présenterez en classe vos conclusions. La présentation durera 45 min.
Vous y résumerez votre rapport. 

Vous montrez aussi votre maîtrise technique en démontrant la présence de failles dans l'application étudiée.

Date de la présentation: **4 mars 2019**

### Travail individuel

Pour la partie individuelle, vous choisirez une problématique liée à la sécurité applicative.
Le sujet reste libre: analyse d'une faille spécifique, problématique de sécurité liée à une nouvelle technologie,
audit d'une application, ... Vous détaillerez vos résultats dans un rapport simplifié.

Les éléments utilisés (scripts, programmes, ...) devront aussi ếtre sous une forme au choix (partage de fichiers, repository, ...)

Taille estimée: au moins 3 pages.

Date limite : **25 février 2019**

### Bonus

La participation active à ce site (ajout de notes, la relecture, propositions d'amélioration, ...)
pourra donner lieu à une bonification de la note.

## Instructions pour les prochains cours

### Vendredi 7 décembre

Il est conseillé d'utiliser un environnement linux (Debian-based de préférence) pour cette session (facilitera énormément les installations). 
Une VM peut être utilisée ([Androl4b](https://github.com/sh4hin/Androl4b) par exemple).
 - Installer python 2
 - Télécharger [DIVA](https://github.com/cotonne/appsec-hitema/diva.apk)
 - Décompresser le dossier, suivre les instructions d'AndroLabServer
 - Télécharger et installer le [sdkmanager](https://developer.android.com/studio/), paragraphe "**Command line tools only**". Il n'est pas nécessaire de télécharger Android Studio.
 - Installer les composants suivants:

```
$ cd <dossier  où l'archive a été extraite>
$ bin/sdkmanager "platform-tools"  # fourni adb
$ bin/sdkmanager "platforms;android-27" # fourni les images
$ bin/sdkmanager "system-images;android-27;default;x86_64"# emulator
$ bin/sdkmanager "system-images;android-27;google_apis;x86" # emulator
$ bin/avdmanager create avd --device "Nexus 6" --package "system-images;android-27;default;x86_64" --name "testx"
$ cd ../emulator
$ ./emulator @testx
$ cd ../platform-tools
$ ./adb install InsecureBankv2.apk
```

### Vendredi 16 novembre

Nous utiliserons les outils suivants lors de cette séance:

 - Google Gruyère. [Suivre les instructions pour installer en local](https://google-gruyere.appspot.com/part1) (Utile en cas de coupure réseau
 - [bodgeit](https://github.com/psiinon/bodgeit)
 - [OWASP Juice Shop](https://www.owasp.org/index.php/OWASP_Juice_Shop_Project)

### Mardi 16 octobre

Installer les composants suivants:

 - OWASP Zap
 - WebGoat 7

### Lundi 15 octobre

Aucun

### Logiciels utilisés pendant le cours

Les logiciels utilisés pendant le cours sont tous gratuits (Free as in beer) and open source.
Les étudiants sont invités à les lire leur code source pour bien comprendre leur fonctionnement.

 - OWASP Zap
 - OWASP WebGoat
 - Google Gruyere
 - Android SDK et platform-tools
 - InsecureBankv2
 - SQLMap
 - Kali
 - frida
 - jd-gui
 - apktool
 - IDA
 - Intellij

## Contact

 - Adresse email : yvan (point) phelizot (arobase) arolla (dot) fr



