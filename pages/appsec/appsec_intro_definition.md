---
title: Definitions
tags: [formatting]
keywords: notes, tips, cautions, warnings, admonitions
last_updated: July 3, 2016
summary: ""
sidebar: appsec_sidebar
permalink: appsec_intro_definition
---

# Sécurité

D'après le dictionnaire Larousse, la sécurité se définit comme:
 * Situation dans laquelle quelqu'un, quelque chose n'est exposé à aucun danger, à aucun risque, en particulier d'agression physique, d'accidents, de vol, de détérioration
 * Situation de quelqu'un qui se sent à l'abri du danger, qui est rassuré.
 * Absence ou limitation des risques dans un domaine précis

Plusieurs termes sont importants à noter:
 * Quelqu'un/Quelque chose: que cherche-t'on à protéger?
 * Risque: Possibilité, probabilité d'un fait, d'un événement considéré comme un mal ou un dommage. Il s'agit de quelque chose de négatif qui peut arriver.
 * Danger: dans notre cas, on parlera d'"attaquant". On pourra aussi penser aux vers, virus, cheval de Troie (trojan en anglais), ...

En anglais, le "Oxford Dictionnary" nous donne la définition suivante:
 * The state of being free from danger or **threat**.

On gardera ce mot en tête.

# Actifs

Un actif, au niveau d'une entreprise, est tout ce qui peut avoir de la valeur pour une organisation, son métier et la continuité de celui-ci. Cela inclut les informations qui servent à la mission de l'organisation.

Les actifs visés peuvent être matériels (argent, informations clients, contrats, ...) ou immatériels (comme la réputation).

# Sécurité de l'Information

La Sécurité de l'Information (en: Information Security/InfoSec) est définie comme telle dans la loi américaine [LAW]:

    Practice of preventing unauthorized access, use, disclosure, disruption, modification, inspection, recording or destruction of information

La sécurité de l'Information cherche l'équilibre entre efficacité opérationnelle et mise en oeuvre de la politique de sécurité.
Elle adresse un large éventail de menaces.

Un acronyme courant est CIA pour "Confidentiality, Integrity, Availability". Il définit les propriétés souhaitées pour un système en termes de sécurité de l'Information.
Il s'agit d'un modèle pour penser et discuter des concepts sur la sécurité.

La notion de sécurité de l'information n'est d'ailleurs pas limité aux données purement numériques ou au monde de l'informatique.

Une norme adresse particulièrement les problématiques liées à la sécurité de l'information: **ISO/CEI 27XXX**. Cette norme sera vue dans le cours **XXX**.

Ressources:
 * [LAW](https://www.law.cornell.edu/uscode/text/44/3542)

# Sécurité Applicative

La Sécurité Applicative (AppSec) cherche à protéger les actifs au niveau de l’application. La Sécurité applicative est le processus qui a pour but de trouver, corriger et améliorer la sécurité des applications afin de les rendre plus sûres.

La norme ISO 27034 (Guide ISO sur la sécurité applicative) donne la définition suivante: 

    « La sécurité applicative est un processus effectué pour appliquer des contrôles et des mesures aux applications d’une organisation afin de gérer le risque de leur utilisation ».

Wikipedia propose cette définition:

    « measures taken to improve the security of an application often by finding, fixing and preventing security vulnerabilities » (Wikipedia 2018)

Globalement, on s’intéresse à tout ce qui a trait à l’application :  Architecture, design, développement, test, déploiement,  ...
