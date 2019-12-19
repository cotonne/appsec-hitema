---
title: OWASP Top 10
tags: [web]
keywords: web, rappels
last_updated: July 3, 2016
summary: "OWASP Top 10"
sidebar: appsec_sidebar
permalink: appsec_web_top10_owasp
---

La fondation [OWASP](https://www.owasp.org/index.php/Main_Page) (pour Open Web Application Security Project) 
est une organisation qui a pour but d'aider à améliorer la sécurité des applications WEB. 
Aujourd'hui, elle propose des recommandations sur de nombreux sujets ainsi que des outils pour sécuriser 
les applications.

L'un des projets phare de l'OWASP est le TOP 10. Il en existe de nombreuses déclinaisons, le plus connu
étant le [TOP 10 WEB](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). 
Celui-ci recense les menaces les plus courantes sur les applications web. L'un des buts de ce classement
est d'aider à la priorisation des tests de sécurité.

Le tableau suivant présente le classement depuis 2007:


| 2007                                                 | 2010                                            | 2013                                            | 2017                                           |
|------------------------------------------------------|-------------------------------------------------|-------------------------------------------------|------------------------------------------------|
| A1 - XSS                                             | A1-Injection                                    | A1-Injection                                    | A1-Injection                                   |
| A2 - Injection flaws                                 | A2-Cross Site Scripting (XSS)                   | A2-Broken Authentication and Session Management | A2-Broken Authentication                       |
| A3 - Malicious File Execution                        | A3-Broken Authentication and Session Management | A3-Cross-Site Scripting (XSS)                   | A3-Sensitive Data Exposure                     |
| A4 - Insecure Direct Object Reference                | A4-Insecure Direct Object References            | A4-Insecure Direct Object References            | A4-XML External Entities                       |
| A5 - CSRF                                            | A5-Cross Site Request Forgery (CSRF)            | A5-Security Misconfiguration                    | A5-Broken Access Control                       |
| A6 - Information Leakage and Improper Error Handling | A6-Security Misconfiguration                    | A6-Sensitive Data Exposure                      | A6-Security Misconfiguration                   |
| A7 - Broken Authentication and Session Management    | A7-Insecure Cryptographic Storage               | A7-Missing Function Level Access Control        | A7-XSS                                         |
| A8 - Insecure Cryptographic Storage                  | A8-Failure to Restrict URL Access               | A8-Cross-Site Request Forgery (CSRF)            | A8-Insecure Deserialization                    |
| A9 - Insecure Communications                         | A9-Insufficient Transport Layer Protection      | A9-Using Components with Known Vulnerabilities  | A9-Using Components with Known Vulnerabilities |
| A10 - Failure to Restrict URL Access                 | A10-Unvalidated Redirects and Forwards          | A10-Unvalidated Redirects and Forwards          | A10-Insufficient Logging & Monitoring          |


Plusieurs constatations:
 - Depuis 2010, la première menace est l'"Injection". Il s'agit d'un terme assez large comme nous le verrons par la suite
 - XSS est passé de la premère à la septième place. Cela est principalement due à l'utilisation de frameworks web rendant plus difficile ce type de vulnérabilité.
 - Certaines menaces sont renommées, fusionnées ou disparaissent du classement. Par exemple, 2013-A4 et 2013-A7 ont été fusionnées en 2017-A4

## 2017-A1 Injection

### Présentation

Le principe de l'injection est le fait qu'une application utilise des données non sûres qui entraine une 
modification de son comportement. 

Trois éléments sont nécessaires:
 - Une source de données non sûres est utilisée
 - Un interpréteur qui va se servir des données
 - Un système intermédiaire qui transmet les données à l'interpréteur

L'attaquant envoie des données construites de façon à modifier le comportement de l'interpréteur.
Le problème est que le système intermédiaire ne fait aucune vérification et que les données se 
retrouvent contenir un programme.

Prenons un exemple courant: l'injection SQL
 - L'attaquant inclut dans la source de données des commandes SQL qui seront interprétées par le serveur SQL
 - L'interpréteur est le serveur SQL
 - Le système intermédiaire peut être un programme écrit en python, java, C#... qui transmet les données et appuie ces décisions sur ce que renvoie l'interpréteur.

Prenons l'exemple suivant. Imaginons que l'application expose une URL comme:

> http://host/path?username=john&password=pass

L'application est developpée en Java et utilise une base de données SQL

```java
public class userCheck extends HttpServlet {
	@Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
		String query = "SELECT * FROM users WHERE " +
	             "username = '" + request.getParameter("username") + 
	             "' AND password='" + request.getParameter("password") + "'";
		try {
		    Statement statement = connection.createStatement( ... );
		    ResultSet results = statement.executeQuery( query );
                    if(results.first() {
                       // return secret information
                    }
	    	}
	}
}
```

Normalement, la requête qui envoyée au serveur SQL est:

> SELECT * FROM users  where username='john' and password='pass'

Si la table **users** contient une ligne avec une utilisateur appellé **john** dont le mot de passe
est **pass**, alors la ligne est retournée à l'application Java qui renvoie les informations secrètes.
Mais, que se passe-t'il si un attaquant accède à l'application avec l'URL suivante:

> http://host/path?username=john&password=mauvais_password'%20or%20'1'%3D'1

L'application Java envoie alors la requête SQL suivante à la base:

> SELECT * FROM  users where username='john' and password='mauvais_password' or '1'='1'

Décomposons un peu le résultat. En SQL :
 - **'1'='1'** est toujours vrai
 - Supposons que l'utilisateur **john** existe, **username='john'** est alors vrai
 - On peut supposer que mauvais_password n'est pas valide, donc **password='mauvais_password'**

On a alors la requête suivante:

> SELECT * FROM users WHERE (TRUE AND FALSE) OR TRUE

> SELECT * FROM users WHERE FALSE OR TRUE

> SELECT * FROM users WHERE TRUE

> SELECT * FROM users

La requête finale exécutée par le serveur SQL revient à renvoyer toute la table users.
Or, dans le programme Java, il suffit qu'au moins une ligne soit renvoyée pour que
la partie "secret information" s'exécute.
Sans connaître le mot de passe de l'utilisateur, il a été possible d'obtenir les informations.

Bien utilisée, une injection SQL peut permettre de récupérer l'ensemble du contenu de la base.

Comme indiqué, il est possible de remplacer la partie "interpréteur". On retrouve ainsi les injections suivantes:

 - Injection LDAP
 - Injection NoSQL
 - Injection de commandes
 - ...

### Solutions

Pour s'en protéger, il existe plusieurs solutions:
 - Échappement: les caractères sensibles, c'est-à-dire qui pourrait être traitées par l'interpréteur, doivent être rendus inopérants par l'application intermédiaire. Ainsi, la simple quote (') vue dans l'exemple précédent devient \'.
 - Sanitization/Purification: ici, les caractères sensibles sont tout simplement supprimées
 - Validation: on vérifie le format du contenu. Ainsi, est-ce normal que le nom contient le signe égal? A noter que la validation est moins facile pour le mot de passe pour lequel on souhaite garder une forte entropie. Se reporter à la partie [Secure Coding]() pour voir comment valider correctement une donnée
 - Prepared Statements: cette solution ne marche que si l'interpréteur est une base SQL. On pré-construit la requête via une [Prepared Statement)[https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html].
 - Dans une approche "défense-en-profondeur", il est aussi possible de réduire les droits du compte utilisé pour interagir avec l'interpréteur. Dans le cas du serveur SQL, inutile que l'utilisateur est accès aux tables système par exemple. Si une exécution de commandes doit être faite, on peut utiliser des solutions comme Docker, chroot, SELinux,... pour isoler l'exécution de la commande.

