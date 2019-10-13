---
title: Le Cloud, nouvelles opportunités
tags: [cloud]
keywords: cloud, opportunités
last_updated: July 3, 2016
summary: "Le Cloud, nouvelles opportunités"
sidebar: appsec_sidebar
permalink: appsec_cloud_opportunités
---

Le Cloud offre de nouvelles possibilités mais aussi de nouvelles solutions.
Sécurité par design, nouveaux outils, nouvelles approches...

# Les outils du Cloud

Les outils fournis par les différents fournisseurs de Cloud varient.
On peut cepedant retrouver un certain nombre de points communs
utiles pour la sécurité.

## Des solutions pré-packagés, sécurisés par défaut

Le premier aspect du Cloud est qu'il offre des solutions avec un ensemble de 
services assurant un bon niveau de sécurité.

Dans le cas de machines virtuels pour le IaaS, le fournisseur propose généralement des versions à jour. 
Pour les services managés type PaaS, le fournisseur gère les versions et leur mise à jour.
Il assure la sécurité et la bonne configuration des services.

De plus, le fournisseur propose de nombreuses services à haute valeur ajoutée pour la sécurité, 
disponibles en quelques clics. S'il ne les gère pas, le fournisseur dispose d'une *"MarketPlace*"
qui lui permet de vendre ces solutions en tant qu'intermédiaire. Par exemple:

 - Pare-feau applicatif (ou WAF) ([AWS WAF](https://aws.amazon.com/waf/), [Azure Application Gateway avec WAF](https://docs.microsoft.com/en-us/azure/application-gateway/waf-overview), [GCP via des parternaires](https://cloud.google.com/security/partners/)...)
 - Vault
 - Security Operation Center ([Azure Sentinel](https://azure.microsoft.com/en-gb/services/azure-sentinel/)...)

## La sécurité intégrée

La sécurité est aussi nativement intégrée dans le Cloud. 
Monitoring, supervision, solutions d'exploitation...

Le pilotage de la sécurité est aussi au centre de ces outils. 
Deux grands besoins existent:
 - Mettre en oeuvre les politiques de sécurité dans le Cloud
 - S'assurer de leur déploiement

Les fournisseurs de Cloud ont créé des solutions qui permettent de détecter ou d'empêcher
la non-conformité avec les politiques. Ainsi, il est possible d'avertir ou de bloquer le 
déploiement d'un nouveau service ne respectant pas la politique de sécurité.
Exemple de solutions: [AWS Organizations](https://aws.amazon.com/organizations/), [Azure Policy](https://azure.microsoft.com/services/azure-policy/)

## Le déploiement industrialisé

## Le maintien en condition opérationelle (MCO) et de sécurité (MCS) facilité

Le MCO "*désigne la stratégie de prévention mise en oeuvre pour garantir la disponibilité de votre infrastructure et de vos applications*" ([Définition](https://www.intersed.fr/infogerance/maintien-en-condition-operationnelle)). Le MCS est son pendant pour la stratégie permettant de s'assurer que la sécurité ne se dégrade pas avec le temps.

Une fois déployé, l'application doit pouvoir être gardé au niveau attendu en termes d'exploitation et de sécurité.
Il convient donc de s'interroger sur:
 - Que se passe-t'il le jour où un datacenter s'arrête: il faut construire un PRA/PCA validé et testé
 - Quid si des données sont effacées (par erreur ou à cause d'une attaque)? La retour dans un état stable passera sans doute par la restauration de la dernière sauvegarde non corrompue. La stratégie de sauvegarde doit donc être définie.
 - Que faire en cas d'augmentation du nombre d'utilisateurs ou d'une attaque DDoS? La stratégie de réplication des services et données doit être déployée.
 - Une des dépendances présentes un problème. La gestion des patchs et mises à jour est à faire.
 - Est-ce que tout va bien sur mon appliction? Supervision et alarming.

Nous avons déjà vu que les fournisseurs de Cloud disposent déjà de nombreux services "prêt-à-l'emploi":
 - Sauvegarde géo-répliquée
 - Scalabilité verticale et horizontale
 - Services maangées automatiquement patchés
 - Solutions de monitoring, d'alarming et d'actions automatiques

Il convient alors d'avoir architecturé et développé correctement son application pour s'assurer de
profiter au mieux de ces options.

# Nouvelles approches pour la sécurité

Outre les services et les produits des fournisseurs, le Cloud repense la manière de construire et penser
les applications. Besoin d'un serveur supplémentaire? Plus besoin d'attendre 6 mois avant de pouvoir 
déployer en développement (Véridique!). Besoin de patcher? Facile! ...

Nous allons voir comment certaines propriétés d'un Cloud aide à améliorer la sécurité d'une application.

## Facilité de création

Créer un serveur dans le cloud est chose aisée. 

Avec [Azure](https://docs.microsoft.com/bs-latn-ba/azure/virtual-machines/windows/quick-create-cli):

    az vm create \
      --resource-group myResourceGroup \
      --name myVM \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password myPassword

Avec [AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html):

    $ aws ec2 run-instances --image-id ami-xxxxxxxx \
         --count 1 \
         --instance-type t2.micro \
         --security-group-ids sg-903004f8 \
         --subnet-id subnet-6e7f829e

Ainsi, il devient facile d'imaginer de commander des resources spécifiques par service.
Cela emmène vers des approches micro-service. Si bien implémenté, nous pouvons obtenir 
un très bon niveau en terms d'isolation et de contrôle d'accès.

## Facilité de destruction

Il est facile de détruire, il est tout aussi facile de détruire de l'arrêter, de la déplacer 
ou de la détruire.

Pour un attaquant, une fois une faille trouvée, il lui est nécessaire de "s'installer" 
([Persister](https://attack.mitre.org/tactics/TA0003/)) sur les serveurs auxquels il a
accès. 
En "recyclant"/reconstruisant régulièrement les instances, nous pouvons détruire cette
présence. L'attaquant devra alors recommencer son processus, le ralentissant dans son
action.

## Facilité de re-création

Le besoin de scalabilité oblige les concepteurs d'application à ne plus gérer les états au 
niveau des serveurs d'application. On souhaite rendre les [serveurs immutables](https://martinfowler.com/bliki/ImmutableServer.html).
Grâce à cela, il est facile d'ajouter ou de retirer des serveurs sans avoir besoin de 
dupliquer ou de propager un quelconque état partagé. De plus, niveau sécurité, cela rend
impossible la tâche de persisntance pour un attaquant qui ne peut plus que s'installer en 
mémoire.


## The Three R's of Enterprise Security by Justin Smith

Justin Smith résume ainsi cette approche avec la vision **["Cloud-Native Enterprise Security](https://pivotal.io/it/cloud-native-security)** qui s'oppose aux phases plus classiques de la gestion d'incident ([Containment, Eradication, Recovery](https://www.securitymetrics.com/blog/6-phases-incident-response-plan)):

 - Rotate datacenter credentials every few minutes or hours. 
 - Repave every server and application in the datacenter every few hours from a known good state. 
 - Repair vulnerable operating systems and application stacks consistently within hours of patch availability


