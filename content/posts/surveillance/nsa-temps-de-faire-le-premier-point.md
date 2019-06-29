+++
author = "Skhaen"
categories = ["SIGINT", "adminsys", "leaks"]
date = 2014-07-04T10:00:00Z
description = ""
draft = false
slug = "nsa-temps-de-faire-le-premier-point"
tags = ["SIGINT", "adminsys", "leaks"]
title = "[Snowden] NSA - temps de faire le (premier) point."

+++

> *Ne trouvant aucune base de données des différents programmes provenant des fuites de Snowden, [nous](https://github.com/nsa-observer) avons travaillé à la création d'une [base de données](https://www.nsa-observer.net) concernant la NSA et le GCHQ. L'article qui suit est une synthèse d'une partie de nos travaux.*

<hr />

![](/content/images/2015/09/nsaobserver_programs.png)

### (petite) Introduction

En 1988, le journaliste [Duncan Campbel](https://en.wikipedia.org/wiki/Duncan_Campbell_%28journalist%29) révèla l'existence d'un programme de renseignement, *[Echelon](https://fr.wikipedia.org/wiki/ECHELON)*, dans un article pour The New Statesman qui s'intitulait "*[Somebody's listening](http://www.duncancampbell.org/menu/journalism/newstatesman/newstatesman-1988/They%27ve%20got%20it%20taped.pdf)*". En 1996, c'était le journaliste néo-zélandais [Nicky Hager](http://www.nickyhager.info/) qui publiait "*[Secret power](http://www.nickyhager.info/ebook-of-secret-power/)*" sur l'implication de son pays dans le programme. En 1999, suite à une demande du Parlement Européen, Duncan Campbell rend un rapport intitulé "[*Interception capabilities 2000*](http://www.cyber-rights.org/interception/stoa/interception_capabilities_2000.htm])" pour le *[STOA](https://fr.wikipedia.org/wiki/STOA)* (*Science and Technology Options Assessment* du Parlement Européen -- traduction française disponible sous le titre "[*surveillance éléctronique planétaire*](http://www.editions-allia.com/fr/livre/438/surveillance-electronique-planetaire)" aux éditions Allia).

<!--more-->

Pour rappel, le [projet Echelon](https://fr.wikipedia.org/wiki/ECHELON) désignait un système mondial d'interception [SIGINT](https://fr.wikipedia.org/wiki/Renseignement_d%27origine_%C3%A9lectromagn%C3%A9tique] des communications privées et publiques) via les satellites, et regroupant les États-Unis (NSA), le Royaume-Uni (GCHQ), le Canada (CSTC), l’Australie (DSD) et la Nouvelle-Zélande (GCSB) dans le cadre du [traité UKUSA](https://fr.wikipedia.org/wiki/UKUSA).

Pour ne pas rester derrière, la France a rapidement mis en route un projet similaire, surnommé "*[frenchelon](http://bugbrother.blog.lemonde.fr/2009/06/16/frenchelon-la-carte-des-stations-espion-du-renseignement-francais/)*".

### Ce qui nous amène à...

Edward Snowden. Cet (ex-)analyste de la CIA et de la NSA décide d'alerter l'ensemble de la planète sur les capacités d'écoute et d'interception des États-unis et de ses alliés. Nous n'allons pas nous étendre là-dessus. Vous pouvez lire cette histoire un peu partout autour de vous.

Fin 2013, ne trouvant aucune base de données des différents programmes, [La Quadrature du Net](https://www.laquadrature.net/) commence grâce à quelques bénévoles le site [nsa-observer.net](https://www.nsa-observer.net) qui regroupe l'ensemble des programmes de la NSA et du GCHQ provenant des fuites de Snowden.



### et à un premier tour d'horizon

Même si l'immense majorité des programmes est sous l'égide de la NSA (*National Security Agency*) et donc des États-unis, certains programmes viennent d'autres pays (en particulier du Royaume-uni via le GCHQ). La NSA classe ses pays partenaires en 3 grandes familles :

* **Five Eyes** ([FVEY](http://en.wikipedia.org/wiki/UKUSA_Agreement#Five_Eyes) / AUSCANNZUKUS)
 * **États-unis** (indicatif pays : **USA** - agence : NSA),
 * **Royaumes-unis** (indicatif pays : **GBR** - agence : GCHQ),
 * **Nouvelle-Zélande** (indicatif pays : **NZL** - agence : GCSB),
 * **Canada** (indicatif pays : **CAN** - agence : CSEC),
 * **Australie** (indicatif pays : **AUS** - agence : ASD).


* **Nine Eyes** : consiste aux Five Eyes avec en plus : le Danemark, la France, les Pays-bas et la Norvége.

* **Fourteen Eyes** (SSEUR - Sigint Senior EURope) : consiste aux Nine Eyes avec en plus l'Allemagne, la Belgique, l'Italie, l'Espagne et la Suède.

## La collecte de données

Les services récupèrent tout ce qu'ils peuvent, c'est à dire dans le désordre :

Tout ce que vous faites avec un navigateur (sans oublier sa version, la langue, les plugins installés...), les flux voix (VoIP, GSM, téléphone fixe...), fax, emails, chats (MSN-like, Jabber...), vidéos (que ce soit en provenance de youtube, de skype,...), photos, fichiers (en transferts, stockés, ...), [DNI](http://en.wikipedia.org/wiki/Digital_Network_Intelligence) (_Digital Network Intelligence_), [DNR](http://en.wikipedia.org/wiki/Pen_register), empreintes des réseaux wifi (cf. [VICTORY DANCE](https://www.nsa-observer.net/category/mission/family/collect/name/VICTORYDANCE) en dessous), activités sur les réseaux sociaux, détails des comptes google/yahoo/outlook...), et j'en passe.

Pour faire simple, **tout** ce que vous faites laisse forcément des traces quelque part. Et c'est ce qu'ils cherchent, collectent, et parfois stockent. Voici donc la première partie de ce tour d'horizon concernant quelques programmes de la NSA et du GCHQ.

#### [UPSTREAM](https://www.nsa-observer.net/category/program/family/collect/name/UPSTREAM) / [TEMPORA](https://www.nsa-observer.net/category/program/family/collect/name/TEMPORA) / [RAMPART](https://www.nsa-observer.net/category/program/family/collect/name/RAMPART)-(x)

Aussi connu sous le nom de "*[ROOM641A](https://en.wikipedia.org/wiki/UPSTREAM)*", le programme UPSTREAM a commencé en 2003, a été révélé en 2006 et consiste à la collecte via des "écoutes" sur les câbles de fibres optiques (oui, je [simplifie énormément](https://en.wikipedia.org/wiki/Fiber_tapping)). Le projet UPSTREAM concerne la collecte sur l'ensemble des fibres optiques trans-océaniques, qui permettent l'acheminement des communications internationales, comme Internet, les appels, les sms...

**UPSTREAM** [englobe](http://www.washingtonpost.com/wp-srv/special/politics/prism-collection-documents/) beaucoup de sous-programmes qui correspondent à des régions du monde. Ils collectent différentes données (DNI, DNR, métadonnées, contenu, voix, fax...). Pour plus de détails sur UPSTREAM et ses sous-programmes, je vous conseille le blog [electrospaces](http://electrospaces.blogspot.fr/2013/12/nsas-global-interception-network.html).

**TEMPORA** est l'équivalent d'**UPSTREAM** pour le Royaume-uni, et **RAMPART** l'équivalent en coopération via certains pays Européens.

#### [PRISM](https://www.nsa-observer.net/category/program/family/process/name/PRISM)

PRISM est un accès direct aux serveurs de certaines compagnies américaines : Microsoft (+Skype), Yahoo, Google (+Youtube), Facebook, PalTalk, AOL, Apple...

Ce programme permet de faire des requêtes concernant des personnes ou des groupes précis concernant les emails, les chats/vidéos, les photos, les données stockées (coucou le cloud), de la VoiP, etc etc. Il suffit de voir les différents services de ses compagnies pour avoir une idée de qu'ils peuvent avoir. PRISM est donc utilisé pour "cibler" quelqu'un en particulier.

####  [MUSCULAR](https://www.nsa-observer.net/category/program/family/collect/name/MUSCULAR)

Ce programme a fait beaucoup parler de lui quand il a été rendu public, et pour cause : il permet l'interception des données entre les datacenters de Google (et de Yahoo) ce qui permet entre autre d'éviter les connexion HTTPS clients/serveurs. Google a chiffré ses communications inter-datacenters depuis.

#### [VICTORY DANCE](https://www.nsa-observer.net/category/mission/family/collect/name/VICTORYDANCE)

Coopération entre la CIA et la NSA au Yémen. Ils ont listé les empreintes wifi de presque toutes les grandes villes Yéménites via des drones (si vous ne voyez pas à quoi ça peut servir, cf. [XKEYSCORE](https://www.nsa-observer.net/category/program/family/process/name/XKEYSCORE)).

Je vous laisse vous rappeler ce que [les Google Cars ont fait pendant un bon moment dans le reste du monde](http://www.nytimes.com/interactive/2012/05/23/business/How-Google-Collected-Data-From-Wi-Fi-Networks.html?ref=technology&_r=0) et ce que ça a pu devenir depuis...

#### [MYSTIC](https://www.nsa-observer.net/category/program/family/collect/name/MYSTIC)

MYSTIC est un ensemble de programme qui collecte des données (le plus souvent provenant des téléphones et/ou des GSM) dans certains pays. On peut ainsi noter ACIDWASH (qui a collecté entre 30 et 40 millions de métadonnées par jour en Afghanistan), DUSKPALLET (qui vise les GSM au Kenya), EVENINGWEASEL (wifi mexicain) et [SOMALGET](https://www.nsa-observer.net/category/program/family/collect/name/SOMALGET).

[SOMALGET](https://www.nsa-observer.net/category/program/family/collect/name/SOMALGET) permet de "monitorer" les systèmes de télécommunications d'un pays (via des entreprises américaines implantées localement le plus souvent). Ce programme a visé les [Bahamas ](https://firstlook.org/theintercept/article/2014/05/19/data-pirates-caribbean-nsa-recording-every-cell-phone-call-bahamas/) (sans doute pour servir de test avant de passer à plus gros) puis  l'[Afghanistan](https://wikileaks.org/WikiLeaks-statement-on-the-mass.html) où il a collecté et stocké TOUS les appels téléphoniques, aussi bien localement que vers l'international.


## Mais que fait-on des données après ?

Voici justement quelques exemples.

#### [ANTICRISIS GIRL](https://www.nsa-observer.net/category/program/family/collect/name/ANTICRISISGIRL)

Via des programmes comme [UPSTREAM](https://www.nsa-observer.net/category/program/family/collect/name/UPSTREAM) / [TEMPORA](https://www.nsa-observer.net/category/program/family/collect/name/TEMPORA) / [RAMPART](https://www.nsa-observer.net/category/program/family/collect/name/RAMPART) qui permettent de faire de la collecte passive, il est possible de collecter les adresses IP des personnes se connectant à un site, [par exemple au site wikileaks.org ou au recherches Google ayant permis l'accès au site](https://firstlook.org/theintercept/article/2014/02/18/snowden-docs-reveal-covert-surveillance-and-pressure-tactics-aimed-at-wikileaks-and-its-supporters/).

#### [XKEYSCORE](https://www.nsa-observer.net/category/program/family/process/name/XKEYSCORE)

XKEYSCORE consiste en un ensemble d'interfaces et de bases de données qui permettent de sélectionner certaines données collectés via différents moyens (et il y en a beaucoup).

Voici quelques exemples des possibilités de ce programme :

* trouve moi tous les allemands (langue du navigateur) qui sont en Afghanistan (géolocalisation de l'IP) et qui ont été sur youporn et sur Facebook dans les dernières 24h.
* marque comme "extrémiste" toutes les personnes (connexion IP) allant sur le site du [projet Tor](https://www.torproject.org/) ou sur celui du projet [Tails](https://tails.boum.org/). Même chose si la source télécharge Tor.

Ne vous leurrez pas, ce dernier exemple est bel et bien réel, je vous laisse voir par vous même :

* [XKeyscore exposed: How NSA tracks all German Tor users as 'extremists'](http://rt.com/news/170208-nsa-spies-tor-users/)
* [The NSA Is Targeting Users of Privacy Services, Leaked Code Shows](http://www.wired.com/2014/07/nsa-targets-users-of-privacy-services/).

#### [OPTIC NERVE](https://www.nsa-observer.net/category/program/family/collect/name/OPTIC%20NERVE) (GCHQ)

En 2008, ce programme a collecté une photo toutes les 5 secondes provenant de chaque flux vidéos des [chat Yahoo](https://messenger.yahoo.com/) (soit environ 1.8 millions d'utilisateurs sur une période de 6 mois). Les documents indique qu'il y a un avertissement pour les opérateurs car l'on y trouve entre 8 et 12% de "nudité" (à prendre au sens large).

Pour information, en 2008 déjà, le GCHQ indique faire des tests pour de la reconnaissance faciale automatique, ainsi que pour la XBOX360. Nous sommes maintenant en 2014, je parie que c'est aussi le cas pour [Skype](http://pses2014.libwalk.so/#/9/2)) ainsi que pour Snapchat. N'oubliez pas que [la X BOX ONE](http://pses2014.libwalk.so/#/9/2) a une caméra et un micro allumé 24/7, avec de la reconnaissance faciale/vocale par défaut).

#### [La NSA espionne aussi certains sites pornos](http://www.bbc.com/news/technology-25118156)

Et oui ! Et il parait que c'est pour la lutte contre le terrorisme (comme le reste quoi...) : par exemple pouvoir faire pression sur un intégriste musulman parce qu'il a été "vu" en train de regarder du porno.


## Une cible ? Que peut-on faire ?


#### [QUANTUM](https://www.nsa-observer.net/category/attack%20vector/family/network/name/QUANTUM) (à lire en écoutant [Attack](http://www.youtube.com/watch?v=olILVp-J7Y8) !)

Pour rediriger une cible vers un serveur [FOXACID](https://www.nsa-observer.net/category/program/family/target/name/FOXACID), la NSA réalise un [Man-in-the-Middle](http://en.wikipedia.org/wiki/Man-in-the-middle_attack) (ou Man-on-the-Side) sur une connexion vers des serveurs de compagnies US (google (gmail), yahoo, linkedin..) grâce à l'emplacement de noeuds ([TAO](https://www.nsa-observer.net/category/compartment/family/collect/name/TAO) nodes) à des endroits clés sur le réseau (comprendre : "sur les dorsals de certains FAI"). La NSA utilise donc ses noeuds pour permettre des redirections à la volée vers les serveurs [FOXACID](https://www.nsa-observer.net/category/program/family/target/name/FOXACID), qui lanceront eux-mêmes différents types d'attaques selon la cible et les besoins.

Il existe beaucoup de sous-programmes pour QUANTUM, chacun vise quelque chose en particulier, comme [QUANTUMBOT](https://www.nsa-observer.net/category/attack%20vector/family/network/name/QUANTUMBOT) pour IRC, [QUANTUMINSERT](https://www.nsa-observer.net/category/attack%20vector/family/network/name/QUANTUM%20INSERT) (implantation de malware - utilisé dans le  _hack_ de Belgacom), [QUANTUMCOOKIE](https://www.nsa-observer.net/category/attack%20vector/family/network/name/QUANTUMCOOKIE) (force les cookies dans le navigateur ciblé), [QUANTUMSPIM](https://www.nsa-observer.net/category/program/family/collect/name/QUANTUMSPIM) (messagerie instantanée, comme MSN ou XMPP)...

#### [HUNT SYSADMINS](https://www.nsa-observer.net/category/mission/family/collect/name/IHUNTSYSADMINS) (à lire en écoutant "*[a bullet in your head](http://www.youtube.com/watch?v=o_-QGNUYL5g)*")

    ATTENTION : j'ai volontairement fait au plus rapide en "coupant" la partie technique, merci de suivre les liens pour approfondir ! C'est TRÈS intéressant !

Je dois vous avouer que j'ai réellement hurlé en lisant les posts sur un forum interne d'un opérateur de la NSA qui s'intitule "[hunt sysadmins](https://firstlook.org/theintercept/document/2014/03/20/hunt-sys-admins/)". Cet opérateur raconte que le meilleur moyen d'avoir accès à des informations est de "passer" par les administrateurs systèmes qui ont accès aux serveurs ou au routeurs (possibilité de mettre la main sur la topologie des réseaux, trouver des configurations, obtenir des accès, des emails...).

Il explique ainsi comment **ils collectent PASSIVEMENT toutes les transmissions via le protocole [telnet](http://en.wikipedia.org/wiki/Telnet)** (programme [DISCOROUTE](https://www.nsa-observer.net/category/program/family/collect/name/DISCOROUTE)). Oui, quand je parle de telnet je parle bien du procotole développé en 1968 (dans la [RFC 15](http://tools.ietf.org/html/rfc15), c'est dire que c'est vieux !), absolument non sécurisé (aucun chiffrement), et qui est apparemment encore pas mal utilisé pour administrer des routeurs à distance.

> Dude! Map all the networks!!!
> <small>(l'opérateur de la NSA en question)</small>

Il explique alors comment identifier un administrateur système, trouver [son webmail et/ou son compte Facebook](https://firstlook.org/theintercept/document/2014/03/20/hunt-sys-admins/) (oui oui) et de là utiliser la famille [QUANTUM](https://www.nsa-observer.net/category/attack%20vector/family/network/name/QUANTUM) pour réaliser une attaque et avoir un accès à sa machine ET/OU au(x) routeur(s).

![IHUNTSYSADMINS_shorter](/content/images/2015/09/IHUNTSYSADMINS_shorter.png)

##### Et là, comme tout le monde, tu te dis "Moi j'utilise SSH \o/"

Sauf qu'il explique AUSSI comment identifier une personne ayant accès à un serveur en SSH. **Shorter** : on peut deviner qui a *vraiment* accès à un serveur selon la taille des paquets qui passent et le temps de connexion.

![IHUNTSYSADMINS_shorter_ssh](/content/images/2015/09/IHUNTSYSADMINS_shorter_ssh.png)

Une fois la source (via l'IP) identifiée, vous pouvez voir (par exemple avec [XKEYSCORE](https://www.nsa-observer.net/category/program/family/process/name/XKEYSCORE)) les connexions à des webmails, Facebook (ou n'importe quoi qui peut l'identifier), et là, PAN ! Tu peux le [QUANTUM](https://www.nsa-observer.net/category/attack%20vector/family/network/name/QUANTUM)-ifier \o/

Et il termine gentiment sur le pourquoi et comment prendre la main sur un routeur (fort instructif aussi).

**ATTENTION** : encore une fois,  [TOUT le document est disponible en ligne](http://cryptome.org/2014/03/nsa-hunt-sysadmins.pdf), je vous conseille GRANDEMENT la lecture (et c'est obligatoire si vous êtes sysadmins ;-)).

#### [INTERDICTION](https://www.nsa-observer.net/category/attack%20vector/family/hardware/name/INTERDICTION)

C'est tout simplement la possibilité pour la NSA d'intercepter un colis (comme l'ordinateur que vous venez de commander en ligne), d'installer ce qu'ils veulent dessus (comme un firmware persistant dans le BIOS, un composant permettant à un appareil de la famille d'[ANGRYNEIGHBOR](https://www.nsa-observer.net/category/attack%20vector/family/hardware/name/ANGRYNEIGHBOR) (qui porte toujours aussi bien son nom) de fonctionner...).

Il suffit de jeter un coup d'oeil au [catalogue](http://leaksource.wordpress.com/2013/12/30/nsas-ant-division-catalog-of-exploits-for-nearly-every-major-software-hardware-firmware/) pour avoir une idée de ce qu'ils peuvent faire.

## Et pour finir

> "*le monde ne sera pas détruit par ceux qui font le mal, mais par ceux qui les regardent sans rien faire.*"
> <small>Albert Einstein</small>


Je pense qu'après ce tour d'horizon rapide (il y a plus de [400 programmes](http://www.cyphercat.eu/d3js/) sur [nsa-observer.net](https://www.nsa-observer.net)), tout le monde aura compris que cette voie est dangereuse. Il est impossible de prévoir qui décidera de son utilisation demain, ni même aujourd'hui.

**Nous ne pouvons pas faire confiance** aux gouvernements et encore moins aux entreprises pour assurer notre sécurité et notre vie privée.

**Nous pouvons, par contre, nous appuyer** sur la [société civile](https://fr.wikipedia.org/wiki/Soci%C3%A9t%C3%A9_civile) (comme l'[EFF](http://www.pcinpact.com/news/81255-prism-dix-neuf-associations-americaines-portent-plainte-contre-nsa.htm) ([eff.org](https://www.eff.org/)) ou [La Quadrature du Net](http://laquadrature.fr), les lanceurs d'alerte (comme [Chelsea Manning](https://en.wikipedia.org/wiki/Chelsea_Manning) ou [Edward Snowden](https://en.wikipedia.org/wiki/Edward_snowden)) et sur des [outils](https://prism-break.org/) qui ne nous trahiront pas, comme les [logiciels libres](https://www.april.org/).

**La cryptographie fonctionne** ! Et c'est une des nouvelles importantes de ces révélations. Il existe des tutoriaux partout sur le net pour se mettre à [chiffrer ses communications](http://www.cyphercat.eu/). Je vous laisse aller voir [OTR pour Jabber](https://help.riseup.net/en/security/message-security/otr) (messagerie instantanée), [SSL/TLS pour à peu près tout](http://www.iletaitunefoisinternet.fr/ssltls-benjamin-sonntag/) ([mails](http://www.iletaitunefoisinternet.fr/lemail-par-benjamin-sonntag/), [chat](http://www.libwalk.so/installer-un-serveur-xmppjabber-avec-prosody/),...), [GPG](http://www.cyphercat.eu/tuto_enigmail.php) (qui demande un niveau technique un peu supérieur), [Tor](http://torproject.org/), et surtout, surtout, je vous invite à [venir à des cryptoparty / café vie privée](http://www.cryptoparty.in/location#france) pour apprendre à s'en servir :)

