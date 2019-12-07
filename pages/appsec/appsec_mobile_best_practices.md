---
title: TPs et bonnes pratiques
tags: [mobile]
keywords: mobile, TP, Androl4b
last_updated: November 2, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_mobile_best_practices
---

# Travaux pratiques

## Installation de l'environnement

 * Télécharger [Androl4b disponible ici](https://github.com/sh4hin/Androl4b). Attention, il vous faudra environ 20Go de disponible.

De préférence, utiliser VirtualBox. 

Lors de l'installation:
 - Assigner 2 CPU à la VM
 - Désactiver le support de l'USB.

## Emulateur Android

Par défaut, Androl4b a un émulateur installé prêt à l'emploi. Vous pouvez le démarrer en cliquant sur l'icône "Emulator" sur le bureau ou en exécutant la commande suivante dans un invité de commande:

```bash
andro@l4b:~$ /home/andro/Android/Sdk/emulator/emulator @lab
```

Il est alors possible de se connecter sur le téléphone émulé avec:
```bash
andro@l4b:~$ adb shell
root@generic:/ # id
uid=0(root) gid=0(root) context=u:r:shell:s0
root@generic:/ # 
```

On notera que, par la suite, les commandes éxécutées sur un terminal Linux d'Androl4b seront préfixées par **andro@l4b:** et celles sur le téléphone par **root@generic:**.

Si vous souhaitez tester les commandes sur votre téléphone, vous devez activer le [mode développeur](https://developer.android.com/studio/debug/dev-options) dessus.
Dans le cas de l'émulateur, celui-ci est rooté, nous permettant d'effectuer des actions impossibles sur un vrai téléphone.

# Analyse d'un APK

## Analyse statique

### Ouvrir un APK avec jadx

### Modifier les sources

### 

## Analyse dynamique

### frida

#### Installation

Pour l'analyse statique du code, nous nous appuyerons sur [Frida](https://github.com/frida/frida/releases). Frida permet de faire de l'instrumentation dynamique de code, c'est-à-dire qu'il peut modifier le programme sans avoir besoin de modifier les sources.

Frida est déjà installé sur la VM. Il est préférable de commencer par mettre à jour frida:
```bash
andro@l4b:~$ sudo pip install --upgrade frida
andro@l4b:~$ /home/andro/.local/bin/frida --version
12.7.25
```

Télécharger ensuite le serveur frida qui servira pour manipuler le programme Android:
```bash
andro@l4b:~$ wget -O frida-server.xz https://github.com/frida/frida/releases/download/$(~/.local/bin/frida --version)/frida-server-$(~/.local/bin/frida --version)-android-arm.xz
andro@l4b:~$ unxz frida-server.xz
```

```bash
andro@l4b:~$ adb forward tcp:27042 tcp:27042
andro@l4b:~$ adb push frida-server /data/local/tmp/ 
andro@l4b:~$ adb shell "chmod 755 /data/local/tmp/frida-server"
andro@l4b:~$ adb shell "/data/local/tmp/frida-server &"
andro@l4b:~$ ~/.local/bin/frida-ps -U | grep frida
1066  frida-server
```

#### UnCrackable-Level1

L'OWASP a développé [un ensemble d'applications](https://github.com/OWASP/owasp-mstg/tree/master/Crackmes) que nous pouvons utiliser pour nous entrainer. Chaque application contient un mot de passe qu'il va valoir récupérer et valider dans l'application.

Commençons par récupérer le niveau 1:

```bash
andro@l4b:~$ wget https://github.com/OWASP/owasp-mstg/raw/master/Crackmes/Android/Level_01/UnCrackable-Level1.apk
andro@l4b:~$ adb install UnCrackable-Level1.apk
```

Il est installé dans la liste des apps sous le nom **Uncrackable1**. Si vous tentez de le démarer, l'application affiche une popup et s'arrête juste après indiquant que le téléphone est rooté.

{% include image.html file="uncrackable1_popup.png" caption="Popup au démarrage" %}

Analysons le code pour voir le problème avec jadx:

```bash
andro@l4b:~$ /home/andro/Desktop/Tools/MARA_Framework/tools/jadx-0.6.0/bin/jadx-gui UnCrackable-Level1.apk 
```

Si nous analysons l'unique Activity de cet apk, nous voyons qu'elle fait appel à plusieurs fonctions de **sg.vantagepoint.a.c**:
{% include image.html file="uncrackable1_src_activity.png" caption="Code source de l'activity" %}

La classe c implémente plusieurs [mécanismes de détection de rooting](https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05j-Testing-Resiliency-Against-Reverse-Engineering.md) du téléphone: présence des programmes comme su, APKs spécifiques, ...
{% include image.html file="uncrackable1_c.png" caption="Code source de la classe c" %}

Il nous est nécessaire d'outre-passer cette protection. Frida va nous y aider.
Dans le code, nous avons la condition suivante:

```java
if (c.a() || c.b() || c.c()) {
  a("Root detected!"); // End of application
}
```

Il faut donc faire en sorte que toutes les fonctions retournent **false** pour ne pas rentrer dans la condition. Frida peut installer des **hooks** dans le code de l'application qui va remplacer les fonctions existantes par nos fonctions. Créer le fichier hook.js avec le contenu suivant:

```javascript
Java.perform(function () {
    const RootDetector = Java.use('sg.vantagepoint.a.c');
    RootDetector.a.overload().implementation = function (arg) {
        return false;
    }
    RootDetector.b.overload().implementation = function (arg) {
        return false;
    }
    RootDetector.c.overload().implementation = function (arg) {
        return false;
    }
}
```

Nous pouvons ensuite lancer le hook avec la commande suivante:
```bash
andro@l4b:~$ frida -U -f org.owasp.uncrackable1 -l hook.js
```




# Allez plus loin...

## Installer un autre émulateur

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