### Outils

Des outils existent pour détecter ce type de problèmes.

Via l'analyse statique de code, des outils comme [Sonar](https://rules.sonarsource.com/java/tag/SonarSecurity/RSPEC-3649), [Checkmarx](https://www.checkmarx.com/knowledge/knowledgebase/SQLi), [LGTM](https://lgtm.com/rules/3970084/), Veracode, Coverity... pour n'en citer quelqu'uns détectent la présence de telles failles.

Il est aussi possible de découvrir ces vulnérabilités via une approche dynamique, On pourra citer les outils suivants:
 - [SQLMap](http://sqlmap.org/)
 - [NoSQLMap](https://github.com/codingo/NoSQLMap)
 - [ZAProxy](https://medium.com/@cmpbilge/beginners-guide-to-sql-injection-sqlmap-owasp-zap-d1e6b1f97c3d)


## A2-Broken Authentication

### Présentation

Beaucoup d'applications mettent en oeuvre des solutions d'authentification et d'autorisation.
Ces mécanismes sont indispensables pour assurer les propriétés de confidentialité et d'intégrité des données.

Si un attaquant arrive à outrepasser ou à détourner la phase d'authentification, il est impossible
d'assurer que nous savons qui accède à notre système. La menace, représentée par 
[2017-A2 Broken Authentication](https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication)
est que le le processus d’authentification ne joue pas son rôle.

Différents scénarios sont possibles pour :
 - L'authentification a mal été configurée ou mal implémentée.
 - Si un attaquant possède l'identifiant d'un utilisateur, il peut tenter de se connecter en essayant de deviner son mot de passe de l'utilisateur.
 # L'attaquant teste de nombreux mots de passe différents sur l'interface de connexion. On parle alors de mode "online". Le mot de passe peut-être extrèmement simple voire très courant. L'attaquant pourra alors s'appuyer sur des listes comme les [10 millions de mots de passe les plus courants](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10-million-password-list-top-100000.txt)
 # L'attaquant a réussi à récupérer le mot de passe hashé dans la table des utilisateurs. Il tente alors de le retrouver via un mode "offline" ou hors-ligne.
 - L'attaquant a volé l'identifiant de session. L'utilisateur a pu ne pas se déconnecter sur un ordinateur public. L'application peut aussi transmettre l'[identifiant de session dans l'URL](https://www.php.net/manual/fr/session.idpassing.php). Si un attaquant récupère l'URL sur un forum par exemple, il pourrait utiliser l'identifiant pour voler la session.

### Solutions

Les exemples de scénarios nous aident à imaginer des solutions simples:
 - Règles de mots de passe: il est courant d'avoir des règles comme 'longueur du mot de passe supérieur à 8 caractères'. Le but de ce type de règles est de faire en sorte que l'attaquant doive essayer un grand nombre de mot de passe. Le nombre d'essais pour trouver le mot de passe en essayant, même de façon aléatoire, les combinaisons correspond à la "[force du mot de passe](https://en.wikipedia.org/wiki/Password_strength)" ou d'entropie (une mesure de l'aspect aléatoire du mot de passe). Des [libraries](https://github.com/jreesuk/entropizer) peuvent calculer cette valeur. Généralement, un mot de passe est considéré comme sûr quand son entropie est autour de 29 bits dans le cas d'attaque "online" et de 96 bits pour des attaques "offline" (cf. [Wikipedia Password Strength](https://en.wikipedia.org/wiki/Password_strength#Required_bits_of_entropy))
 - Ré-authentification sur les actions sensibles: si la session a été "volée", l'attaquant ne connaît pas forcément le mot de passe de l'utilisateur. Pour toute action sensible, il est pertinent de redemander le mot de passe de l'utilisateur.
 - Multi-Factor Authentication: il est intéressant d'implémenter plusieurs solutions pour réussir l'authentification. C'est le but du MFA. Ainsi, si le mot de passe est compromis, il semble peu probable que l'attaquant réussisse aussi à compromettre le téléphone.
 - Respecter les guidelines pour les sessions: ne pas exposer l'identifiant de session, limiter la durée de validité, gérer correctement côté serveur...
 - Détecter les comportement suspects: il faut limiter le nombre de tentatives, en fonction de l’IP, de l’heure (soir) ou de la date (week-end), ... "bloquer" le compte et prévenir son propriétaire et l'administrateur.
 - Stocker correctement les mots de passe en base: ils doivent être hashés et salés avec des algorithmes sûrs type bcrypt ou PBKDF2.

### Outils

Lors des tests, il peut être intéressant de mettre en oeuvre les mêmes outils que les aattaquants:
 - [Hashcat](https://hashcat.net/hashcat/) & [john the ripper](https://www.openwall.com/john/): de manière régulère, vous pouvez extraire les mots de passes et tenter de les casser pour découvrir les mots de passe faibles.
 - [Hydra](https://github.com/vanhauser-thc/thc-hydra/)

## A3-Sensitive Data Exposure

### Présentation

L'"exposition de données sensibles" ou **Sensitive Data Exposure** est une menace pesant sur la récupération
de données par l'attaquant. Les données ont d'abord une valeur métier: numéros de cartes de crédit, informations
personnelles, mots de passes.

L'exposition de ces données est possible de nombreuses façons:
 - Le numéro de carte bleue est affiché en clair dans le profil de l'utilisateur. Si l'utilisateur a laissé sa session ouverte sur un poste publique, un attaquant opportuniste peut récupérer ce numéro.
 - La réponse JSON ou un en-tête d'une API renvoie beaucoup trop d'informations. Toutes ne sont pas affichées à l'écran mais un attaquant peut repérer cette fuite de données.
 - Les données transites en clair sur le réseau WiFi dans un aéroport. Via une écoute passive, un attaquant peut facilement les récupérer.
 - Les mots de passes sont stockées en clair dans la base. Si une faille type SQL Injection était présente dans l'application, il serait trivial pour un attaquant d'avoir accès à ces mots de passe.

### Solutions

Ici, il est important mettre en place du chiffrement efficace pour assurer la confidentialité des données.
Pour ce faire, il faut vérifier :
 - Que les données sont chiffrées que ce soit en transit ou lorsqu'elles sont stockées.
 - De respecter les guides pour le choix et la configuration des algorithmes de chiffrement
 - Que les clés soient fortes et correctement protégées

### Outils

 - [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/)


## A4-XML External Entities

### Présentation

### Solutions

### Outils


## A5-Broken Access Control

### Présentation

### Solutions

### Outils

## A6-Security Misconfiguration

### Présentation

### Solutions

### Outils

## A7-XSS

### Présentation

### Solutions

### Outils

## A8-Insecure Deserialization

### Présentation

### Solutions

### Outils

## A9-Using Components with Known Vulnerabilities

### Présentation

### Solutions

### Outils

## A10-Insufficient Logging & Monitoring

### Présentation

### Solutions

### Outils




