+++
author = "Skhaen"
categories = ["ssl", "apache2", "nginx", "adminsys"]
date = 2014-07-27T10:00:00Z
description = ""
draft = false
slug = "serveur-du-tls-avec-un-peu-de-hsts-et-un-soupcon-de-tlsa-sil-vous-plait"
tags = ["ssl", "apache2", "nginx", "adminsys"]
title = "Serveur ? Du TLS, avec un peu de HSTS et un soupçon de TLSA s'il vous plait !"

+++

Continuons notre voyage dans la crypto, nous avons vu dernièrement la mise en place d'un [serveur Prosody](https://www.libwalk.so/tag/prosody.html) avec une sécurité satisfaisante. Aujourd'hui, nous allons jouer avec Apache (mais aussi Nginx), en configurant du SSL/TLS avec du [HSTS](https://www.eff.org/deeplinks/2014/02/websites-hsts), 

#### SSL/TLS (*[Secure Sockets Layer](https://fr.wikipedia.org/wiki/Transport_Layer_Security)* et *[Transport Layer Security](https://fr.wikipedia.org/wiki/Transport_Layer_Security)*)

Pour bien commencer, je vous conseille vivement la conférence de Benjamin Sonntag sur le [SSL/TLS](http://www.iletaitunefoisinternet.fr/ssltls-benjamin-sonntag/). Il y a dedans tout ce que vous avez besoin de savoir, et bien plus encore.

<iframe><video class='video' controls='controls' width='700'>
<source src='http://data.confs.fr/ssl-tls-sonntag/360p/IEUFI_ssl-tls_Sonntag_360.mp4' type='video/mp4'></source>
<source src='http://data.confs.fr/ssl-tls-sonntag/360p/IEUFI_ssl-tls_Sonntag_360.webm' type='video/webm'></source>
<object data='/assets/flashmediaelement.swf' height='360' type='application/x-shockwave-flash' width='640'>
<param name='allowFullScreen' value='true'></param>
<param name='flashvars' value='controls=true&amp;file=http://cdn.media.ccc.de/congress/2013/mp4/30c3-5713-en-de-To_Protect_And_Infect_Part_2_h264-hq.mp4'></param>
</object>
</video>
</iframe>



#### **HSTS** (*[HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)*) 
> <u>RFC 6797, [psaume 8.4](http://tools.ietf.org/html/rfc6797#section-8.4)</u> :<br />When connecting to a Known HSTS Host, the UA MUST terminate the connection if there are any errors, whether "warning" or "fatal" or any other error level, with the underlying secure transport.  For example, this includes any errors found in certificate validity checking that UAs employ, such as via Certificate Revocation Lists ([CRLs](http://tools.ietf.org/html/rfc5280)) , or via the Online Certificate Status Protocol ([OCSP](http://tools.ietf.org/html/rfc2560)), as well as via [TLS server identity checking](http://tools.ietf.org/html/rfc6125).

HSTS permet au serveur d'indiquer à votre navigateur que oui, il sait communiquer en http**s**, et que si il revient par la suite, qu'il le fasse directement de manière sécurisée pour une période donnée. Ce qui veut dire que le navigateur remplace automatiquement TOUS liens non sécurisés (http) par des liens sécurisés (https) avant d'accéder au serveur pour ladite période.

Pour information :

* HSTS ne permet pas l'utilisation de certificat auto-signé ou périmé.
* HSTS sur une longue durée est obligatoire pour avoir un [A+](https://www.ssllabs.com/ssltest/analyze.html?d=libwalk.so) sur le site de [ssllabs](https://www.ssllabs.com).

## Création d'un certificat SSL/TLS

#### génération du CSR (*Certificate Signing Request*)

Nous allons commencer par créer une requête de certificat (*[certificate request](https://en.wikipedia.org/wiki/Certificate_signing_request)*) avec openssl :

```
openssl req -sha256 -out /etc/ssl/private/exemple.csr -new -newkey rsa:2048 -nodes -keyout /etc/ssl/private/exemple.key
```

Cette commande génère (<code>openssl req -out</code>) une [CSR](https://en.wikipedia.org/wiki/Certificate_signing_request) (<code>-new</code>) ainsi qu'une nouvelle [clé RSA](https://fr.wikipedia.org/wiki/Chiffrement_RSA) de 2048 bits (<code>-newkey rsa:2048</code>) qui ne sera pas chiffrée (<code>-nodes</code>). Si vous voulez encore plus de détails, c'est sur [openssl.org](https://www.openssl.org/docs/apps/req.html#).


Je vous laisse vous inscrire sur [startssl.com](http://www.startssl.com/) (bonne chance !), il faut choisir la **première formule** (qui est gratuite). Après la création de votre compte, la validation de votre nom de domaine, etc etc... nous pouvons passer à la suite.

* Après votre authentification sur le site, cherchez l'onglet « *Certificates Wizard* » qui se trouve normalement vers la gauche de votre écran,
* il vous demande alors pourquoi vous voulez un certificat ("*Select Certificate Purpose*"), vous choisissez donc **Web Server SSL/TLS certificate**,
* le site vous demande de créer une clé privée ("*Generate Private Key*") : **VOUS NE LE FAITES PAS !**, vous appuyez au contraire sur **skip** (nous avons créé la clé privée juste avant en local),
* vous arrivez maintenant sur la page de demande de certificat ("*Submit Certificate Request (CSR)*"), toujours dans un terminal, vous devez taper la commande suivante :

```
cat /etc/ssl/private/exemple.csr
```

Cette commande vous donnera un résultat qui doit ressembler à ceci (bien entendu sans la même suite de caractères) :

```
-----BEGIN CERTIFICATE REQUEST-----
MIIChjCCAW4CAQAwQTELMAkGA1UEBhMCRlIxCzAJBgNVBAgMAkZSMQswCQYDVQQH
DAJGUjELMAkGA1UECgwCRlIxCzAJBgNVBAsMAkZSMIIBIjANBgkqhkiG9w0BAQEF
AAOCAQ8AMIIBCgKCAQEAsoJcj6/bwl9naKG9C9seKt4HjBicV5o96zqoO0YxtJAe
X9k2t4KTp0CrzQ85c9DfggY8oAMq/DX/xRFL0cPxamxSwwW5ttVoBQ04wBWDhjEo
a2ixpe5UMmfakuY3Q56HsIbhh7Vo4RZS1OtPOv7E2J0CfDVUhrNCpDjZbtM8akTE
9P86BkXdroJgU8tfwONMFDBF2K8ElhN6mqftb89KGIUpgm1fcDq8woRpnFER7A3H
OwfCfnlkLrtMWVca1smEWnlutBKw6cgk6uSMK9V9/Y44wMKZHoOrOQE0R26+MGrA
MLhprqPaANIvhamq+tSsSASYZDeajDS3R1JWX188awIDAQABoAAwDQYJKoZIhvcN
AQEFBQADggEBAHYSpBxHhRP87qmWNqp9Sf8dYz3oQfJLA2cLpQV2MOIfFW0mmOyz
JG6TVISKVmiEHZtHqgW4TL3BSKBAWENBM8mjAjmxXCmy2MBSWBVhDVaGz4w+x3hO
UMtNMubYxkkc/xgX5vwbuReH6y1sbkMUQm1UETb6Fnmm8dyDzwPI0zV+NdzUqqhI
ARjMM2RrwPH7QZ2lSAOiB/X+fXKhwMSg0qUExYiln20JKBi6f58GdyOu6Hp/Fi+m
r8xnIcnZ2ZIIyjh4B2bfAfybTOWHHRtOaI9yH8pTP3HnKqgbtxZJYqioTAAAQxjQ
hFmXThFFrfhTDnqJ0Fc+bjcoiLoy46FtLz8=
-----END CERTIFICATE REQUEST-----
```


* Copier le (de <code>-----BEGIN CERTIFICATE</code> à <code>END CERTIFICATE REQUEST-----</code> !) dans la fenêtre en bas. 
* Il est ensuite nécessaire d'indiquer le site,
* puis le sous-domaine visé (si vous ne savez pas vous pouvez mettre <code>www</code> par défaut.
* vous arrivez alors sur la page « *Save Certificate* » avec votre nouveau certificat dans une fenêtre :

```
-----BEGIN CERTIFICATE-----
MIIGWjCCBUKgAwIBAgIDEm6yMA0GCSqGSIb3DQEBBQUAMIGMMQswCQYDVQQGEwJJ
TDEWMBQGA1UEChMNU3RhcnRDb20gTHRkLjErMCkGA1UECxMiU2VjdXJlIERpZ2l0
YWwgQ2VydGlmaWNhdGUgU2lnbmluZzE4MDYGA1UEAxMvU3RhcnRDb20gQ2xhc3Mg
[...]
-----END CERTIFICATE-----
```

 * copiez le dans un nouveau fichier appelé <code>exemple.crt</code> sur votre serveur en faisant bien attention de prendre du début de <code>-----BEGIN CERTIFICATE REQUEST</code> à la fin de <code>END CERTIFICATE REQUEST-----</code> !

### Création d'un certificat chaîné

Pour créer un certificat chaîné, faites la manipulation suivante (startssl.com vous donne les liens vers ces deux certificats juste au dessus du bouton **finish »»** de la page **Save Certificate**) :

* télécharger le certificat intermédiaire de startssl :

```
wget https://www.startssl.com/certs/sub.class1.server.ca.pem
```

* puis le certificat root : 

```
wget https://www.startssl.com/certs/ca.pem
```

* puis on créé le le certificat chaîné (ne pas oublier les **2** <code>>></code>) :

<pre>cat sub.class1.server.ca.pem > /etc/ssl/private/exemple.chain && cat ca.pem >> /etc/ssl/private/exemple.chain</pre>

## Installation et configuration

On ne verra pas l'installation d'Apache et de Nginx ici, si vous êtes en train de lire ces lignes, vous devriez normalement déjà savoir le faire. Si ce n'est pas le cas, voici quelques pistes à suivre :

* [installation d'Apache sous Debian](http://fr.openclassrooms.com/informatique/cours/apprenez-a-installer-un-serveur-web-sous-debian) (fr.openclassrooms.com),
* [installation de Nginx sous Debian](http://blog.nicolargo.com/2011/01/installation-automatique-de-nginx-php-fpm-memcached-sous-debian.html) (blog.nicolargo.com),

### Apache

#### SSL/TLS

Ce qui suit est à rajouter à votre [fichier de configuration](http://httpd.apache.org/docs/2.2/vhosts/examples.html) dans <code>/etc/apache2/sites-enabled</code> au sein d'un bloc <code>VirtualHost *:443</code>. En cas de problème, vous pouvez aller voir la [FAQ d'Apache en français concernant SSL/TLS](http://httpd.apache.org/docs/current/ssl/ssl_faq.html). 

```
SSLCertificateFile        /etc/ssl/private/exemple.com.crt
SSLCertificateChainFile   /etc/ssl/private/exemple.com.chain
SSLCertificateKeyFile     /etc/ssl/private/exemple.com.key

SSLEngine on
SSLProtocol +TLSv1.2 +TLSv1.1 +TLSv1
SSLHonorCipherOrder on
SSLCipherSuite ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM 
SSLCompression off
```
Vous pouvez remplacer l'avant dernière ligne par la suivante si le coeur vous en dit (la précédente est mieux sur le long terme si vous maintenez votre machine à jour) :

```
SSLCipherSuite "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA !RC4 !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS"
```
Pour vérifier que votre configuration est bonne, vous pouvez lancer la commande suivante :

    apache2ctl -t

#### DHparam

Apache génère automatiquement un DHparam de 1024 bits, si vous voulez modifier cette configuration, il est préférable de passer en [version 2.4](http://httpd.apache.org/docs/2.4/).

#### HSTS

Pour permettre l'utilisation du HSTS, il faut commencer par activer le <code>mod_headers</code> d'[Apache](http://httpd.apache.org/docs/current/mod/mod_headers.html).

```
a2enmod headers
```

Puis rajouter dans votre fichier de configuration la ligne suivante :

> ATTENTION ! 31622400 équivaut à **366 jours** !
Réfléchissez bien avant de l'activer !</blockquote>

```
Header set Strict-Transport-Security "max-age=31622400"
```

Si vous voulez aussi inclure vos **sous-domaines** dans le HSTS (ce qui veut dire que vous avez soit un certificat <a href="https://fr.wikipedia.org/wiki/Certificat_%C3%A9lectronique#Certificats_X.509_omnidomaines">wildcard</a>, soit un certificat différent par sous-domaine **mais surtout qu'aucun sous-domaine n'utilise PAS SEULEMENT HTTP !**), vous pouvez ajouter la ligne suivante :

```
Header append Strict-Transport-Security includeSubDomains
```

### Nginx

#### DHparam

Génération du [dhparam](https://www.openssl.org/docs/apps/dhparam.html#) :

```
openssl dhparam -out /etc/ssl/private/dh2048.pem -outform PEM -2 2048
```


Et si vous vous demandez à quoi consiste les signes qui s'affichent lors de la génération, la réponse est ici : [Legend for OpenSSL’s dhparam output](http://ix-notes.blogspot.co.uk/2009/04/legend-for-openssls-dhparam-output.html).

#### SSL/TLS

Ce qui suit est à rajouter à votre fichier de configuration dans <code>/etc/nginx/sites-enabled</code>. Pour plus de détails sur la mise en place du SSL/TLS pour nginx, c'est ici : [Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html).

```
listen 443 ssl;
ssl on;
ssl_session_cache   shared:SSL:10m;
ssl_session_timeout 10m;

ssl_certificate /etc/ssl/private/exemple.com.crt;
ssl_certificate_key /etc/ssl/private/exemple.com.key;
ssl_dhparam /etc/ssl/private/dh2048.pem; 
ssl_prefer_server_ciphers on;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_ciphers ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM;
```

Vous pouvez remplacer la ligne `ssl_certificate /etc/ssl/private/exemple.com.crt;` par `ssl_certificate /etc/ssl/private/exemple.com.+chain;` si vous avez un [certificat chaîné](https://www.startssl.com/?app=42).

#### HSTS

Pour activer le HSTS, il faut rajouter la ligne suivante avant la dernière accolade : 

> ATTENTION ! 31622400 équivaut à **366 jours** !
Réfléchissez bien avant de l'activer !</blockquote>

```
add_header Strict-Transport-Security max-age=2678400;
```

#### DANE & TLSA

> Le système [DANE](http://en.wikipedia.org/wiki/DNS-based%20Authentication%20of%20Named%20Entities) ([DNS-based Authentication of Named Entities](https://fr.wikipedia.org/wiki/DNS_-_based_Authentication_of_Named_Entities)) permet d'augmenter la sécurité des échanges chiffrés avec TLS en permettant au gérant du serveur de publier lui-même le certificat qu'il utilise, et en permettant au client de vérifier, par une deuxième voie, le DNS, que le certificat est le bon. <small>Stéphane Bortzmeyer, [Sécurité de DANE](http://www.bortzmeyer.org/securite-dane.html)</small>

Nous pouvons faire un premier pas vers [DANE](http://en.wikipedia.org/wiki/DNS-based%20Authentication%20of%20Named%20Entities) en publiant l'empreinte (hash) de nos certificats dans nos enregistrements DNS avec la méthode suivante :

* générer l'empreinte du certificat :

```
openssl x509 -in example.com.crt -outform DER | sha256sum | awk '{print $1}'
```

Ce qui me donne la chaîne de caractères suivante :

```
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```

* ajouter les 2 lignes suivantes dans vos enregistrements DNS (**avec le hash de VOS certificats** !) :

```
_443._tcp.www IN TLSA 3 0 1 e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
_443._tcp IN TLSA 3 0 1 e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```

##### Un peu de lecture en plus : 
    
* AFNIC : [Sécuriser les communications sur Internet de bout-en-bout avec le protocole DANE](http://www.afnic.fr/fr/l-afnic-en-bref/actualites/actualites-generales/7444/show/securiser-les-communications-sur-internet-de-bout-en-bout-avec-le-protocole-dane.html#!)
* plugin DNSSEC pour [Chrome](https://chrome.google.com/webstore/search-extensions/dnssec)
* plugin [DNSEC/TLSA](https://www.dnssec-validator.cz/) pour tout le monde.

### PageSpeed

Et pour finir, je vous conseille de faire un tour sur les outils de Google, comme [PageSpeed](https://developers.google.com/speed/pagespeed/), et particulièrement sur [Analyze your site online](https://developers.google.com/speed/pagespeed/insights/) qui permet de voir ce qu'il y a à optimiser sur son site pour améliorer sa rapidité.

