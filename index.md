---
title: "Securité Applicative - HiTeMa"
keywords: sample homepage
tags: [getting_started]
sidebar: appsec_sidebar
permalink: index.html
summary: Ce site reprend les principales informations du cours "Sécurité Applicative" à HiTeMa ainsi que les instructions pour les cours
---

{% include note.html content="A l'attention des étudiants HiTeMa : Merci de consulter ce site avant le cours pour avoir les différentes instructions" %}

## Instructions pour les prochains cours

### Vendredi 7 décembre

 - Télécharger [InsecureBankV2](https://github.com/dineshshetty/Android-InsecureBankv2/raw/master/InsecureBankv2.apk)
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

## Modalités d'évaluation

### Examen

Un examen aura lieu le **Date à définir** afin de valider les différentes compétences acquises pendant
le cours.

### Projet

Un projet est prévu le 4 mars 2019. Il donnera lieu à un rapport présentant:

 - le problème
 - la méthodologie suivie
 - les solutions envisagées

Le rapport utilisera au mieux les connaissances acquises lors du cours "Sécurité Applicative".

Le rapport pourra être fait à un ou plusieurs.

Date limite : **à définir**

### Bonus

La participation active à ce site (ajout de notes, la relecture, propositions d'amélioration, ...)
pourra donner lieu à une bonification de la note.

