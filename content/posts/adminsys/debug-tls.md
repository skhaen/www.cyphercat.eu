+++
author = "Skhaen"
categories = ["adminsys", "ssl", "apache2"]
date = 2015-07-17T12:53:00Z
description = ""
draft = false
slug = "debug-tls"
tags = ["adminsys", "ssl", "apache2"]
title = "Debug SSL/TLS avec OpenSSL - partie 1"

+++

Si tu es en pleine session de dé/bug SSL/TLS, cet article peut t'éviter quelques désagréments ;-)

### [SSL Labs](https://www.ssllabs.com/), l'incontournable

On ne le présente plus, le site de Qualys permet de [tester la configuration de son serveur](https://www.ssllabs.com/ssltest/) facilement avec une interface web bien faite. Le test permet d'avoir un rapport concernant les points suivants :

* Authentification : validité du certificat (nom du site, période de validité...),
* présence et validité de la chaîne de certificat (chain)
* Protocoles diposnibles (SSLv2, SSLv3, TLS 1.0, 1.1 et 1.2),
* Cipher suites disponibles,
* Simulations de connexions avec de nombreux clients (Android, Firefox, IE, Java, Safari, ...),
* détails (Forward Secrecy, HSTS, utilisation du protocole RC4, etc etc...).

Le tout avec des notes allant de A à F et les pistes à suivre pour améliorer sa connexion. Si vous ne l'avez pas encore fait c'est la première étape parfaite. À noter que SSL Labs permet aussi de [tester son navigateur](https://www.ssllabs.com/ssltest/viewMyClient.html) et fournit de la data sur [l'écosystème HTTPS sur Internet](https://www.trustworthyinternet.org/ssl-pulse/).

###  Création d'un .csr et d'un .key

Pour créer une *[Certificate Signing Request](https://en.wikipedia.org/wiki/Certificate_signing_request)* (*.csr*) et une nouvelle [clé privée](https://fr.wikipedia.org/wiki/Cryptographie_asym%C3%A9trique) (*.key*):

```
openssl req -sha256 -out /etc/ssl/private/exemple.csr -new -newkey rsa:2048 -nodes -keyout /etc/ssl/private/exemple.key
```


Détaillons (il parait que c'est mieux pour comprendre) :

* `req` : permet de [créer et de gérer les CSR](https://www.openssl.org/docs/apps/req.html)
* `-sha256` : Vous avez normalement vue [les annonces](http://googleonlinesecurity.blogspot.fr/2014/09/gradually-sunsetting-sha-1.html) concernant le passage de SHA1 à SHA2. Pour l'inclure dans votre CSR (*Certificate Signing Request*), il suffit de rajouter l'option `-sha256` à OpenSSL
* `-out /etc/ssl/private/exemple.csr` : indique le fichier où sera enregistrée la CSR
* `-new` : génère une nouvelle CSR et demande à l'utilisateur les valeurs qu'il souhaite
* `-newkey rsa:2048` : crée une nouvelle clé privée, de type rsa et d'une taille de 2048 bits
* `-nodes` : la clé privée n'aura pas de mot de passe
* `-keyout /etc/ssl/private/exemple.key` : indique le fichier où sera enregistrée la clé privée

### c'est pourquoi déjà ce certificat ?

Quand soudainement (enfin disons quelques temps plus tard), le drame ! Vous ne savez plus quel certificat fait quoi et pour qui, ou jusqu'à quand il est valide :

```
openssl x509 -text -in VotreCertificat.crt -noout
```
### et les deux fichiers, là, ils vont ensemble ?

Pour la clé privée :

```
openssl rsa -noout -modulus -in server.key | md5sum
```

Pour le certificat :

```
openssl x509 -noout -modulus -in server.crt | md5sum
```

Si le résultat est le même pour les deux commandes, alors votre clé privée et votre certificat sont bien pour le même nom de domaine, avec les mêmes caractéristiques (validité...)

### configuration SSL/TLS sous Apache2 :

Lors de la présence de plusieurs vhosts différents sur un même serveur Apache2, il arrive que l'on puisse avoir des résultats étranges dans les algos de chiffrement, par exemple la présence de SSLv3 alors que l'on a bien mis (et vérifié beauc... trop de fois) que l'on a bien `SSLProtocol all -SSLv2 -SSLv3` dans sa configuration.

Il faut savoir que l'on peut mettre sa configuration SSL/TLS dans **deux** fichiers différents :


Le premier (`/etc/apache2/mods-enabled/ssl.conf`) sera le choix par défaut pour l'ensemble des vhosts, alors que le second (`/etc/apache2/sites-enabled`) concernera seulement le vhost pour lequel la configuration est présente dans son fichier de configuration.  L'idée est donc de mettre la configuration générale dans `ssl.conf` et les points de configuration propre à chaque vhost dans `sites-enabled` (ou dans `sites-enabled` selon votre configuration), comme les certificats ou encore HSTS (Strict-Transport-Security) avec lequel je vous conseille de faire TRÈS attention.

* configuration générale : `/etc/apache2/mods-enabled/ssl.conf`

```
SSLCipherSuite ALL:!aNULL:!eNULL:!LOW:!EXP:!RC4:!3DES:+HIGH:+MEDIUM
SSLHonorCipherOrder on
SSLProtocol all -SSLv2 -SSLv3
```

**NOTE de non bas de page** : mon extrémiste préféré pour le ssl/tls [aeris](https://blog.imirhil.fr/extremiste-oui-et-meme-fier-de-letre.html) vous propose cette configuration un poil extrême (mais ô combien efficace) à la place de la précédente :

```
SSLCipherSuite EECDH+AES:+AES128:+AES256:+SHA
```

* configuration propre à un vhost : `etc/apache2/sites-enabled/` (entre les balises `<VirtualHost>` et `</VirtualHost>`) :

```
SSLEngine on
SSLCertificateKeyFile /etc/ssl/private/libwalk.key
SSLCertificateFile /etc/ssl/private/libwalk.crt
SSLCertificateChainFile /etc/ssl/private/libwalk.chain
SSLCompression off
Header set Strict-Transport-Security "max-age=31622400
</VirtualHost>
```

### .chain ? késako

La directive [SSLCertificateChainFile](https://httpd.apache.org/docs/2.2/mod/mod_ssl.html#sslcertificatechainfile) d'Apache2 permet d'indiquer l'emplacement du fichier comportant les [certificats chaînés](https://developer.mozilla.org/fr/docs/Introduction_%C3%A0_la_cryptographie_%C3%A0_clef_publique/Certificats_et_authentification#Cha.C3.AEnes_de_certificat) (merci mozilla pour l'explication), c'est à dire votre certificat ET les certificats de votre [CA](https://en.wikipedia.org/wiki/Certificate_authority) (comme startssl, gandi, comodo).

Quand vous faites une requêtes (CSR) chez une [CA](https://en.wikipedia.org/wiki/Certificate_authority), cette dernière, au moment de vous fournir votre certificat (.crt), vous indique (normalement) où se trouvent ses certificats à prendre dans votre cas.

Nous allons prendre comme exemple ici [startssl](https://www.startssl.com) vu qu'ils permettent d'avoir des certificats gratuits assez facilement. Une fois que j'ai fait ma requête via le formulaire en envoyant ma CSR et qu'ils m'ont fournit ma clé, je peux faire la manipulation suivante :

**IMPORTANT** : l'ordre dans le fichier **doit** commencer par le certificat le plus bas (nous), puis remonter progressivement vers le plus haut.

Imaginons que nous avons déjà notre clé privée (`exemple.tld.key`) et notre certificat (`exemple.tld.crt`)&nbsp;:

* télécharger le certificat intermédiaire de startssl :

```
wget https://www.startssl.com/certs/sub.class1.server.ca.pem
```

* puis le certificat root :

```
wget https://www.startssl.com/certs/ca.pem
```

puis on forge le certificat chaîné (ne pas oublier les 2 >>) :

```
cat exemple.tld.crt >> /etc/ssl/private/exemple.tld.chain
cat sub.class1.server.ca.pem >> /etc/ssl/private/exemple.tld.chain
cat ca.pem >> /etc/ssl/private/exemple.tld.chain
```

Et le tour est joué* \o/

<small>* si vous n'avez pas oublié de rajouter la ligne suivante dans le fichier de configuration de votre vhost :

```
SSLCertificateChainFile /etc/ssl/private/exemple.tld.chain
```
</small>

### s_client

Permet de faire des tests grâce à un client SSL (cf [wiki.openssl.org](https://wiki.openssl.org/index.php/Command_Line_Utilities#s_client))

```
openssl s_client -servername www.libwalk.so -connect www.libwalk.so:443
```

Attention à ne pas oublier `-servername` et le nom du site que vous voulez tester, sinon la commande prendra le certificat présenté par défaut sur le serveur (et donc pas forcément le bon...).

Exemple :

* **avec** `-servername www.libwalk.so`

```
openssl s_client -servername www.libwalk.so -connect www.libwalk.so:443

CONNECTED(00000003)
depth=1 C = IL, O = StartCom Ltd., OU = Secure Digital Certificate Signing, CN = StartCom Class 1 Primary Intermediate Server CA
[...]
 0 s:/description=clwEqu9Rv2hyAXKy/C=FR/CN=www.libwalk.so/emailAddress=postmaster@libwalk.so
```

 * **sans** `-servername www.libwalk.so`

```
openssl s_client -connect www.libwalk.so:443

CONNECTED(00000003)
[...]
0 s:/OU=Domain Control Validated/OU=PositiveSSL Wildcard/CN=*.octopuce.fr
```

Pour plus de précisions, vous pouvez lire "*[Using OpenSSL’s s_client command with web servers using Server Name Indication (SNI)](https://major.io/2012/02/07/using-openssls-s_client-command-with-web-servers-using-server-name-indication-sni/)*" pour faire connaissance avec notre ami le *SNI* (*Server Name Indication*).

#### OpenSSL sur mon système :

```
openssl ciphers -v 'ALL:COMPLEMENTOFFALL'
```

Ce qui est activé par défaut (vous pouvez noter la présence des suites d'EXPort) :

```
openssl ciphers -v 'DEFAULT'
openssl ciphers -v 'ALL:!aNULL:!eNULL'
```

```
openssl list-cipher-commands
openssl list-message-digest-commands
```

Vous pouvez "créer" votre suite avec [les critères suivants](https://www.openssl.org/docs/apps/ciphers.html#CIPHER_STRINGS) (algo de chiffrement, de hashage, d'authentification...), la question que vous allez poser devrait donc être "ok, mais on fait comment ?".

> **Note** : vous pouvez à tout moment regarder ce que contient [un ensemble d'algos](https://www.openssl.org/docs/apps/ciphers.html#CIPHER_STRINGS) avec la commande que l'on vient de voir. Si par exempke vous voulez voir ce que `EXP` contient, vous pouvez le faire avec la commande `openssl ciphers -v 'EXP'`.

On prend tout (`ALL`), on enlève les algos qui ne permettent pas ni l'authentification (`!aNULL`), ni chiffrement (`!eNULL`), on continue en enlevant `!LOW` (chiffrement avec 56 ou 64 bits, la norme est de 128 bits de nos jours), les algos d'export (`!EXP`) et l'algo RC4 (`!RC4`) qui est considéré comme [dangereux](https://twitter.com/ioerror/status/398059565947699200).

#### pour aller plus loin (mais vraiment très, très loin) :

* un seul livre à lire : l'[inégalable](http://www.amazon.com/Bulletproof-SSL-TLS-Understanding-Applications/dp/1907117040/ref=asap_bc?ie=UTF8) "*[Bulletproof SSL and TLS](https://www.feistyduck.com/books/bulletproof-ssl-and-tls/)*" d'Ivan Ristić.
* mais sinon, en plus court et en gratuit, il y a [openssl cookbook](https://www.feistyduck.com/books/openssl-cookbook/) du même auteur, ainsi que "*[SSL/TLS Deployment Best Practices](https://www.ssllabs.com/downloads/SSL_TLS_Deployment_Best_Practices.pdf)*" (merci [matlink](https://fr.matlink.fr/) pour le rappel).

