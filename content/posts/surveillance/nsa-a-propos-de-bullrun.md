+++
author = "Skhaen"
categories = ["SIGINT", "leaks", "adminsys"]
date = 2015-01-25T16:18:00Z
description = ""
draft = false
slug = "nsa-a-propos-de-bullrun"
tags = ["SIGINT", "leaks", "adminsys"]
title = "[Snowden] NSA - À propos de BULLRUN"

+++

Dans cet article, nous allons revenir sur les révélations du Spiegel datant de fin décembre lors du 31c3 (Chaos Computer Congress) et du 17 janvier 2015 portant sur les moyens offensifs de la NSA ainsi que d'autres agences concernant la cryptographie.


La conférence "*[Reconstructing narratives](https://media.ccc.de/browse/congress/2014/31c3_-_6258_-_en_-_saal_1_-_201412282030_-_reconstructing_narratives_-_jacob_-_laura_poitras.html#video)*" de Laura Poitras et Jacob Appelbaum présentant ces [documents](http://www.spiegel.de/international/world/nsa-documents-attacks-on-vpn-ssl-tls-ssh-tor-a-1010525.html) est visible ci-dessous (mais aussi sur le site du [CCC](http://media.ccc.de/browse/congress/2014/31c3_-_6258_-_en_-_saal_1_-_201412282030_-_reconstructing_narratives_-_jacob_-_laura_poitras.html#video)).


## BULLRUN, qu'est ce que c'est ?

BULLRUN est un "programme" de la NSA exploitant différents moyens pour accéder à du contenu chiffré. Le New York Times avait abordé le sujet fin 2013 dans son article "*[Secret Documents Reveal N.S.A. Campaign Against Encryption](http://www.nytimes.com/interactive/2013/09/05/us/documents-reveal-nsa-campaign-against-encryption.html?_r=0)*" mais sans aucun détails (comme [The Guardian](http://www.theguardian.com/world/interactive/2013/sep/05/nsa-project-bullrun-classification-guide) ou encore [propublica](http://www.propublica.org/article/the-nsas-secret-campaign-to-crack-undermine-internet-encryption)).

On savait, à l'époque, que l'on pouvait distinguer en simplifiant trois méthodes que la NSA utilise pour pouvoir accéder à du contenu chiffré :

* utiliser les mathématiques, c'est à dire trouver de nouvelles méthodes pour réussir à casser un algorithme (par exemple pour ["casser" RC4](http://blog.cryptographyengineering.com/2013/03/attack-of-week-rc4-is-kind-of-broken-in.html)),

* La deuxième méthode permet "simplement" d'accéder aux [clés privées](https://fr.wikipedia.org/wiki/Cryptographie_asym%C3%A9trique#Principe_g.C3.A9n.C3.A9ral) de la cible (pouvant aussi bien être une personne qu'une multinationale) ou aux informations demandées. On arrive là dans [un ensemble d'autres programmes](https://firstlook.org/theintercept/2014/10/10/core-secrets/), un des plus secrets de la NSA (classification est "[CORE SECRETS](https://firstlook.org/theintercept/2014/10/10/core-secrets/)"). On trouve dans les documents que les agents peuvent être sous couverture dans une entreprise (qu'elle soit américaine ou étrangère), ou encore que le programme [TAREX](https://www.nsa-observer.net/category/program/family/target/name/TAREX) (pour TARget EXploitation) conduit des opérations clandestines aussi bien [SIGINT](https://fr.wikipedia.org/wiki/Renseignement_d%27origine_%C3%A9lectromagn%C3%A9tique) (renseignement d'origine électromagnétique) qu'[HUMINT](https://fr.wikipedia.org/wiki/Renseignement_humain) (renseignement humain) partout dans le monde pour exploiter des systèmes via différents moyens : “*off net-enabling*” (activité clandestine ou sous couverture sur le terrain), “*supply chain-enabling*” (modifier des équipements directement dans la chaîne logistique ou via le détournement de livraisons) et “*hardware implant-enabling*” qui semble regrouper un peu des deux précédents.

Les États-unis ne sont évidemment pas obligé de passer par ce genre de d'opérations pour obtenir ce qu'ils veulent de leurs entreprises, il existe le "*[Foreign Intelligence Surveillance Act](https://en.wikipedia.org/wiki/Foreign_Intelligence_Surveillance_Act)*" (FISA) et les "*[lettres de sécurité nationale](https://fr.wikipedia.org/wiki/Lettres_de_s%C3%A9curit%C3%A9_nationale)*" qui sont des [requêtes contraignantes](https://fr.wikipedia.org/wiki/Lettres_de_s%C3%A9curit%C3%A9_nationale) et qui peuvent obliger une entreprise à permettre un accès à quelque chose [en ayant l'obligation de ne pas en parler](https://en.wikipedia.org/wiki/Gag_order).

Ainsi, en 2013, l'entreprise Lavabit décida de fermer plutôt que de [donner sa clé privée SSL/TLS au FBI](http://www.vanityfair.fr/actualites/international/articles/ladar-levison-lhomme-qui-defie-le-renseignement-americain/1369), le tribunal la menaçait d'une amende de 5000 € par jour de retard. Lavabit herbergait les mails d'Edward Snowden parmis ses 400 000 utilisateurs.

En 2008, Yahoo a été menacé d'une [amende de 250 000 $ par jour de retard](http://www.theguardian.com/world/2014/sep/11/yahoo-nsa-lawsuit-documents-fine-user-data-refusal) si il ne donnait pas des données d'utilisateurs à la NSA


* La troisième méthode consiste à mettre en place ou profiter d'une mauvaise implémentation, comme le générateur de nombre aléatoire [Dual_EC_DRBG](https://en.wikipedia.org/wiki/Dual_EC_DRBG), qui pourrait permettre, par exemple, de [lire des flux SSL/TLS](http://blog.cryptographyengineering.com/2013/09/the-many-flaws-of-dualecdrbg.html). Une méthode complémentaire consiste à payer une entreprise pour qu'elle utilise quelque chose en particulier,  la NSA à payé [10 millions de dollars](http://www.reuters.com/article/2014/03/31/us-usa-security-nsa-rsa-idUSBREA2U0TY20140331) à l'entreprise RSA, pour utiliser Dual_EC_DRBG dans certains de ses produits (comme [BSAFE](https://en.wikipedia.org/wiki/RSA_BSAFE)).

Ainsi, la NSA (et sans doute les autres agences) a activement travaillé à insérer des vulnérabilitées dans des produits commerciaux, des réseaux (par exemple en se connectant à un routeur pour diminuer la crypto d'un VPN), des protocoles (vous pouvez [lire les spéculations de John Gilmore sur la NSA et IPsec](http://www.mail-archive.com/cryptography@metzdowd.com/msg12325.html)) ou directement sur des périphériques de cibles.

Le [New York Times](http://www.nytimes.com/interactive/2013/09/05/us/documents-reveal-nsa-campaign-against-encryption.html?_r=0) rapporte qu'en 2006, la NSA avait réussi à pénétrer les communications de trois compagnies aériennes, un système de réservation de voyages, un programme nucléaire d'un gouvernement étranger en craquant les VPNs les protégeant, et en 2010, EDGEHILL (l'équivalent Britannique de BULLRUN) avait réussi à "déchiffrer" le traffic de 30 cibles.


## À propos de configurations

Pour ce que l'on en sait, il suffit d'une bonne configuration pour résoudre la majorité des problèmes (à noter tout de même : les documents datent de 2012, et énormement de choses ont pu changer depuis (Heartbleed, Poodle, nouvelles failles, etc).



### SSL/TLS

Le cryptographe [Matthew Green](https://www.blogger.com/profile/05041984203678598124) a fait [un article](http://blog.cryptographyengineering.com/2013/12/how-does-nsa-break-ssl.html) sur les différents moyens que la NSA a pour "casser" SSL/TLS. Selon lui, la NSA peut utiliser plusieurs méthodes :

* **Voler les clés RSA** (voir l'introduction de cet article). En volant cette clé, la NSA peut "lire" le contenu des flux SSL/TLS, mais aussi déchiffrer les flux qu'elle a capté AVANT.
 * contre-mesure : utiliser les courbes elliptiques et surtout le [Forward Secrecy](https://fr.wikipedia.org/wiki/Confidentialit%C3%A9_persistante) (FS, ou *[confidentialité persistante](https://fr.wikipedia.org/wiki/Confidentialit%C3%A9_persistante)*). 
* **avoir "accès" aux [chipsets matériel de chiffrement](http://blog.cryptographyengineering.com/2013/12/how-does-nsa-break-ssl.html)** 
 * contre-mesure : hum... changer de matos ? (et avoir de l'[open-hardware](https://news.ycombinator.com/item?id=6336505), utiliser un générateur externe quand c'est possible)
* **avoir "accès" aux chipsets logiciel de chiffrement** (comme Dual_EC_DRBG)
 * contre-mesure : utiliser un générateur externe.
* **Side channel attacks** : un système peut faire fuiter des informations (*operation time, resource consumption, cache timing, RF emissions*) qui peuvent parfois être exploitable en étant sur le même espace physique (i.e le même serveur physique, voir [Attack of the week: Cross-VM side-channel attacks](http://blog.cryptographyengineering.com/2012/10/attack-of-week-cross-vm-timing-attacks.html)). Matthew parle aussi d'une autre attaque, "*[remote timing information](http://crypto.stanford.edu/~dabo/abstracts/ssl-timing.html)*", en notant que [certains chipsets matériels désactivent la contre-mesure](http://cloudhsm-safenet-docs.s3.amazonaws.com/007-011136-002_lunasa_5-1_webhelp_rev-a/Content/configuration/partition_policies.htm) par défaut.
 * contre-mesure : faire attention à ses voisins, [RSA blinding](http://en.wikipedia.org/wiki/Blinding_(cryptography))

#### Bonnes pratiques :

* dans la mesure du possible, centraliser la configuration SSL/TLS pour un logiciel sur le serveur. Pour apache2, par exemple, vous pouvez mettre votre configuration (SSLCipherSuite...) dans <code>/etc/apache2/mods-available/ssl.conf</code>, cette conf sera alors pour l'ensemble de vos vhosts, et il faudra donc la changer uniquement là en cas de besoin (et non vhost par vhost, au risque d'en oublier)
* clé RSA égale ou supérieur à 2048 bits 
* utilisation des courbes elliptiques
* Forward Secrecy
* [SSLHonorCipherOrder on](https://httpd.apache.org/docs/2.2/mod/mod_ssl.html#sslhonorcipherorder)
* enlever les ciphers faibles et/ou obsolètes et/ou dangereux (DES, [RC4](https://twitter.com/ioerror/status/398059565947699200), ...)
* enlever SSLv3, et s'assurer par la même occasion que SSLv2 est bien mort et enterré (<code>SSLProtocol all -SSLv2 -SSLv3</code> pour Apache2 par exemple)
* compression désactivée, pour éviter l'attaque [CRIME](https://en.wikipedia.org/wiki/CRIME)

Pour vous aider dans la configuration de votre serveur web, vous pouvez utiliser le site [jeveuxhttps.fr](https://www.jeveuxhttps.fr/%C3%89tape_1_Installer_HTTPS_sur_le_serveur,_pour_informaticien), et bien entendu le désormais célèbre [ssllabs.com](https://www.ssllabs.com) qui permet de tester sa configuration. 

Des outils comme [sslscan](https://github.com/rbsec/sslscan), [xmpp.net](https://xmpp.net/) (XMPP/Jabber), [starttls.info](https://starttls.info/) (mails) peuvent aussi être utile.

### SSH

La seule trace du terme *backdoor* dans les documents à propos d'openSSH est en fait un rootkit, ce qui signifie qu'il FAUT avoir réussi à rentrer dans le serveur et à être root pour pouvoir modifier le binaire et permettre l'accès à une clé ou à un mot de passe, à noter que cette modification empêche de voir les connexions "pirates" de ces comptes dans les logs. 

Un problème concernant SSH pourrait être l'utilisation de ciphers obsolètes. Vous pouvez vérifier les ciphers proposés par votre serveur avec la commande <code>ssh -vvv</code> pour se connecter. Une connexion sur un serveur donne quelque chose ressemblant à ceci comme résultat ;

	ssh -vvv skhaen@exemple.com

	[...]
	kex_parse_kexinit: ssh-rsa,ssh-dss
	kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
	kex_parse_kexinit: hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-sha2-256,hmac-sha2-256-96,hmac-sha2-512,hmac-sha2-512-96,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
	[...]
	
Arcfour est en fait un autre nom de RC4, qui pour le coup est cassé depuis un moment. La version 6.7 d'OpenSSH le supprime, et [Microsoft recommande de le désactiver](http://blogs.technet.com/b/srd/archive/2013/11/12/security-advisory-2868725-recommendation-to-disable-rc4.aspx) depuis 2013. 

Pour améliorer la configuration de votre ssh (il n'y a pas que les ciphers), vous pouvez vous référer à ces trois articles. Faites TRÈS attention avant toutes modifications ! Ça peut vous empêcher de vous (re)connecter à votre serveur !

* [Secure Secure Shell](https://stribika.github.io/2015/01/04/secure-secure-shell.html) par [stribika](https://twitter.com/stribika) (hard way, nécessite d'avoir un système _récent_.)
* [NSA-proof SSH](http://kacper.blog.redpill-linpro.com/archives/702) par [whykacper/](https://twitter.com/whykacper) (soft way)
* [Bettercrypto.org](https://bettercrypto.org/static/applied-crypto-hardening.pdf)

### IPSec

Concernant IPSec et comment mieux le configurer, vous pouvez vous référer à l'article de Paul Wouters "*[Don’t stop using IPsec just yet](https://nohats.ca/wordpress/blog/2014/12/29/dont-stop-using-ipsec-just-yet/)*" (en espérant que le problème se trouve bien là). 

**TL;DR:**

* utiliser Perfect Forward Secrecy : <code>pfs=yes</code> 
* éviter les PreSharedKeys : <code>authby=secret</code>

## Je peux utiliser quoi alors ?

Il ne faut surtout pas tomber dans le piège "on est tous foutu, rien ne marche, on ne peut rien faire", ce n'est absolument pas le cas. Dans les bonnes nouvelles, nous avons aussi la preuve que [Tor](https://torproject.org/), [OTR](https://otr.cypherpunks.ca/), [GPG](https://www.gnupg.org/), [TAILS](http://tails.boom.org/) et [Redphone](https://play.google.com/store/apps/details?id=org.thoughtcrime.redphone&hl=fr_FR) sont sûr (du moins en 2012 ;-)).

![trival_catastrophic](/content/images/2015/09/trivial_catastrophic.png)

* Trivial : suivre le parcours d'un document sur Internet
* Mineur : enregistrer un chat privé sur Facebook
* Modéré : décrypter un email envoyé via le service de messagerie russe mail.ru
* Majeur : utiliser des services comme Zoho (?), Tor, TrueCrypt ou OTR
* Catastrophique : utiliser plusieurs protocoles en même temps, par exemple faire passer de la VoIP utilisant ZRTP par le réseau TOR, le tout à partir d'un ordinateur sous Linux.

On notera avec beaucoup d'intérêts qu'il n'y a aucune trace de logiciels commerciaux ([bitlocker](https://cryptoservices.github.io/fde/2014/12/08/code-execution-in-spite-of-bitlocker.html)...) dans les documents.

* **[Tor](https://torproject.org/)** est un logiciel libre qui, grâce à une technique de routage en oignon, d’anonymiser les connexions sur Internet.
* **[OTR](https://otr.cypherpunks.ca/)** permet de chiffrer des communications, en particulier sur XMPP/Jabber, ne pas hésiter à l'utiliser avec du [SSL/TLS](https://www.libwalk.so/2014/02/14/installer-un-serveur-xmppjabber-avec-prosody.html) par dessus.


![OTR_encrypted](/content/images/2015/09/OTR_encrypted1.png)

* **[GPG](https://www.gnupg.org/)** permet de chiffrer un mail, un fichier ou un dossier entier. Il permet aussi de signer un fichier (ou un mail) pour s'assurer de son authenticité.

![PGP encrypted](/content/images/2015/09/PGP_encrypted1.png)

* **[TAILS](http://tails.boom.org/)** est un système d'exploitation live, que vous pouvez démarrer, sur quasiment n'importe quel ordinateur, depuis un DVD, une clé USB, ou une carte SD. Son but est de préserver votre vie privée et votre anonymat, et de vous aider à utiliser Internet de manière anonyme et contourner la censure (toutes les connexions sortantes vers Internet sont obligées de passer par le réseau Tor).
* **[Redphone](https://play.google.com/store/apps/details?id=org.thoughtcrime.redphone&hl=fr_FR)** est une application pour Android qui permet d'avoir des conversations chiffrées (on peut aussi s'intéresser à **[Textsecure](https://play.google.com/store/apps/details?id=org.thoughtcrime.securesms&hl=fr_FR)**, application Android de [la même entreprise](https://whispersystems.org/) pour chiffrer les sms).

Dans un autre document, le logiciel **[Truecrypt](https://ciphershed.org/)** est aussi considéré comme "solide", il ne reste plus qu'à [attendre que son fork](https://ciphershed.org/) soit prêt.



Le point commun de tous ces logiciels ? Ce sont des logiciels libres.

Les documents sont disponibles sur le site du Spiegel

* [ NSA Documents: Attacks on VPN, SSL, TLS, SSH, Tor](http://www.spiegel.de/international/world/nsa-documents-attacks-on-vpn-ssl-tls-ssh-tor-a-1010525.html)
* [The Digital Arms Race: NSA Preps America for Future Battle](http://www.spiegel.de/international/world/new-snowden-docs-indicate-scope-of-nsa-preparations-for-cyber-battle-a-1013409-2.html)

