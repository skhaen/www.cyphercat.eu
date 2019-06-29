+++
author = "Skhaen"
categories = ["ssl", "adminsys"]
date = 2014-10-15T15:35:00Z
description = ""
draft = false
slug = "die-sslv3-die"
tags = ["ssl", "adminsys"]
title = "DIE SSLv3, DIE!"

+++

Vous avez sans doute vu passer la faille concernant le protocole SSLv3 trouvée par une équipe de Google et annoncée dans la nuit du 14 octobre 2014 (<em>POODLE</em>, pour <em>[Padding Oracle On Downgraded Legacy Encryption](https://www.openssl.org/~bodo/ssl-poodle.pdf)</em>). Cette faille concerne directement le protocole et ne peut pas être corrigée simplement. La solution la plus saine est de désactiver SSLv3 en vérifiant bien que TLSv1.0, TLSv1.1 et TLSv1.2 fonctionnent.

* Annonce de Google sur  : <a href="http://googleonlinesecurity.blogspot.de/2014/10/this-poodle-bites-exploiting-ssl-30.html">googleonlinesecurity.blogspot.de</a> ou <a href="https://www.openssl.org/~bodo/ssl-poodle.pdf">là en pdf</a> (security advisory),
* Explication technique de la faille par Adam Langley : <a href="https://www.imperialviolet.org/2014/10/14/poodle.html">POODLE attacks on SSLv3</a>
* Explication en français sur nextinpact.com : <a href="http://www.nextinpact.com/news/90427-la-faille-poodle-sslv3-permet-decrypter-donnees-sur-connexion.htm">La faille POODLE de SSLv3 permet de décrypter les données sur une connexion</a>.
* Mozilla : <a href="https://blog.mozilla.org/security/2014/10/14/the-poodle-attack-and-the-end-of-ssl-3-0/">The POODLE Attack and the End of SSL 3.0</a>

Il faut par contre noter que <a href="http://fr.wikipedia.org/wiki/Internet_Explorer_6">Internet Explorer 6</a> sous Windows XP ne pourra plus se connecter de manière "sécurisée". Pour rappel, IE6 n'est plus maintenu par Microsoft depuis 2010, le bleu sur la carte ci-dessous signifie que <a href="https://www.modern.ie/en-us/ie6countdown">IE6 est à moins de 1% de parts de marché des navigateurs dans cette zone</a> et moins de 0.8% des navigateurs en France : 


![IE6](/content/images/2015/09/ie6world.png)

<center>**<a href="https://www.modern.ie/en-us/ie6countdown">IE6 représente largement moins de 1% des navigateurs</a> en Europe et en Amérique du nord**.</center>


## Dans les navigateurs

#### Pour [Mozilla Firefox](https://wiki.mozilla.org/Security/Server_Side_TLS)

Après avoir ouvert votre navigateur, tapez <code>about:config</code> dans votre barre d'adresse, après avoir cliqué sur <em>"je ferai attention, promis"</em>, vous arrivez dans la configuration de votre navigateur. Vous pouvez alors rechercher <code>security.tls.version.min</code> dans la barre de recherche. Le but ici est de modifier la valeur minimum pour la version de TLS, il suffit donc de mettre <code>security.tls.version.min</code> à 1 pour dire à Firefox d'utiliser TLSv1.0 au minimum (et non SSLv3). Il faut redémarrer son navigateur pour prendre en compte cette modification.

![](/content/images/2015/09/firefoxssl.png)

À noter que <a href="https://blog.mozilla.org/security/2014/10/14/the-poodle-attack-and-the-end-of-ssl-3-0/">SSLv3 sera désactivé par défaut à partir de Mozilla Firefox 34</a>.

#### Pour Google Chrome/Chromium

Selon Adam Langley (Senior Staff Software Engineer, Google), Google Chrome implémente depuis Février <code>TLS_FALLBACK_SCSV</code> qui empêche  les protocoles de descendre en SSLv3. Si vous voulez vous passer définitivement de SSLv3, il faut lancer Chrome (et Chromium) avec l'option <code>--ssl-version-min=tls1</code> (cf. "<a href="http://www.chromium.org/for-testers/command-line-flags">How to specify command line flags</a>" sur chromium.org)

#### Pour Internet Explorer

Pour IE6, la méthode est très simple : arrêtez de l'utiliser, passez à <a href="https://www.mozilla.org/fr/">Mozilla Firefox</a> ! Pour les autres versions, vous pouvez activer/désactiver les protocoles dans <em>outils &gt; options Internet &gt; avancé</em>. Vous pouvez désactiver tout en bas SSLv3 et activer TLSv1.1 et TLSv1.2 si ce n'est pas le cas (et vérifiez bien que SSLv2 est AUSSI désactivé, on ne sait jamais).

![](/content/images/2015/09/ie8ssl.png)

<center>Capture d'écran d'un IE8 sous Microsoft Windows (oui, c'est vieux).</center>

## Sur les serveurs

N'oubliez pas de recharger la configuration de vos logiciels en cas de modification (<code>apache2ctl -t</code> pour tester votre configuration puis <code>service apache2 reload</code> pour prendre en compte la nouvelle conf pour Apache2).

#### Configuration <a href="https://httpd.apache.org/docs/2.2/mod/mod_ssl.html#sslprotocol">Apache</a> avec OpenSSL

    SSLProtocol All -SSLv2 -SSLv3

Nous vous conseillons grandement d'éditer le fichier <code>/etc/apache2/mods-enabled/ssl.conf</code> pour améliorer la configuration par défaut du serveur en modifiant au minimum la ligne <code>SSLProtocol</code> pour la suivante : `SSLProtocol -SSLv2 -SSLv3`.

#### Configuration <a href="http://nginx.org/en/docs/http/configuring_https_servers.html">Nginx</a> avec OpenSSL

```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
```

#### Configuration <a href="http://wiki2.dovecot.org/SSL">Dovecot</a>

```
ssl_cipher_list = ALL:!LOW:!SSLv2:!SSLv3:!EXP:!aNULL:!RC4:!3DES
```
        
#### Configuration <a href="http://www.courier-mta.org">Courier </a>pop &amp; imap

Mieux vaut utiliser Dovecot, vous ne voulez pas jouer avec imapd-ssl et pop3d-ssl.
Sinon, dans imapd-ssl et pop3d-ssl, remplacez (ou ajoutez-là si besoin) la ligne <code>TLS_PROTOCOL</code> comme ceci :

```
TLS_PROTOCOL="TLS1_2:TLS1_1:TLS1"
```

Attention, les anciennes versions de Courier ne prennent pas en compte <code>TLS1_2</code>.

#### Configuration <a href="http://www.postfix.org/TLS_README.html#server_cipher">Postfix</a> smtp

```
smtpd_tls_mandatory_protocols = !SSLv2, !SSLv3
smtpd_tls_mandatory_ciphers=high
```

#### Configuration <a href="http://prosody.im/doc/advanced_ssl_config">Prosody</a>

```
options = { "no_sslv2", "no_sslv3", "no_ticket", "no_compression","cipher_server_preference", "single_dh_use","single_ecdh_use" };
```

#### Configuration <a href="https://www.ejabberd.im/">eJabberd</a>

Ce n'est apparemment pas possible pour le moment.

#### Configuration <a href="https://community.openvpn.net/openvpn/wiki/Hardening?version=6#Useof--tls-cipher">OpenVPN</a>

Pour améliorer la sécurité d'OpenVPN, se référer à l'article <a href="https://community.openvpn.net/openvpn/wiki/Hardening?version=6#Useof--tls-cipher">Hardening OpenVPN</a>

###Comment je peux tester ma configuration ?

Sur le plan général, je vous conseille aussi le très bon "<a href="https://bettercrypto.org/static/applied-crypto-hardening.pdf">Applied crypto hardening</a>" qui ne demande qu'à être améliorer. Tout comme [JeveuxHTTPS.fr](http://jeveuxhttps.fr/). Vous pouvez tester votre configuration avec le très, très bon <a href="http://ssllabs.com/">SSLLabs.com</a>, ou encore avec les commandes suivantes dans un terminal : 

#### Avec OpenSSL
Avec la commande <code>OpenSSL s_client</code>:

```
openssl s_client -connect HOST:PORT OPTION
```

Par exemple avec la commande suivante pour voir si j'ai du SSLv3 sur www.libwalk.so (les options qui peuvent vous intéresser ici sont les suivantes : <code>-ssl2</code>, <code>-ssl3</code>, <code>-tls1</code>, <code>-no_ssl2</code>, <code>-no_ssl3</code>, <code>-no_tls1</code>).

```
openssl s_client -connect www.libwalk.so:443 -ssl3
```
#### Avec nmap

```
nmap --script ssl-enum-ciphers -p 443 exemple.org
```

