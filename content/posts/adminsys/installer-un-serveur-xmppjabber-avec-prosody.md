+++
author = "Skhaen"
categories = ["adminsys", "ssl", "prosody", "xmpp"]
date = 2014-02-14T18:00:00Z
description = ""
draft = false
slug = "installer-un-serveur-xmppjabber-avec-prosody"
tags = ["adminsys", "ssl", "prosody", "xmpp"]
title = "Installer un serveur XMPP/jabber avec Prosody sous Debian"

+++

**Sous-titre** : comment installer Prosody 0.9.3 sous Debian et avoir un A sur [xmpp.net](https://xmpp.net).
### update

* 2015-03-19 : comment avoir un certificat autosigné qui fonctionne avec les versions "récentes" de pidgin (voir EN BAS de cet article)
* 2015-01-05 : on oublie bettercryto, et l'on rajoute une jolie ciphersuite pour avoir seulement du PFS
* 2014-12-30 : ajout de la section [bettercrypto](https://bettercrypto.org) concernant TLS
* 2014-12-12 : ajout de la [ciphersuite](http://prosody.im/doc/advanced_ssl_config#ciphers) dans la partie SSL/TLS
* 2014-08-22 : refonte du texte, changement du fournisseur du certificat SSL/TLS de CaCert pour [starssl](https://www.startssl.com/)


* Site de [Prosody](http://prosody.im),
* Twitter : [@prosodyim](https://twitter.com/prosodyim).

# Installation

Pour installer [Prosody](http://prosody.im) sous Debian, il suffit de rajouter le dépôt dans son *[sources.list](https://wiki.debian.org/SourcesList)*, on peut utiliser la ligne suivante pour le faire. Elle permet de simplifier énormement la manipulation (pour les détails, c'est [ici](http://prosody.im/download/package_repository)) :

```
echo "deb http://packages.prosody.im/debian $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list
```

Puis on ajoute la clé GPG du dépôt :

```
wget https://prosody.im/files/prosody-debian-packages.key -O- | sudo apt-key add -
```

On met à jour les dépôts, puis on installe Prosody :

```
aptitude update && aptitude install prosody
```

Le paquet [**lua-sec**](https://packages.debian.org/wheezy/lua-sec)  dans debian est légèrement... ancien, si vous voulez les derniers algos de chiffrement et que le *[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy)* soit pris en compte, il faut le mettre à jour :

sur Debian Wheezy :

```
echo "deb http://ftp.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list
aptitude update && aptitude -t wheezy-backports install lua-sec-prosody
```

# Configuration

L'ensemble de la configuration se fait dans un seul fichier : 

```
/etc/prosody/prosody.cfg.lua
```

La première partie est commune à l'ensemble des VirtualHosts que vous allez mettre en place. Ce qui sera écrit sous chaque *VirtualHost "exemple.com"* (en bas) sera propre à chaque VirtualHost.

Ce qui veut dire que si vous mettez votre configuration SSL dans la première partie, elle sera commune à **tout** les VirtualHosts. Faites attention aux répercussions que ça peut avoir ;-)

## Étape 1 - la première brique

Ouvrez le fichier avec votre éditeur préféré (pour moi ça sera <a href="https://fr.wikipedia.org/wiki/Vim">vim</a>)

```
vim /etc/prosody/prosody.cfg.lua
```

Il faut [modifier/compléter les champs suivants](http://prosody.im/doc/configure) (avec évidemment **vos** informations à la place de *example.tld*) :

Qui est administrateur du serveur (tout en haut du fichier) ? 

```
admins = {"admin@exemple.tld"}
```

Voulez vous autoriser les inscriptions sur votre serveur ? À moins de savoir ce que vous faites, je vous conseille grandement de le mettre sur `false` afin d'éviter de servir aux spammeurs et aux bots.

```
allow_registration = false;
```
    
Quel domaine voulez vous utiliser (tout en bas du fichier) ? N'oubliez pas de changer *example.tld* pour votre domaine.
	
```
VirtualHost "example.tld"
enabled = true
```

### Les logs

c'est **très** important (et déjà partiellement configuré). Ils permettent de trouver où est le problème quand ça ne marche pas (pour plus de détails sur la configuration : [logging](http://prosody.im/doc/logging)), la modification par rapport au fichier original consiste à mettre le *<a href="http://prosody.im/doc/logging">prosody.log</a>* en *warn* et non plus en *info* et faire la même manipulation pour le *<a href="http://prosody.im/doc/advanced_logging">syslog</a>* :

```
log = {
    warn= "/var/log/prosody/prosody.log";
    error = "/var/log/prosody/prosody.err";
    { levels = { "error" }; to = "syslog";  };

```
Si vous avez un problème à régler, je vous conseille de rechanger le premier *warn* pour *debug*, puis de redémarrer prosody (via la commande `prosodyctl restart`, vous aurez alors toutes les infos qu'il vous faut dans `/var/log/prosody/prosody.log` et bien plus encore pour trouver d'où ça vient.

### Un minimum de sécurité

De base, Prosody [enregistre les mots de passe des comptes en clair](http://prosody.im/doc/modules/mod_auth_internal_hashed) sur le serveur, pour qu'ils soient [hashés](http://www.cyphercat.eu/expl_hashage.php), il faut modifier la ligne `authentication = "internal_plain"` »  pour la ligne suivante : 

```
authentication = "internal_hashed"
```

### prosodyctl

Prosody vient avec une commande qui aide à communiquer avec lui facilement : *[prosodyctl](http://prosody.im/doc/prosodyctl)*, et qui s'utilise sous la forme suivante :

```
prosodyctl COMMAND [OPTIONS]
```

Où *COMMAND* peut être :

* `adduser` - exemple@exemple.tld - crée le compte utilisateur
* `passwd` - exemple@exemple.tld - configure le password de l'utilisateur
* `deluser` - exemple@exemple.tld - supprime l'utilisateur
* `start` - démarre Prosody
* `stop` - arrête Prosody
* `restart` - redémarre Prosody
* `reload` - recharge la configuration de Prosody
* `status` - indique le status actuel de Prosody

Plutôt utile non ? :]

Nous allons ajouter un utilisateur (il vous demandera un mot de passe), puis nous rechargerons la configuration pour prendre en compte les changements (`exemple.tld` doit être le même que vous avez mis pour votre `VirtualHost`)  :

```
prosodyctl adduser exemple@exemple.tld
prosodyctl reload
```

Et là, c'est l'instant de plaisir, si vous essayez de vous connecter avec un client jabber (comme pidgin),  ça devrait se connecter. Si ce n'est pas le cas (faire dans l'ordre) :

* voir les logs de prosody,
* voir les logs de votre client jabber,
* changer les logs de prosody de info à debug,
* boire un thé (ou une bière)
* vérifier votre firewall,
* rebrancher la box,
* reprendre un thé,
* changer de FAI,

### Les modules

Comme vous pouvez le voir dans le fichier de configuration, il y a déjà un certain nombres de [modules](http://prosody.im/doc/developers/global_modules) qui sont [activés par défaut](http://prosody.im/doc/modules_enabled) (roster, saslauth, tls, posix...). ils sont dans la partie `modules_enabled = { }`.

Si je vous parle de ça, vous vous doutez que ce n'est pas pour rien, il existe beaucoup de modules, dont un pour [Tor](https://www.torproject.org/) :

* liste des modules  : [core modules](https://prosody.im/doc/modules),
* liste des modules tiers : [prosody-modules](https://code.google.com/p/prosody-modules/w/list),
* mod_onions : [XMPP Federation Over Tor Hidden Services](https://blog.thijsalkema.de/blog/2013/06/11/xmpp-federation-over-tor-hidden-services/).


## Chiffrons tout pour tout le monde

> Manifesto - *[A public statement about ubiquitous encryption on the XMPP network.](https://github.com/stpeter/manifesto)*


Si vous ne connaissez pas (ou mal) le SSL/TLS, je ne peux que vous conseiller d'attaquer le sujet par la conférence de Benjamin Sonntag dans le cadre du cycle de conférence « *[Il était une fois Internet](http://iletaitunefoisinternet.fr)* ».

### Création du certificat


> Si votre certificat n'est pas reconnu « valide » par votre client jabber, par exemple en cas de certificat auto-signé, il demandera autorisation pour l'utiliser. Les versions récentes de pidgin (> v2.10.10) [refusent de se connecter](http://pidgin.10357.n7.nabble.com/16412-Unable-to-connect-to-XMPP-servers-using-self-signed-certificates-td130960.html) à des serveurs utilisant un certificat autosigné, dans ce cas, voir EN BAS de cet article

#### La méthode simple (et vraiment rapide)

La méthode « *simple* » consiste à la création d'un [certificat auto-signé](http://prosody.im/doc/certificates) avec l'aide de **prosodyctl** :

```
# exemple.tld étant le domaine que vous avez mis dans prosody.cfg.lua
prosodyctl cert generate exemple.tld
```

* vérifier l'emplacement des certificats une fois qu'ils sont générés
* vous pouvez les bouger dans `/etc/prosody/certs/` si vous avez envie (on part du principe que c'est le cas pour la suite)
* N'oubliez pas les deux lignes suivantes :

```
chmod 600 /etc/prosody/certs/exemple.tld.key
chown prosody:prosody /etc/prosody/certs/exemple.tld.key
chown prosody:prosody /etc/prosody/certs/exemple.tld.crt
```

Pour aller plus loin :

* Prosody - [certificats](http://prosody.im/doc/certificates),
* Prosody - [configuration SSL avancé](http://prosody.im/doc/advanced_ssl_config).


#### La méthode un peu moins simple mais qui est vachement mieux

Nous allons créer ce que nous avons besoin en faisant une requête à [startssl.com](hhttp://www.startssl.com/) :

```
openssl req -sha256 -out /etc/prosody/certs/exemple.csr -new -newkey rsa:2048 -nodes -keyout /etc/prosody/certs/exemple.key
```

Cette commande génère (`openssl req -out`) une [CSR](https://en.wikipedia.org/wiki/Certificate_signing_request) (`-new`) ainsi qu'une nouvelle [clé RSA](https://fr.wikipedia.org/wiki/Chiffrement_RSA) de 2048 bits (`-newkey rsa:2048`) qui ne sera pas chiffrée (`-nodes`). Si vous voulez encore plus de détails, c'est sur [openssl.org](https://www.openssl.org/docs/apps/req.html#).

Je vous laisse sur [startssl.com](hhttp://www.startssl.com/) et réaliser les différentes étapes pour s'inscrire (bonne chance !), il faut choisir la **première formule** (celle qui est gratuite). Après la création de votre compte, la validation de votre nom de domaine, etc etc... nous pouvons passer à la suite.

* Après votre authentification sur le site, cherchez l'onglet « *Certificates Wizard* » qui se trouve normalement vers la gauche de votre écran,
* il vous demande alors pourquoi vous voulez un certificat ("*Select Certificate Purpose*"), vous choisissez donc **XMPP (jabber) SSL/TLS certificate**,
* le site vous demande de créer une clé privée ("*Generate Private Key*") : **VOUS NE LE FAITES PAS !**, vous appuyez au contraire sur **skip** (nous avons créé la clé privée quelques lignes au dessus),
* vous arrivez maintenant sur la page de demande de certificat ("*Submit Certificate Request (CSR)*"), toujours dans un terminal, vous devez taper la commande suivante :

```
cat /etc/prosody/certs/exemple.csr
```

Cette commande vous donnera un résultat qui devrait ressembler à 

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


Copier le dans la fenêtre en bas (de `-----BEGIN CERTIFICATE REQUEST-----` à `-----END CERTIFICATE REQUEST-----` !). 
Vous aurez alors votre certificat peu de temps après, transférez le sur votre serveur, dans `/etc/prosody/certs/` avec comme nom exemple.tld.**crt**.

### Création d'un certificat chaîné

Pour créer un [certificat chaîné](http://prosody.im/doc/certificates#certificate_chains), faites la manipulation suivante (startssl.com vous donne les liens vers ces deux certificats juste au dessus du bouton **finish »»** de la page **Save Certificate**) :

* télécharger le certificat intermédiaire de startssl :

```
wget https://www.startssl.com/certs/sub.class1.server.ca.pem
```

* puis le certificat root : 

```
wget https://www.startssl.com/certs/ca.pem
```

* puis on forge le certificat chaîné (ne pas oublier les **2** `>>`) :

```
cat sub.class1.server.ca.pem >> /etc/prosody/certs/exemple.tld.crt
cat ca.pem >> /etc/prosody/certs/exemple.tld.crt
```

### Génération du dhparam

Création du **dhparam** ([prosody.im/dhparam](https://prosody.im/doc/advanced_ssl_config#dhparam)) :
 
```
openssl dhparam -out /etc/prosody/certs/dh-2048.pem 2048
```

<blockquote>Java ne peut pas utiliser un dhparam supérieur à 1024 bits, ce qui empêche d'utiliser par exemple le logiciel [Jitsi](https://jitsi.org). Pour éviter cela (c'est tout de même un client grandement utilisé), on peut générer un dhparam de 1024 bits avec la commande suivante (ne pas oublier de modifier la configuration pour remplacer dh-2048.pem par dh-1024.pem):</p>

```
openssl dhparam -out /etc/prosody/certs/dh-1024.pem 1024
```

</blockquote>

### Configuration du SSL/TLS

On retourne dans */etc/prosody/prosody.cfg.lua* : 

```
ssl = {
    key = "/etc/prosody/certs/exemple.tld.key";
    certificate = "/etc/prosody/certs/exemple.tld.cert";
    dhparam = "/etc/prosody/certs/dh-2048.pem";
    options = { "no_sslv2", "no_sslv3", "no_ticket", "no_compression",
    "cipher_server_preference", "single_dh_use",
    "single_ecdh_use" };
    ciphers = "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH EDH+aRSA !RC4 !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS";
    }
```

* **c2s** signifie « *Client TO Server* »,
* **s2s** signifie « *Server TO Server* ».

```
c2s_ports = { 5222 }
s2s_ports = { 5269 }
c2s_require_encryption = true
s2s_require_encryption = true
s2s_secure_auth = false
allow_unencrypted_plain_auth = false;
```

***s2s_secure_auth*** est sur *false* pour les [raisons suivantes](http://prosody.im/doc/s2s#security) ([TL;DR](http://www.urbandictionary.com/define.php?term=tl%3Bdr): possibilité de certificat auto-signé en face). Peut marcher avec *s2s_auth_fingerprint* mais c'est douloureux.

Les [options](https://prosody.im/doc/s2s#security) *s2s_secure_domains* et *s2s_insecure_domains*  peuvent aussi vous intéresser, surtout si vous voulez parler avec des personnes qui sont sur gmail (ce qui veut dire que ça ne sera pas chiffré).

Concernant la **ciphersuite**, [le choix par défaut](http://prosody.im/doc/advanced_ssl_config#ciphers) de Prosody est déjà pas mal :

```
ciphers = "HIGH+kEDH:HIGH+kEECDH:HIGH:!PSK:!SRP:!3DES:!aNULL";
```

Il faut tout de même enlever `RC4` et `eNULL`, et tant qu'à faire, autant faire une liste de ciphers avec seulement du PFS ([Perfect Forward Secrecy](https://fr.wikipedia.org/wiki/Confidentialit%C3%A9_persistante)) (c'est à mettre dans le bloc `ssl { }`) :

```
ciphers = "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH EDH+aRSA !RC4 !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS";
```


#### DNS

N'oubliez pas d'ajouter les enregistrements [SRV dans vos DNS](https://fr.wikipedia.org/wiki/Enregistrement_SRV) :

```
_xmpp-client._tcp.jabber        IN      SRV     0 5 5222 host.exemple.com.
_xmpp-server._tcp.jabber        IN      SRV     0 5 5269 host.exemple.com.
```


#### Firewall

En cas de firewall, il faut ouvrir les ports 5269 et 5222 (IN et OUT) en **TCP**. La configuration pour `iptables` donne ceci :

```
iptables -I INPUT -p tcp --dport 5269 -j ACCEPT
iptables -I OUTPUT -p tcp --sport 5269 -j ACCEPT
iptables -I INPUT -p tcp --dport 5222 -j ACCEPT
iptables -I OUTPUT -p tcp --sport 5222 -j ACCEPT
```


#### Avoir un certificat autosigné qui passe bien (par exemple pour un hidden service tor)

Les versions récentes de pidgin (> v2.10.10) [refusent de se connecter](http://pidgin.10357.n7.nabble.com/16412-Unable-to-connect-to-XMPP-servers-using-self-signed-certificates-td130960.html) à des serveurs utilisant un certificat autosigné. C'est dû à une option de [X509](https://fr.wikipedia.org/wiki/X.509), [basicConstraints](https://www.openssl.org/docs/apps/x509v3_config.html#Basic-Constraints), qui ne doit pas contenir `CA:false`

Nous allons faire une copie du fichier `/etc/ssl/openssl.cnf` sur le poste servant à générer nos clés, afin de pouvoir l'éditer et histoire de pouvoir revenir en arrière sans problème  :


```
cp /etc/ssl/openssl.cnf /var/tmp/openssl2.cnf
```

On édite le fichier avec son éditeur préféré (ici `vim`) :

```
vim /etc/ssl/openssl.cnf
```

Et l'on modifie certaines lignes pour avoir une bonne configuration par défaut :

Dans la partie `[ req ]`, vérifiez bien que `default_md` est présent avec comme valeur (au minimum) `sha256` :

```
[ req ]
default_md              = sha256
default_bits            = 2048
default_keyfile         = privkey.pem
distinguished_name      = req_distinguished_name
attributes              = req_attributes
x509_extensions         = v3_ca
string_mask             = nombstr
```


Dans la partie `[ v3_ca ]`, vérifiez bien la présence de `CA:true` :


```
[ v3_ca ]

# Extensions for a typical CA
subjectKeyIdentifier=hash
authorityKeyIdentifier=keyid:always,issuer
basicConstraints = CA:true

```

Pour finir, dans la partie `[ req_distinguished_name ]`, vous pouvez commenter la plupart des ligne pour éviter les questions chiantes et ne garde que les 2 suivantes :

<blockquote>Vous pouvez bien entendu les garder et les pré-remplir si vous en avez besoin, si vous avez un doute sur la marche à suivre, vous pouvez passer cette partie sans problème</blockquote>

```
[ req_distinguished_name ]

commonName      = Your FQDN
commonName_max  = 256
```

Il suffira ensuite de lancer les 3 commandes suivantes pour générer nos clés avec la bonne configuration :

> Dans cet exemple, nous générons une **clé de 2048 bits** (`genrsa [...] 2048`) en utilisant notre fichier openssl.cnf (`-config openssl.cnf`) pour **une durée de 10 ans** (`-days 3650`) </blockquote>

```
openssl genrsa -out server.key 2048
openssl req -config /var/tmp/openssl2.cnf -new -key server.key -out server.csr
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
```

## Test

Comme [SSLLabs](https://www.ssllabs.com/) pour tester le SSL/TLS sur les serveurs webs, il existe [xmpp.net](https://xmpp.net/) pour les serveurs xmpp. Je vous conseille **vivement** de tester votre site ! 

Il faut **toujours** vérifier, même quand on pense pouvoir avoir confiance.

Si vous vous demandez comment améliorer votre note, c'est là : [xmpp.net/about](https://xmpp.net/about.php).

