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
 - Bonnes pratiques d'Architecture réseau : isolation des environnements, filtrage et contrôle des flux...
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
 - Un mauvaise usage du Cloud entraîne facilement une exposition inutile des services. Il faut contrôler la surface d'attaque qui est offerte.
 - La supervision doit fonctionner de manière efficace.


# Architecture pour le Cloud

Pour définir une architecture pour le Cloud, il est bon d'avoir des principes pour nous guider.
Nous allons nous intéresser aux principes de design pour construire une application sécurisée.
Ces principes possèdent plusieurs propriétés intéressantes:

 * Ils sont peu nombreux (< 20)
 * Ils sont simples à comprendre et à retenir
 * Ce sont des principes de haut-niveau, indépendant de l'implémentation
 * Ils adressent des problèmes fondamentaux dans la sécurité
 * Ils sont “fractales”, c'est-à-dire qu'on peut les retrouver et les appliquer à tous les niveaux, de la conception en passant par l'architecture jusqu'au code.


Puis, nous verrons les solutions offertes par le Cloud et comment traduire nos choix en solutions concrètes

## Secure by Design

En 1975,  Jerome Saltzer et Michael Schroeder ont proposé dans leur article intitulé *"The Protection of Information in Computer Systems"* plusieurs principes de design pour construire une application sécurisée:

