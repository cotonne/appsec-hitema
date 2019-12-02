---
title: Quelques rappels poru le mobile
tags: [mobile]
keywords: mobile, rappels
last_updated: November 2, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_mobile_rappels
---

# Travaux pratiques

## Installation de l'environnement de développement

 * Télécharger [Androl4b disponible ici](https://github.com/sh4hin/Androl4b). Attention, il vous faudra environ 20Go de disponible.
 * Exécuter les commandes suivantes pour créer un émulateur en local:
```bash
$ cd /home/andro/Android/Sdk/tools/bin/
$ ./sdkmanager "platform-tools" "platforms;android-19" “system-images;android-19;default;armeabi-v7a”
$ ./avdmanager create avd --force --name myAVD --abi default/armeabi-v7a --package 'system-images;android-19;default;armeabi-v7a' --device "Nexus S"
$ /home/andro/Android/Sdk/emulator/emulator64-arm -avd myAVD
```

## Applications vulnérables

### DIVA

 * Télécharger DIVA 

### UnCrackable

 * Télécharger [UnCrackable](https://github.com/OWASP/owasp-mstg/tree/master/Crackmes)
