---
title: Les menaces du Cloud
tags: [cloud]
keywords: cloud, menaces
last_updated: July 3, 2016
summary: "Les menaces du Cloud"
sidebar: appsec_sidebar
permalink: appsec_cloud_menaces
---

Le Cloud est un nouvel outil puissant. C'est un allié dans la réussite de nombreux projets métier.
Pour de nombreuses entreprises, il s'agit d'un outil indispensable. Cependant, ce n'est pas un outil 
sans risques.

Il convient ainsi de déterminer les menaces existantes pour un tel outil. Il est aussi nécessaire de
voir comment le Cloud s'insère dans les politiques existantes et les évolutions qu'il provoque sur 
la gouvernance et les processus. La question se pose aussi sur la mise en oeuvre d'une application
dans le Cloud, notamment pour définir une architecture qui épouse le Cloud tout en garantissant la 
sécurité.

# Risques de sécurité

Des risques ont été identifiés quant à l'usage du Cloud. Nous verrons deux grandes listes qui 
servent de base lors d'une analyse de risque d'une application Cloud.

## OWASP Cloud Top 10 2017

OWASP propose un [Top 10](https://www.owasp.org/index.php/Category:OWASP_Cloud_%E2%80%90_10_Project)
pour le Cloud.

1.  R1 - Accountability and Data Ownership

Le risque porte sur le contrôle de la donnée. Le fournisseur a la possibilité de lire les données 
non chiffrées (Confidentialité), modifier (Intégrité) voire d'en interdire l'accès en cas de conflit.
Le choix du Cloud publique pose une réelle question quant à la souverainté. De plus, la sécurité des
données est transférée en grande partie au fournisseur. Il convient de s'assurer de la mise en oeuvre
de bonnes pratiques via des certifications exposées, par exemple.

Plusieurs solutions sont possibles:
 - Mettre en oeuvre un Cloud privé (ex: [Azure Stack](https://azure.microsoft.com/fr-fr/overview/azure-stack/),
[AWS Outposts](https://aws.amazon.com/fr/outposts/)).
 - Chiffrer les données
 - Répliquer/Sauvegarder les données soit sur plusieurs fournisseurs Cloud, soit en interne
 - Vérifier la politique et l'historique du fournisseur sur ce sujet
 - Ne pas stocker les données les plus sensibles sur le Cloud

2. R2 - User Identity Federation

Plusieurs fournisseurs d'identité peuvent être utilisés pour authentifier un utilisateur. Pour les 
applications grand public, il n'est pas rare de voir des boutons comme "Connecter avec Facebook", Microsoft
ou Goole.

L'entreprise doit être en mesure de fédérer et de contrôler ces différentes identités.

3. R3- Regulatory Compliance

Les lois sont différentes entre les pays. L'exemple le plus flagrant est la gestion de la vie privée (privacy).
Entre l'Europe et RGPD et les Etats-Unis, le lien est compliqué. 
Quelle est la legislation qui s'applique alors? Celle du pays du fournisseur? De la localisation de son datacenter?
De l'entreprise qui utilise le Cloud? Celle de l'utilisateur?
Different regulatory laws across countries or regions

4. R4 - Business Continuity and Resiliency

Que se passe-t'il en cas de panne? Le Cloud n'est pas forcément fiable à 100%:
 - [Azure Status History](https://status.azure.com/en-us/status/history/)
 - [AWS Service Health](https://status.aws.amazon.com/)

Les pannes peuvent aller jusqu'à la [perte d'un datacenter](https://aws.amazon.com/message/41926/).
Il convient donc de concevoir l'application avec cet état de fait en tête.
Il faut aussi adapter le PRA/PCA au Cloud et les tester.
Un bon élève dans le domaine est [NetFlix et son armée de singes](https://github.com/Netflix/SimianArmy)
qui a automatisé le test de son plan de reprise.

5. R5 - User Privacy and Secondary Usage of Data

Le fait de s'appuyer sur le Cloud induit un tiers. Celui-ci peut, pour ses besoins, capter des informations
d'usage, notamment sur les utilisateurs de nos applications.

Il convient donc de s'assurer du respect de la vie privée affiché par le fournisseur.

6. R6 - Service and Data Integration

Les données sont stockées dans le Cloud. Pour des applications  interne (RH, Marketing...), ces informations
circulent sur Internet et peuvent être interceptées.

Une protection des données en transit doit être mis en place (VPN, HTTPS...).

7. R7 - Multi Tenancy and Physical Security

Le fournisseur de Cloud met à disposition des ressources partagées entre plusieurs clients (tenants).
Ce partage de ressources présente un risque sur la disponibilité (CPU, bande passante pour le réseau...)
mais aussi sur la confidentialité (attaque par canal caché).

L'isolation doit donc être validée.

8. R8 - Incidence Analysis and Forensic Support

Pour de nombreuses applications, les ressources utilisées sont gérées (managées) par le fournisseur.
Traditionnellement, les machines attaquées étaient isolées puis analysées pour comprendre l'attaque,
la contrer et potentiellement, identifier l'attaquant.

Avec le Cloud, il n'est pas forcément possible d'avoir un accès en direct. De plus, entre la 
détection et l'action, l'instance a pu être recyclée.

Les logs doivent être normalement déversées dans un "log sink" (AWS CloudWatch, Azure Monitor).
Dans une architecture micro-service, ils doivent aussi être conçus de manière à recréer l'historique
des évènements.

9. R9 - Infrastructure Security

Pour un Cloud type IaaS, les bonnes pratiques d'infrastructures doivent déployées:
 - Systèmes "sécurisés par défaut" et renforcés (hardened): configurations sécurisées à l'installation, désactivation des services inutiles, anti-virus, produits applicatifs à jour...
 - Bonnes pratiques d'Architecture réseau : isolation des environnements, filtrage et contrôle des flux, 
 - Contrôle des accès admin 

10. R10 - Non Production Environment Exposure

Afin de réduire la surface d'attaque, les environnements hors-production (développement, qualification...)
ne doivent pas être accessibles par tous.

## CSA Top Threat to Cloud Computing

La "Cloud Security Alliance" propose une liste: [The Egregious 11](https://cloudsecurityalliance.org/download/artifacts/top-threats-to-cloud-computing-egregious-eleven/).

1. Data Breaches
2. Misconfiguration and Inadequate Change Control
3. Lack of Cloud Security Architecture and Strategy
4. Insufficient Identity, Credential, Access and Key Management
5. Account Hijacking
6. Insider Threat
7. Insecure Interfaces and API
8. Weak Control Plane
9. Metastructure and Applistructure Failures
10. Limited Cloud Usage Visibility
11. Abuse and Nefarious Use of Cloud Services

## Analyse des différents Top 10

Ces deux listes mettent en avant plusieurs risques qu'il faut peser lors du choix d'un fournisseur.

 - Le Cloud permet de donner une bonne assurance sur la disponibilité. Cependant, le Cloud publique présente malgré tout un risque en cas de conflit.
 - Comment assurer la confidentialité et l'intégrité des données stockées dans le Cloud? Doit-on s'appuyer sur les solutions de chiffrement des données du Cloud ou utiliser ces propres outils?
 - Le contrôle d'accès est essentiel et doit être assurer par un processus cohérent.
 - Un mauvaise usage du Cloud entraine facilement une exposition inutile des services. Il faut contrôler la surface d'attaque qui est offerte.
 - La supervision doit fonctionner de manière efficace.

# Resources
 
 - ["Pentesting Azure Applications, The Definitive Guide to Testing and Securing Deployments"](https://nostarch.com/azure), Matt Burrough, No Starch Press, 2017
