---
title: Le Cloud?
tags: [cloud]
keywords: cloud, définitions
last_updated: July 3, 2016
summary: "Le Clooud?"
sidebar: appsec_sidebar
permalink: appsec_cloud_definitions
---

En 2006, Amazon lance son offre cloud avec **Amazon Web Services**. Le Cloud Computing est devenu un composant indispensable à toute entreprise, simplfiant la gestion des ressources et permettant de lancer un nouveau service en quelques clics. 

Le Cloud a facilité l'apparition de nouvelles architectures et de nouvelles pratiques permettant de l'exploiter à son maximum. Malgré tout, il faut adapter et maîtriser cet outil puissant.

# Qu'est-ce qui définit un "Cloud"?

Lorsque l'on s'intéresse à un "Cloud", plusieurs propriétés sont mise en avant:

 * On-demand/Self-service: l'obtention d'une ressource, que ce soit une machine, un produit applicatif comme un serveur Tomcat ou une base de données, est facile. Il n'est plus nécessaire de dépendre d'équipes en charge de la gestion des serveurs: plus de commandes des mois à l'avance en espérant que la capacité calculée est correcte.
 * Elastic: en cas de manque de puissance de calcul, il est possible d'ajouter de nouvelles machines. La scalabilité verticale est facilitée tant que certains contraintes sont respectées, notamment sur le partage d'un état entre machines. De même, l'ajout de CPU, de mémoire ou de disques peut se faire simplement en changeant une propriété. La scalabilité horizontale peut se faire en arrêtant la machine, modifier la propriété et redémarrer. Tout ça, en quelques instants sans dépendance.
 * Minimal management: L'exploitation est facilitée. Les machines peuvent être créées à l'identique, provisionnées ou décommissionner via le portail du fournisseur ou en ligne de commandes. De même, les actions classiques d'exploitation (sauvegarde, Plan de Reprise d'Activité/Plan de Continuité d'Activité (PRA/PCA), gestion des incidents, installations, supervision) sont allégées et facilitées par les services fournis par le Cloud.
 * Abstraction: Virtual Machine, Virtual Network, Function/Lambda, Service managé... L'utilisateur ne travaillera plus directement avec des machines ou des serveurs concrets. Il pourra même déployer son application sans savoir exactement sur quoi elle tourne.
 * Automation: Le Cloud offre de nombreux outils pour automatiser au maximum le déploiement: API, librairies dans un langage, CLI (client en ligne de commande). De plus, de nombreux outils s'interfacent avec les principaux fournisseurs pour offrir des services supplémentaires. On pourra citer [Terraform](https://www.terraform.io/), [Chef](https://www.chef.io/products/chef-infra/), [Ansible](https://www.ansible.com/), [Puppet](https://puppet.com/)
 * Shared: Les ressources peuvent être partagées. Ainsi, plusieurs machines virtuelles de différents clients peuvent co-exister sur la même machine.
 * Evolutionary: Grâce au Cloud, les architectures ne restent pas figées dans le temps. Elles sont malléables et facilement modifiables. Le lecteur intéressé pourra se référer à [https://www.thoughtworks.com/books/building-evolutionary-architectures](https://www.thoughtworks.com/books/building-evolutionary-architectures)

# Modèles de services

Quand on parle **Cloud**, on parle aussi du niveau de contrôle que l'on peut avoir sur les différents composants et notre responsabilité.

{% include image.html file="iaas-paas-serverless-saas.png" caption="Responsabilité/Contrôle selon le modèle (Source: https://dzone.com/articles/a-short-introduction-to-serverless-architecture)" %}

Définissons les modèles de services:

 * IaaS: Infrastructure as a Service. C'est le modèle offrant le plus de liberté. On définit précisement chaque élément constituant notre Système d'Informations.
 * CaaS: Container as a Service. Dans ce cas, l'application est déployé et géré par un orchestrateur. Les plus connus sont [Mesos](http://mesos.apache.org/), [Kubernetes](https://kubernetes.io/) ou [Docker Swarm](https://docs.docker.com/engine/swarm/). 
 * PaaS: Platform as a Service. Ce modèle abstrait les produits applicatifs sur lequel repose l'application (serveurs d'applications, base de données...). Il reste possible de choisir le type de serveur que l'on souhaite utiliser. On pensera à [Heroku](https://www.heroku.com), [Pivotal Cloud Foundry](https://pivotal.io/fr/platform) 
 * FaaS: Function as a Service. Ici, le développeur fournit juste le code. Ensuite, tout est géré par le fournisseur du service.
 * SaaS: Software a a Service. Plus de code, tout est géré.

Ces clouds peuvent être accédés de plusieurs manières:

 * Cloud publique: l'infrastructure est disponible pour tout le monde, via Internet.
 * Cloud privée: une entreprise installe et gère son cloud. 
 * Hybride: deux clouds, un publique et un privé, communique

A cela, on associe aussi la notion "On-premise". Une ressource "on-premise" est hébergée à l'extérieur d'un Cloud.

# Les applications face au Cloud

Le Cloud a changé la manière de concevoir les applications. Dans cette partie, nous nous intéresserons deux grands types d'applications qui "épousent" le Cloud:

 * Micro-services
 * Cloud-Native Applications

## Micro-services

Une application "micro-services" est une application découpée en une multitude de services faible couplé (il est possible de modifier un service sans impacté les autres) et fortement consistant (le service recouvre un domaine précis).

On oppose généralement "application micro-service" et "application monolithique". Historiquement, obtenir des serveurs étaient compliquées. De plus, les applications sont généralement imaginées comme couvrant un petit périmètre. Au cours de leur vie, ce périmètre augmente et l'application devient un géant difficile à maîtriser.

Il n'est pas nécessaire d'être dans un environnement Cloud pour construire une application micro-service.
Cependant, leur déploiement est généralement facilité puisqu'on peut avoir besoin d'allouer de nouvelles ressources en fonctions de l'évolution de l'architecture.

On peut prendre comme exemple les micro-services pour faire fonctionner la plateforme Amazon ou de Netflix: 

{% include image.html file="microservice-amazon-netflix.png" caption="Architecture micro-service d'Amazon ou de Netflix (Source: https://divante.com/blog/10-companies-that-implemented-the-microservice-architecture-and-paved-the-way-for-others/)" %}

## Cloud-Native Applications

La notion de "Cloud-Native Application" fait référence aux applications qui sont pensées pour fonctionner et exploiter au mieux le Cloud.

Ces applications utilisent généralement au maximum les services mangées. Elles sont naturellement scalables, facilement restaurables en cas d'incident. Du point de vue du déploiement, celui-ci se fait en continu (**Continuous Deployment**), sans intervention manuelle. Elles respectent aussi le [Twelve-Factor App](https://www.12factor.net/) proposé par Heroku.

# Le Cloud et la sécurité

Dans ce contexte, de nombreuses questions se posent:

 * Comment assurer la sécurité quand l'accès aux ressources semble si facile? Quelles règles définir par rapport aux aspects légales (Compliance) et à la politique définie dans mon entreprise?
 * Comment assurer la sécurité d'une application déployée en continu, sans pentest?
 * Quels risques représentent le Cloud pour mon organisation?
 * Comment construire une application sécurisée?

En même temps, il est aussi important de voir les apports du Cloud:

 * La gestion facilité. Cela veut-il dire que la sécurité est gérée par le fournisseur? De quoi dois-je me préoccuper?
 * Des services avancées. Existe-t'il des services à valeur ajoutée pour la sécurité?
 * Est-ce que les nouvelles approches offertes par le Cloud peut améliorer la sécurité des applications?