| Principes                       | Description | Applications au niveau de l'architecture
| ---                             |             |                                          
| **Economy of mechanism**        | Un design simple est facile à tester et à valider. Moins il y a, plus la surface d'attaque est réduite. On pourrait rapprocher ce principe de [KISS](https://en.wikipedia.org/wiki/KISS_principle). | Réduire les serveurs au minimum,
| **Fail-safe defaults**          | Par défaut, en cas d'erreur, le système ne doit pas se retrouver dans un état non sécurisé. La gestion des d'erreurs est un point important.
| **Complete mediation**          | L'authenticité (et l'authentification), l'intégrité et l'autorisation doivent être vérifiées à chaque fois qu'une requête est effectuée. Les droits d'accès sont entièrement revalidés à chaque accès. Ce genre de principe nous prémunit de failles telles que [IODR/Broken Object Level Authorization](https://github.com/OWASP/API-Security/raw/develop/2019/en/dist/owasp-api-security-top-10.pdf)
| **Open design**                 | L'obfuscation seule ne sert à rien. Le design doit pouvoir être revu et justifié. Cela rejoint le [deuxième principe de Kerchoffs](https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle#Origins). | Le schéma d'architecture ou les fichiers servant à l'Infra as Code peuvent être publiés sans risque.
| **Separation of privilege**     | Le principe est d'empêcher qu'un décision prise soit validée par la même personne. Cela permet de renforcer la fiabilité d'un système en ayant un contrôle sur les actions d’un seul individu. Le principe est renommé en ["Separation of duties" ou "Segregation of duties"](https://en.wikipedia.org/wiki/Separation_of_duties). | Définition de niveaux d'isolation (tenant, VNet, subnet, ...), Gestion des droits d'accès, ...
| **Least privilege**             | est-ce qu’un simple utilisateur devrait avoir les droits d'administration pour se connecter à une application? A-t'on besoin de faire tourner une application en root sur un serveur? A-t'on besoin de travailler en tant qu'administrateur sur son poste? | 
| **Least common mechanism**      | Les utilisateurs ne devraient pas partager les mêmes outils, sauf si c’est indispensable. Ex: même câble pour le réseau, même PC, … (accès à des fichiers partagés, …). | Par conception, ce principe est mis à mal par le Cloud où les ressources sont fortement partagées. On cherchera à ne pas partager des ressources comme des VMs entre projets.
| **Psychological acceptability** | Si la sécurité est trop compliquée, les utilisateurs trouveront un moyen de la contourner. | Si l'accès au Cloud est trop compliqué, on s'expose à ce que les utilisateurs face du ["Shadow IT"](https://en.wikipedia.org/wiki/Shadow_IT)
| **Work factor**                 | L'effort à fournir par l’attaquant pour rentrer dans le système doit être important. Gros mots de passe, … | 
| **Compromise recording**        | Le système devrait conserver une trace de toutes les attaques même si elles n'ont pas été bloquées. Indispensable pour l'analyse d'incident. | Utilisation de solutions "Log Sink" telles que Azure Monitor ou AWS CloudWatch.

En 2009, Saltzer et Kaashoek ont complété cette liste dans [*"Principles of Computer System Design"*](https://ocw.mit.edu/resources/res-6-004-principles-of-computer-system-design-an-introduction-spring-2009/online-textbook/principles_open_5_0.pdf) avec d'autres principes:

| Principes                          | Description | Applications au niveau de l'architecture 
| ---                                |             |
| **Minimize secrets**               | Parce qu'il y a une chance que le secret puisse être découvert, en changer doit être facile. Là encore, on rejoint le troisième principe de Kerchoff (*"correspondents must be able to change or modify [the key] at will"*). On parle alors de rotation des secrets | PKI as a Service, Vault, [Store config in environment](https://www.12factor.net/config)
| Least astonishment                 | Ne réinventez pas la roue. Vos choix doivent être en cohérence avec l'état de l'art, les attentes et l'expérience des utilisateurs, ... | Il est important de s'appuyer sur des guides d'architecture pour constuire son système ([Azure](https://docs.microsoft.com/fr-fr/azure/architecture/guide/), [AWS](https://aws.amazon.com/fr/architecture/), [GCP](https://www.paloaltonetworks.com/resources/guides/gcp-architecture-guide) )
| Design for iteration               | L'architecture ne sera sans doute pas parfaite du premier coup, concevez-là en prenant en compte ce principe pour faire en sorte que les changements soient faciles | L'amélioration continue appliquée à l'architecture. On s'interessera aux architectures micro-services ou aux notions ["evolutionary architectures"](https://www.thoughtworks.com/books/building-evolutionary-architectures).

D'autres principes importants peuvent aussi être gardés en mémoire:

| Principes                          | Description | Applications au niveau de l'architecture
| ---                                |             |
| **Defense in depth**               | C'est un principe de sécurité important. Plusieurs couches se compensent et se complètent les unes les autres. | Sur une architecture, on mettre en place deux firewalls de constructeurs différents, un WAF, une isolation avec des sous-réseaux, une validation des failles avec un sAST...
| **Deny by default**                | Par défaut, toute actions est refusée. On ouvre les droits de manière parcimonieuse | Toutes les API doivent être sécurisées

L'OWASP propose aussi une liste complète de [10 principes pour la sécurité par design](https://www.owasp.org/index.php/Security_by_Design_Principles). On retiendra:
| Principes                          | Description | Applications au niveau de l'architecture
| ---                                |             |
| **Minimize attack surface**        | Réduire la surface que l'attaquant peut utiliser au minimum | Fermeture des ports, renforcement des machines, ...
| **Fix security issues correctly**  | Tout problème doit être rapidement corrigé | Mettre en place des systèmes de détection telles que [Nessus](https://en.wikipedia.org/wiki/Nessus_(software)) ou [OpenVAS](http://www.openvas.org/)

## Exemples de solution avec Azure

La liste suivante est non exhaustive et soumise aux changements rapides du Cloud.
Elle est organisée selon les différentes couches du modèle OSI:

 - Level 3: NSG, IP Filtering (App Service), VPN, Bastion, Network Watcher
 - Level 4: NSG, NSG DDoS Protection, Firewall
 - Level 5: Application Gateway TLS
 - Level 7: APG WAF, Advanced SQL Threat detection

## Exemple d'architecture

Le schéma suivant est tiré de ["Implement a DMZ between Azure and the Internet"](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).

{% include image.html file="azure-architecture-dmz.png" caption="Schéma d'architecture" %}

On retrouve ainsi:

 - La présence de plusieurs zones pour isoler et limiter l'impact en cas de compromission.
 - Les flux sont clairement indiqués. Il est possible de limiter les accès via des subnets ou les NSG.


# Resources
 
 - ["Pentesting Azure Applications, The Definitive Guide to Testing and Securing Deployments"](https://nostarch.com/azure), Matt Burrough, No Starch Press, 2017
