---
title: Risques et menaces
tags: [formatting]
keywords: threat, risk
last_updated: October 30, 2018
summary: ""
sidebar: appsec_sidebar
permalink: appsec_threat_and_risk
---

# Menaces

## Définition

### Vulnérabilité

Une vulnérabilité est une faiblesse ou un défaut dans un logiciel qui est exploitée par un attaquant pour 
avoir accès à un actif.
Cette faiblesse peut être:

 - d'un bug: une entrée mal protégée devient le point d'entrée d'une attaque. Il s'agit d'une erreur introduite au moment du développement, par exemple
 - d'une fonctionnalité: même si elle est utile, peut être détournée et utilisée à l'avantage d'un attaque. C'est un problème de conception
 - d'une faille volontairement créée: on parlera alors de **backdoor** (porte dérobée)

Il est important de bien identifier et gérer les vulnérabilités. Il faut prioriser leurs corrections en fonction de leur gravité et de leur impact (mauvaise publicité). De plus, il faut noter que l'on retrouve souvent le même type d’erreur dans un contexte donné. Avec un suivi efficace des vulnérabilités, les failles les plus récurrentes pourront donner lieu à une analyse des causes racines et à un traitement particulier.
Par exemple, s'il s'agit d'une faille de type "SQL Injection", une formation sur le bon usage des requêtes préparées (en: Prepared Statements) s'avérera bénéfique.

### Attaque

Une attaque est l'action d’utiliser une ou plusieures vulnérabilités pour obtenir un actif.

### Exploit

Un exploit est un logiciel exploitant une vulnérabilité.

