# Rappels sur le WEB

Ce chapitre présente des rappels indispensables pour comprendre les différentes attaques présentées par la suite

## Le protocole HTTP

[HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), pour HyperText Transfer Protocol, est le 
protocole de base du Web. La version actuelle, HTTP/1.1 a été définie en 1997 et a continué d'évoluer.
Les prochaines versions sont HTTP/2 qui commencent à se diffuser et HTTP/3.

HTTP permet à un client de dialoguer avec un serveur HTTP. Le client envoie une requête et le serveur envoie une réponse.

{% include image.html file="intro_web_query_response.png" caption="Requête/réponse à un serveur" %}

Le protocole est sans état. Le serveur ne garde pas, en théorie, d'informations sur les précédents échanges.
Les sessions ont été créées pour permettre de conserver des informations entre les appels.

Voici un exemple de requête/réponse obtenu avec cURL:

    $ curl -v http://www.lemonde.fr
    > GET / HTTP/1.1
    > Host: www.lemonde.fr
    > User-Agent: curl/7.55.1
    > Accept: */*
    < HTTP/1.1 301 Moved Permanently
    < Location: https://www.lemonde.fr/
    < Content-Length: 0
    < Accept-Ranges: bytes
    < Date: Sun, 30 Sep 2018 07:08:15 GMT
    < Via: 1.1 varnish
    < Age: 27
    < Connection: keep-alive
    < X-Served-By: cache-cdg20729-CDG
    < Set-Cookie: prog-deploy=23; expires=Fri, 29 Mar 2019 07:08:15 GMT; path=/; domain=.lemonde.fr;
    < Set-Cookie: prog-deploy2=78; expires=Fri, 29 Mar 2019 07:08:15 GMT; path=/; domain=.lemonde.fr;


Nous allons analyser les constituants du protocole HTTP et voir comment ils peuvent être détournées par un attaquant.

## Uniform Resource Locator

L'URL est un moyen d'obtenir des ressources situées sur le réseau. Dans l'exemple d'introduction, il s'agissait de **http://www.lemonde.fr**.

Elle se décompose comme suit:

> http://login:pwd@www.here.com:8888/chemin/d/acc%C3%A8s.php?q=req&q2=req2#s

 - http: le schéma ou scheme en anglais
 - www.here.com: l'hôte
 - 8888: le port de connexion
 - /chemin/d/acc%C3%A8s.php: le chemin d'accès ou path en anglais
 - ?q=req&q2=req2: la requête/query
 - #s: le fragment

### Le schéma

Le schéma sert à indiquer la manière dont une ressource doit être accédée. 
Par exemple, il est possible d'avoir les schémas suivants, potentiellement interprétable par un navigateur:

 - http/https
 - file
 - ftp
 - gopher, shttp, ...
 - [javascript:alert(1)](javascript:alert(1)]
 - [data](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAMAAAC6V+0/AAAAwFBMVEXm7NK41k3w8fDv7+q01Tyy0zqv0DeqyjOszDWnxjClxC6iwCu11z6y1DvA2WbY4rCAmSXO3JZDTxOiwC3q7tyryzTs7uSqyi6tzTCmxSukwi9aaxkWGga+3FLv8Ozh6MTT36MrMwywyVBziSC01TbT5ZW9z3Xi6Mq2y2Xu8Oioxy7f572qxzvI33Tb6KvR35ilwTmvykiwzzvV36/G2IPw8O++02+btyepyDKvzzifvSmw0TmtzTbw8PAAAADx8fEC59dUAAAA50lEQVQYV13RaXPCIBAG4FiVqlhyX5o23vfVqUq6mvD//1XZJY5T9xPzzLuwgKXKslQvZSG+6UXgCnFePtBE7e/ivXP/nRvUUl7UqNclvO3rpLqofPDAD8xiu2pOntjamqRy/RqZxs81oeVzwpCwfyA8A+8mLKFku9XfI0YnSKXnSYZ7ahSII+AwrqoMmEFKriAeVrqGM4O4Z+ADZIhjg3R6LtMpWuW0ERs5zunKVHdnnnMLNQqaUS0kyKkjE1aE98b8y9x9JYHH8aZXFMKO6JFMEvhucj3Wj0kY2D92HlHbE/9Vk77mD6srRZqmVEAZAAAAAElFTkSuQmCC)

Un attaquant peut exploiter certains d'entre eux (comme data ou javascript) pour agir sur le comportement d'une application web.

### L'hôte

L'hôte peut-être représenté sous forme d'une adresse IP ou d'un nom de domaine. 
Le nom de domaine est résolu sous le forme d'une IP via un appel au DNS. 

Ici, nous n'étudierons pas les possibilités qu'à un attaquant de détourner les noms de domaines (par exemple, avec du [DNS spoofing](https://en.wikipedia.org/wiki/DNS_spoofing)). De même, un attaquant peut profiter des caractères non-ASCII pour le nom de domaine et effectuer une [attaque homographique](https://en.wikipedia.org/wiki/IDN_homograph_attack) (par exemple, avec [https://www.аррӏе.com/](https://www.xn--80ak6aa92e.com/), à tester sous Firefox). On peut déjà noter que l'attaquant possède plusieurs points pour effectuer une attaque.

La représentation d'une adresse IP n'est pas non plus unique. Ainsi, les représentations suivantes sont valables pour un navigateur:

 - https://216.58.213.164/ : IPv4
 - https://[2a00:1450:4007:808::2004]/ : IPv6
 - http://0xd83ad5a4 : représentation hexadecimale
 - http://0x7f.1/ : représentation héxadécimale réduite
 - http://033016552644 : représentation octale
 - //0xd83ad5a4 : représentation héxadécimale sans schéma

Grâce à ces différentes formes, un attaquant peut essayer de passer des contrôles liés au format. Ainsi [033016552644](033016552644) peut potentiellement vu comme une simple chaine de caractères et être interprété comme une URL par le navigateur.

## Requêtes HTTP

Pour obtenir une réponse d'un serveur, il est nécessaire de fournir au moins la méthode et la resssource.
Les autres éléments sont optionnelles.

Le [Mozilla Developer Network présente avec détails le fonctionnement de HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages).

### Méthodes

Dans l'exemple d'introduction, la méthode était **GET**. Il est possible de tester un appel avec netcat:

> echo -e "GET / HTTP/1.1\nHost: www.lemonde.fr\n\n" | nc www.lemonde.fr 80

Il existe de nombreuses méthodes qui ont une sémantique propre:
 - GET
 - POST: Un corps (**body**) est envoyé avec la requête. C'est généralement dans le body de la requête que le contenu d'un formulaire est envoyé.
 - PUT
 - PATCH
 - DELETE
 - HEAD
 - TRACE
 - OPTIONS: permet de connaître les méthodes supportées par le navigateur. Nous en reparlerons dans le cas de CORS.
 - ...

Le [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) décrit ces méthodes.

Un attaquant peut essayer différentes méthodes et espérer un résultat. Dans le cadre des API, nous verrons comment cela peut conduire à une vulnérabilité de type [Missing Function/Resource Level Access Control](https://github.com/OWASP/API-Security/blob/develop/2019/en/dist/owasp-api-security-top-10.pdf).

### En-têtes 

Des en-têtes sont transmis en même temps que la requête. Ils servent à informer le serveur des attentes du client concernant le contenu.

Là encore, le [MDN décrit les en-têtes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).
On pourra les noter les en-têtes suivants:

 - Host: Spécifie le nom de domaine. Cela peut servir pour du [virtual hosting](https://httpd.apache.org/docs/2.4/vhosts/examples.html) quand plusieurs sites sont sur le même serveur avec des alias différents.
 - Content-Length: longueur du contenu. Ici, il est possible de jouer avec les valeurs pour perturber le serveur [exemple(](https://portswigger.net/web-security/request-smuggling/exploiting))
 - Content-Type: [type MIME](https://www.iana.org/assignments/media-types/media-types.xhtml) du contenu envoyé. Un attaquant peut indiquer un type différent pour tromper l'application
 - Cookie: le "fameux" cookie. Nous en parlons un peu plus dans la suite.
 - Referer: l'adresse de la page précedemment visitée qui contient le lien sur lequel on a cliqué. Cela peut exposer des URLs internes à une entreprise
 - User-Agent: donne des informations sur le client. Exemple:i `user-agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/76.0.3809.100 Chrome/76.0.3809.100 Safari/537.36`. Avec ces informations, un attaquant peut tenter d'exploiter des vulnérabilités spécifiques.

### Le corps

Un contenu est généralement présent avec des requêtes de type POST ou PUT.
Les champs d'un formulaire sont envoyés dedans.

## Réponses HTTP

Les réponses donnent généralement beaucoup d'informations sur le serveur.

### Code de retour

Le serveur renvoie différents codes de retour. Le premier chiffre indique la classe de la réponse:
 - 1XX: message informatif. On le retrouve avec les websocket par exemple qui renvoie un code **100 Continue**.
 - 2XX: le serveur répond avec succès (Exemple: 200 OK)
 - 3XX: Redirection (Exemple
 - 4XX: Erreur de la part du client (Exemple: 404 Not Found)
 - 5XX: Erreur côté serveur. Le message **500 Internal Server Error** indique que le serveur a "planté" du fait de la requête. Il est généralement plutôt conseillé de transformer la réponse en code 400 et de tracer l'erreur. L'attaquant aura plus de difficulté à faire la différence entre une requête pouvant être détournée car elle fait "planter" le serveur et une requête avec des paramètres incorrects.

Dis autrement [Tweet original](https://twitter.com/stevelosh/status/372740571749572610):

> 1xx: hold on
> 2xx: here you go
> 3xx: go away
> 4xx: you fucked up
> 5xx: I fucked up

### En-têtes

De même, de nombreux en-têtes existent. Les plus intéressants sont:

 - Server: renvoie le nom du serveur et la version (Exemple: `server: meinheld/0.6.1`). Cet en-tête peut être modifié ou supprimé. C'est rarement le cas. Grâce à cette information, un attaquant peut sélectionner un exploit pour perturber ou prendre possession du serveur.
 - X-Powered-By: ici aussi, le serveur indique quelle framework est utilisée (Exemple: `X-Powered-By: Flask`). 
 - Via/X-Forwarded-For: la requête a été modifiée par un proxy ou un répartiteur de charge (**LoadBalancer**).
 - Strict-Transport-Security: indique que le certificat doit être considéré comme sûr pendant un certain temps. Si un attaquant tente de changer le certificat, le client refusera la réponse du serveur.
 - X-Frame-Options: indique au client si la page peut être dans une autre page. Le but est d'empecher le clickjacking où un site légitime est inclut dans un site contrôlé par l'attaquant. Celui-ci peut tenter de superposer des éléments HTML pour capturer des identifiants par exemple.
 - Content-Security-Policy: cet en-tête décrit au client si tel ressource peut être incluse ou utilisée dans la page.
 - Expect-CT: ce nouvel en-tête a pour but de contrer le cas où une autorité de certification a été compromise. Dans ce cas, un attaquant aurait toute possibilité de créer des certificats reconnus par les navigateurs.

### Contenu

Nous allons parler un peu plus du contenu renvoyé, notamment les pages HTML, dans le dernier paragraphe.

## SSL/TLS

## Présentation

Les protocoles Secure Sockets Layer (SSL) et Transport Layer Security (TLS) ont pour but de mettre en oeuvre une communication sécurisée entre client et serveur. Pour ce faire:
 - La communication est chiffrée avec pour but d'assurer les propriétés d'intégrité et de confidentialité à l'échange. Le client et le serveur s'accordent sur l'algorithme à utiliser via la  "poignée de main" (**handshake**).
 - La communication est authentifiée. Le serveur envoie son certificat signée par une autorité de confiance (une autorité qui a son certificat installée sur le poste client). Le client valide alors le certificat. Généralement, le client ne s'authentifie pas au serveur (ou en tout cas, pas via certificat).

SSL et TLS ont évolué au cours du temps:
 - 1995: SSL1, jamais publié car contienaient des failles conceptuels
 - 1995: SSL2, publié mais avaient des failles
 - 1996: SSL3
 - 1999: TLS1.0
 - 2006: TLS1.1
 - 2008: TLS1.2
 - 2018: TLS1.3, changement dans les algorithmes acceptéss, hash de session pour une connexion plus rapide

### TLS 1.2 et 1.3

La communcation HTTP est sécurisée avec TLS via le protocole HTTPS (pour **HTTP over SSL/TLS**).
TLS n'est pas forcément uniquement utilisé avec HTTP. On le retrouve sur d'autres protocoles comme FTP.

L'ANSSI propose un guide intitulé [Recommendations de sécurité relatives à TLS](https://www.ssi.gouv.fr/uploads/2016/09/guide_tls_v1.1.pdf). Ce guide présente le "handshake" ainsi que les suites de chiffrements recommandées pour TLS 1.2.

Comment lire une suite cryptographique? Prenons la suite `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`:
 - TLS: protocole utilisée
 - EC: Courbe elliptique.
 - DHE: Diffie-Hellmann pour l'échange de clé de session. Cela assure la proprité de **Perfect Forward Secrecy**
 - DSA: Digital Signature Algorithm
 - AES256: algorithme de chiffrement symétrique qui utilise la clé de session
 - GCM: Galois/Counter Mode. Il s'agit d'un mode d'opération pour le chiffrement par bloc, au même titre que CBC ou ECB mais sans leurs défauts respectifs.
 - SHA384

La nouvelle norme TLS 1.3 introduit:
 - un handshake simplifié lors d'une nouvelle visite
 - Le Perfect Forward Secrecy est obligatoire

### Certificats

Récupérons et analysons un certificat:

    $ openssl s_client -connect www.google.fr:443 </dev/null 2>/dev/null|openssl x509 -text
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                13:1d:4a:3d:37:0f:49:26:08:00:00:00:00:19:0d:b6
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=US, O=Google Trust Services, CN=GTS CA 1O1
            Validity
                Not Before: Oct 10 20:55:10 2019 GMT
                Not After : Jan  2 20:55:10 2020 GMT
            Subject: C=US, ST=California, L=Mountain View, O=Google LLC, CN=google.com
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (2048 bit)
                    Modulus:
                        00:cb:71:89:7f:29:21:a5:14:a2:06:8a:ab:44:eb:
                        ...
                        46:16:04:03:85:63:30:93:50:99:6b:13:23:50:4a:
                        3e:29
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Key Usage: critical
                    Digital Signature, Key Encipherment
                X509v3 Extended Key Usage: 
                    TLS Web Server Authentication
                X509v3 Basic Constraints: critical
                    CA:FALSE
                X509v3 Subject Key Identifier: 
                    DA:26:F6:BF:40:67:75:79:01:76:95:56:CE:46:60:07:96:3D:5E:49
                X509v3 Authority Key Identifier: 
                    keyid:98:D1:F8:6E:10:EB:CF:9B:EC:60:9F:18:90:1B:A0:EB:7D:09:FD:2B
    
                Authority Information Access: 
                    OCSP - URI:http://ocsp.pki.goog/gts1o1
                    CA Issuers - URI:http://pki.goog/gsr2/GTS1O1.crt
    
                X509v3 Subject Alternative Name: 
                    DNS:google.com, DNS:*.2mdn.net, DNS:*.android.com, ...
                X509v3 Certificate Policies: 
                    Policy: 2.23.140.1.2.2
                    Policy: 1.3.6.1.4.1.11129.2.5.3
    
                X509v3 CRL Distribution Points: 
    
                    Full Name:
                      URI:http://crl.pki.goog/GTS1O1.crl
    
                CT Precertificate SCTs: 
                    Signed Certificate Timestamp:
                        Version   : v1(0)
                        Log ID    : B2:1E:05:CC:8B:A2:CD:8A:20:4E:87:66:F9:2B:B9:8A:
                                    25:20:67:6B:DA:FA:70:E7:B2:49:53:2D:EF:8B:90:5E
                        Timestamp : Oct 10 21:55:29.328 2019 GMT
                        Extensions: none
                        Signature : ecdsa-with-SHA256
                                    30:44:02:20:57:B5:92:7A:2C:9E:5D:AC:4F:B4:45:B7:
                                    4F:25:45:51:E3:26:8A:F4:66:57:E5:DB:4C:A6:05:6E:
                                    32:4F:D5:DF:02:20:2E:6C:20:22:CF:57:E4:62:C2:52:
                                    45:DD:2F:8B:93:D1:90:2D:DE:D0:0E:C8:1E:61:AD:7C:
                                    62:AA:0B:A7:4A:CA
                    Signed Certificate Timestamp:
                        Version   : v1(0)
                        Log ID    : 5E:A7:73:F9:DF:56:C0:E7:B5:36:48:7D:D0:49:E0:32:
                                    7A:91:9A:0C:84:A1:12:12:84:18:75:96:81:71:45:58
                        Timestamp : Oct 10 21:55:29.380 2019 GMT
                        Extensions: none
                        Signature : ecdsa-with-SHA256
                                    30:44:02:20:78:25:8B:52:C1:33:86:7E:A0:83:54:38:
                                    76:B2:9C:B5:04:13:89:27:7A:40:2D:F5:08:52:C2:52:
                                    22:0A:16:9D:02:20:32:93:C8:68:F9:ED:64:96:DC:90:
                                    D0:DC:EB:2F:16:D7:69:65:8B:BF:21:31:63:07:D2:92:
                                    3C:5E:9D:DA:92:EA
        Signature Algorithm: sha256WithRSAEncryption
             23:a1:ac:33:24:cd:ac:81:dc:e5:9a:c4:07:c2:57:82:cf:46:
             ...
             45:eb:f5:96:2d:4b:c8:c8:7b:1c:3e:89:3d:17:e4:71:6d:79:
             e6:e8:32:8b

On retrouve dans ce certificat:
 - Son propriétaire (`Subject: C=US, ST=California, L=Mountain View, O=Google LLC, CN=google.com`)
 - La clé publique
 - Des extensions pour x509, notamment celle pour Expect-CT
 - La signature de l'autorité de confiance

Par défaut, un certificat est associé à un domaine et, potentiellement, plusieurs sous-domaines. Cependant, cela ne prouve pas à qui appartient le site.
Par exemple, avec [Let's Encrypt](https://letsencrypt.org/), il suffit d'ajouter un champ TXT avec une valeur spécifique au niveau du DNS pour obtenir un certificat reconnu.
Pour prouver l'identité d'un possesseur, les autorités de confiance contrôlent l'identité et crée un certificat spécial: [Extended Validation Certificate](https://en.wikipedia.org/wiki/Extended_Validation_Certificate).

### Attaques possibles

De nombreuses attaques sont possibles:

 - Absence de certificat: on force la communication à rester en HTTP ou il n'a pas été prévu de redirection vers HTTPS.
 - Remplacement du certificat: via un Man-in-the-Middle par exemple, un attaquant peut tenter d'introduire son certificat
 - Production d'un certificat contrefait: L'utilisateur est dirigé vers un site. L'attaquant a volé un certificat maître connu du client et le certificat est signé par cette autorité de confiance. Le site est alors considéré comme sûr.
 - Faille dans l'implémentation ([HeartBleed](https://en.wikipedia.org/wiki/Heartbleed))
 - Faille dans les algorithmes: les algorithmes faibles comme MD5 pour la signature ou RC4 ne sont pas utilisés avec TLS 
 - Social Engineering pour faire accepter un certificat non valide
 - Interception d'une communication chiffrée et déchiffrement ultérieur grâce au vol du certificat: se référer à la partie **Perfect Forward Secrecy**
 - Downgrade de protocoles ([POODLE](https://en.wikipedia.org/wiki/POODLE))
 - Certaines [organismes](https://en.wikipedia.org/wiki/Dual_EC_DRBG) semblent aussi être à l'oeuvre pour affaiblir les protocoles.

### Protections

#### Perfect Forward Secrecy

Imaginons que la communication entre le client et le serveur ait été interceptée et que l'attaquant posssède la clé privée du certificat.
Normalement, avec des suites type RSA pour la partie échange de clé de session, le client utilise la clé publique du certifcat pour envoyer la clé de session au serveur. Cependant, si l'attaquant possède la clé privée, il est en mesure de déchiffrer l'ensemble de la communication. Nous renvoyons le lecteur à la capture [SSL with decryption keys](https://wiki.wireshark.org/SampleCaptures#SSL_with_decryption_keys) pour tester.

Comment empêcher cette situation? La réponse passe par l'échange selon l'algorithme de Diffie-Hellmann. Cet algorithme permet d'échanger la clé de session de session sans faire transiter la clé sur le canal de communication. Le schéma suivant présente l'échange en action:

{% include image.html file="intro_web_dhe.png" caption="Diffie-Hellmann en action" %}

Le flux est le suivant:
 - Le serveur génère les valeurs a, P et G. Le serveur envoie au client `A = g^a mod p` et (p,g).
 - Le client génère la valeur b. Le client envoie au serveur B = g^b mod p
 - Le serveur calcule alors K = B^a mod p. Le client calcule K = A^b mod p. 

Client et Serveur ont donc la même clé sans que l'attaquant l'ait vu passer. 
Pour pouvoir retrouver les valeurs a et b et potentiellement construire K, un attaquant doit résoudre le problème du [logarithme discret](https://fr.wikipedia.org/wiki/Logarithme_discret) qui est un problème mathématiquement dur.

#### Configuration

 - [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/)
 - [SSLyze](https://tools.kali.org/information-gathering/sslyze)

#### Education

L'éducation des utilisateurs reste un élément essentiel. Savoir quand accepté un certificat ou qu'un site peut ne pas être viable aide dans la sécurisation globale.

## Cookies & Sessions

## Contenu

### HTML

### Javascript

### Same Origin Policy

### Cross-Origin Resource Sharing