Un outil, [Metasploit](https://www.metasploit.com/), tire son nom de ce mot (Meta + exploit).

### Menaces

Une définition de menace (en: **Threat**) : "A **person** or **thing** likely to cause **damage** or danger." [Oxford Dictionnary](https://en.oxforddictionaries.com/definition/threat). Une menace est donc un attaquant ou un logiciel (comme un ver) qui utilisera une vulnérabilité pour causer du dommage.

Certaines attaques peuvent s'étaler sur plusieures années. Par exemple, les **Advanced Persistent Threat** (APT) sont des attaques furtives, continues et présentant une niveau de sophistications importants orchestrées par des organisations.

Ressources:
 - "Sécurité et espionnage informatique", Cédric Pernet
 - MISC Magazine a publié de nombreux articles sur le sujet ([MISC n°79](https://www.miscmag.com/le-point-sur-les-apt-dans-misc-n79/), [ici](https://www.miscmag.com/ledito-de-misc-hs-n10-apte-ou-in-apt-face-aux-a-p-t-a-vous-de-voir/))

## "Connais ton ennemi et connais-toi toi-même" - Sun Tzu

Les typologies d'acteur dans le domaine de la sécurité (en: **Threat Actor Typology**) ont pour but de
définir qui peut nous attaquer. Derrière chaque adversaire se cache des motivations. Les comprendre nous 
permet de mieux anticiper leurs actions et de proposer une riposte adaptée.

Chaque acteur a des motivations, mais il a aussi des moyens matériels à sa disposition: notamment tous n'ont
pas accès au même volume de temps, d'argent et de connaissances.
La liste suivante est un exemple de typologie:

| Acteurs | Motivations | Temps | Argent | Connaissances
| --- | --- | --- | --- | --- |
| Criminels | extortions (ransomware), vol (transactions bancaires), fraude, scam, … | Peu | Peu | Moyen
| Hacktivists  | hack + activists | Beaucoup | Peu | Beaucoup
| Script kiddies | Jeu | Beaucoup | Peu | Peu
| Terrorists | Actions politiques | Beaucoup | Moyen | Moyen à Beaucoup
| Acteurs institutionnels (état, entreprise) | Défenses des intérêts nationaux | Beaucoup | Beaucoup | Beaucoup
| Ancien employé | Vengeance | Peu | Peu | Peu
| Chercheurs | Recherche | Beaucoup | Peu | Beaucoup

Une fois une typologie choisie, la question se pose de savoir si chacun de ces acteurs peut m'attaquer.

Supposons j'ai une faiblesse dans une solution de chiffrement (par exemple une clé faible). Un État a les moyens
de mettre en oeuvre des solutions pour casser la clé via une attaque de type "[brute-force](https://en.wikipedia.org/wiki/Brute-force_attack)".

Si je suis un petite entreprise, je n'ai pas forcément à m'inquiéter d'être pirater par un État comme par exemple 
(au hasard) la Corée du Nord ou les États-Unis. Par contre, si je suis une grosse entreprise, il s'agit d'une menace
sérieuse. Cela rentre dans la guerre économique.

Ressources:

 - [Exemple de typologie](https://www.wodc.nl/binaries/2740_Summary_tcm28-273244.pdf)

### Threat intelligence

La **Threat intelligence** cherche à collecter des informations concernant les menaces sur la sécurité informatique.
Elle s'intéresse notamment à :

 - Tactiques : méthodologies, outils, …
 - Techniques
 - Opérations : analyse des organisations
 - Stratégique : but et plan d’attaques
 - Advanced Persistent Threat
 - Collectes d’informations (HoneyPot, analyse Virus, …)

# Risques

## Définition

Le risque est la probabilité d'un dommage sur un actif suite à l'exploitation d'une vulnérabilité par une menace.Pour avoir un risque, il faut une menace et une vulnérabilité. On pourra définir le risque avec:

    Risque : facteur x probabilité ⇒ conséquences

On associera alors :
 - facteur : vulnérabilité et menace
 - probabilité : visibilité, complexité, capacité des attaquants
 - conséquences : impact technique ou métier de la réalisation du risque (asset)

Du côté de l'attaquant, le risque peut-être vu comme suit (extrait de _The Security Development Lifecycle: SDL: A Process for Developing Demonstrably More Secure Software_ par Michael Howard & Steve Lipner):

> The cost-benefit ratio for a criminal is defined by Clark and Davis (Clark and Davis 1995) as
> Mb + Pb > Ocp + OcmPaPc
> where
>  * Mb is the monetary benefit for the attacker.
>  * Pb is the psychological benefit for the attacker.
>  * Ocp is the cost of committing the crime.
>  * Ocm is the monetary costs of conviction for the attacker (future lost opportunities and legal costs).
>  * Pa is the probability of being apprehended and arrested.
>  * Pc is the probability of conviction for the attacker.

### Threat analysis


## Gestion du risque

Une activité récurrent au cours de la vie d'un projet est la **gestion des risques**. Pendant cette activité,
les risques sont identifier, "mesurer" puis prioriser. En fonction des critères, on pourra choisir d'agir sur les risques les plus importants. On parlera alors de **pilotage par les risques**.

Plusieurs étapes sont présentes lors de ce travail:

 # Identifier les actifs
 # Identifier les menaces
 # Évaluer les faiblesses sur les actifs
 # Évaluer les risques
 # Gérer les risques

L'évaluation se fait souvent de manière qualitative. La probabilité, bien que normalement compris entre 0 et 1, peut être évaluer avec une variable ordinale (par exemple: faible, moyen, élevé, très élevé). De même, la conséquence, souvent financière, peut être associée à une échelle (par exemple: 1 (impact faible - 1 €) à 10 (impact important: fermeture de l'entreprise, mort, ...)).

Face à un risque, plusieurs solutions sont possibles:
 - **Éviter**: on va éliminer, abandonner ou faire en sorte de ne pas le rencontrer. On va part exemple corriger la vulnérabilité.
 - **Réduire**: certaines failles peuvent être compliquées ou chères à corriger. On peut décider d'atténuer le problème. Par exemple, si l'application web présente de nombreuses failles de type XSS, on pourrait imaginer de mettre en oeuvre un [WAF](https://fr.wikipedia.org/wiki/Web_application_firewall) (attention, ce n'est pas la panacée!).
 - **Partager**: on va alors transférer le risque vers un tiers. Par exemple, de nombreuses entreprises sous-traitent (outsourcing) le développement de leur application à d'autres entreprises. L'entreprise donne donc à l'entreprise sous-traitant de fournir un logiciel "sécurisé". Un autre solution est de prévoir une assurance.
 - **Prendre en charge**: enfin, dernière possibilité: accepter le risque et le budgétiser, c'est-à-dire prévoir de la contingence pour le cas où le risque se produirait.

Dans tous les cas, un équilibre "Coût / Conséquence" est à considérer. En effet, un risque peut problème et sans conséquence mérite-t'il d'être corrigé?

On verra, par la suite, des outils évalués des menaces dans le cadre du **Threat modeling**.

## Limites

Plusieures limites existent concernant la sécurité d'un système. Ainsi, une application est:

 - Sécurisé à un moment: de nouvelles failles apparaissent ou certaines failles deviennent plus courantes.
 - Sécurisé à chaque niveau: chaque niveau doit présenté un niveau de sécurité suffisant. Il ne faut pas de maillon faible. Il faut aussi remarquer ici l'asymétrie attaque/défense: l'attaquant peut attaquer où il le souhaite et il suffit d'une faiblesse. La défense doit donc être totale et permanent.
 - Sécurisé globalement: la sécurité doit s'envisager d'un point de vue holistique, en prenant en compte l'ensemble du système. Cependant, les systèmes sont souvent complexes et difficiles à analyser dans la totalité.


