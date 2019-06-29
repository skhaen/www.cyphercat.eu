+++
author = "Skhaen"
categories = ["SIGINT", "leaks", "adminsys"]
date = 2014-09-22T15:55:00Z
description = ""
draft = false
slug = "tao-catalogue-ant"
tags = ["snowden"]
title = "[Snowden] NSA - TAO, la voie du renseignement"

+++

*Ne trouvant aucune base de données des différents programmes provenant des fuites de Snowden, [nous](https://github.com/nsa-observer) avons travaillé à la création d'une [base de données](https://www.nsa-observer.net) concernant la NSA et le GCHQ. L'article qui suit est une synthèse d'une partie de nos travaux.*

> It's not as bad as you thought - it's much worse<small>

> Iain Thomson - [theregister.co.uk / 2013-12-31](http://www.theregister.co.uk/2013/12/31/nsa_weapons_catalogue_promises_pwnage_at_the_speed_of_light/)</small>

Nous allons aborder aujourd'hui le "[catalogue](https://www.nsa-observer.net/tags/ant)" du groupe d'élite de la NSA nommé *TAO* (pour *[Tailored Access Operations](https://en.wikipedia.org/wiki/Tailored_Access_Operations)*). La mission de ce groupe est, depuis sa création en 1997, d'identifier, de monitorer et de [trouver un chemin](http://fr.wikipedia.org/wiki/Tao) vers des réseaux ou des systèmes pour collecter des informations. Ce département a gagné la réputation de collecter certains des meilleurs renseignements parmi les services de renseignements US, que ce soit sur la Chine, sur des groupes terroristes, ou sur les activités politiques, militaires et économiques d'un pays.

[Foreignpolicy.com](http://www.foreignpolicy.com/articles/2013/06/10/inside_the_nsa_s_ultra_secret_china_hacking_group?page=full) indique que plus de 1000 personnes, civiles et militaires, y travailleraient  à partir de 5 bases : ROC (pour *[Remote Operations Center](https://www.nsa-observer.net/category/compartment/family/collect/name/ROC)*), basé à [Fort Meade](https://en.wikipedia.org/wiki/Fort_Meade) (Maryland) qui est entre autres le quartier général de la NSA, [Hawaii](https://en.wikipedia.org/wiki/Kunia_Regional_SIGINT_Operations_Center) (Wahiawa, Oahu), [Fort Gordon](https://en.wikipedia.org/wiki/Fort_Gordon) (Georgie), au *[Texas Cryptologic Center](https://en.wikipedia.org/wiki/Texas_Cryptologic_Center)* et à la base de l'US Air Force [BKF](https://en.wikipedia.org/wiki/Buckley_Air_Force_Base) (Denver). TAO est divisé en quatre départements, chacun spécialisé dans un domaine précis, l'ensemble des équipes travaillant par roulement 24/7 :
* <u>Data Network Technologies</u> (DNT) qui s'occupe de la conception de malwares,
* <u>Telecommunications Network Technologies</u> (TNT) cible spécifiquement les méthodes de pénétrations dans les réseaux et les ordinateurs (bien entendu sans se faire détecter),
* <u>Mission Infrastructure Technologies</u> (MIT) développe et construit les outils de *monitoring*  hardware ainsi que les infrastructures qui permettent de tout supporter.
* <u>Access Technologies Operations</u> (ATO) travaille avec la CIA et le FBI. Ces deux derniers mettant en place des modules de surveillance permettant aux opérateurs de TAO de les utiliser à distance.

# TAO sort de l'ombre

Même si les premières informations publiques datent  de  [l'été 2013](http://www.foreignpolicy.com/articles/2013/06/10/inside_the_nsa_s_ultra_secret_china_hacking_group), cette branche de la NSA est réellement sortie de l'ombre en décembre 2013 avec la conférence "[To protect and infect, part 2: The militarization of the Internet](http://media.ccc.de/browse/congress/2013/30C3_-_5713_-_en_-_saal_2_-_201312301130_-_to_protect_and_infect_part_2_-_jacob.html)" de [Jacob Appelbaum](https://en.wikipedia.org/wiki/Jacob_Appelbaum) au [30c3](http://media.ccc.de/browse/congress/2013/) où il présente les différents programmes d'attaque et de collecte, en coopération avec le Spiegel (*[Inside TAO: Documents Reveal Top NSA Hacking Unit](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969.html)*).

<video class='video' controls='controls' poster='http://static.media.ccc.de/media/congress/2013/5713-h264-hq_preview.jpg' width='700'>
<source src='http://cdn.media.ccc.de/congress/2013/mp4/30c3-5713-en-de-To_Protect_And_Infect_Part_2_h264-hq.mp4' type='video/mp4'></source>
<source src='http://cdn.media.ccc.de/congress/2013/webm/30c3-5713-en-de-To_Protect_And_Infect_Part_2_webm.webm' type='video/webm'></source>
<object data='/assets/flashmediaelement.swf' height='360' type='application/x-shockwave-flash' width='640'>
<param name='allowFullScreen' value='true'></param>
<param name='flashvars' value='controls=true&amp;file=http://cdn.media.ccc.de/congress/2013/mp4/30c3-5713-en-de-To_Protect_And_Infect_Part_2_h264-hq.mp4'></param>
</object>
</video>

En plein milieu des "[révélations Snowden](http://cryptome.org/2013/11/snowden-tally.htm)", c'est un coup de tonnerre.

Les programmes peuvent se classer en différentes catégories : les implants matériels et les implants logiciels, dans lesquels on peut en mettre d'autres, comme ceux qui visent les téléphones (aussi bien GSM, smartphone que satellite), les malwares, et les programmes "RF" (pour radio-fréquences). Comme vous allez le voir, TAO fait plutôt de l'ultra-ciblé que de l'écoute de masse (au contraire du [GCHQ](https://www.libwalk.so/2014/07/04/gchq-jtrig-part1.html)). Ce qui concorde parfaitement avec leur mission : réussir à avoir les infos là où elles sont difficiles d'accès.

# Les programmes

Présenter un tel catalogue implique de sélectionner certains de ses éléments sans s'étendre sur son intégralité. Quasiment tous les programmes sont intéressants de par leur méthodes opérationnelles, cependant, dans la mesure du possible, nous parlerons seulement des programmes qui sortent de l'ordinaire. On ne parlera par exemple pas trop des variantes d'[IMSI catcher](http://en.wikipedia.org/wiki/IMSI-catcher) que l'on peut trouver [partout](https://twitter.com/pbump/status/417688999101083648). 

## Implants logiciels

[IRATEMONK](https://www.nsa-observer.net/IRATEMONK) permet de modifier les firmwares des disques durs de différentes marques (Western Digital, Seagate, Maxtor et Samsung) pour exécuter du code lors du boot.

## Téléphones

* [DROPOUTJEEP](https://www.nsa-observer.net/DROPOUTJEEP) - implant pour iphone, la NSA indique dans le catalogue que DROPOUTJEEP avait 100% de chances de succès. 
* [MONKEYCALENDAR](https://www.nsa-observer.net/MONKEYCALENDAR) - implant ciblant les cartes SIM (*Subscriber Identify Module*) et envoi par sms à un numéro défini les appels et la géolocalisation de l'utilisateur.
* [TOTEGHOSTLY](https://www.nsa-observer.net/TOTEGHOSTLY) - malware ciblant les téléphones Windows (mais quelle version ?), utilisant le framework CHIMNEYPOOL et contrôlé par STRAITBIZARRE. Il est utilisé pour mettre en place ou exfiltrer des fichiers, des sms, la liste des contacts ou encore trouver la géolocalisation via sms ou connection GPRS. L'attaquant a aussi le contrôle de la caméra et du micro.
* [TOTECHASER](https://www.nsa-observer.net/TOTECHASER) - malware ciblant le téléphone satellite/GSM [Thuraya 2520](http://www.thuraya.com/userfiles/files/Thuraya%20SG/Brochure/Thuraya%20SG_2520_Jun2010.pdf) basé sur [Windows CE](http://fr.wikipedia.org/wiki/Windows_CE). Il est utilisé pour exfiltrer les données de géolocalisation ainsi que les logs des appels et la liste de contacts via des sms cachés. Les sms servent aussi à l'attaquant à contrôler le téléphone. Selon les documents, l'implémentation de ce malware nécessite de modifier le téléphone lui même.

# FinFisher / Gamma International

Ce genre d'attaque est la spécialité de l'entreprise <a href="http://en.wikipedia.org/wiki/FinFisher">FinFisher</a> (Gamma International) qui est spécialisée dans les malwares "légaux" (lire "pour les forces de l'ordre, que vous soyez une dictature ou non"). Pour plus de détails sur les capacités de leurs malwares, lire "*<a href="https://netzpolitik.org/2014/gamma-finfisher-hacked-40-gb-of-internal-documents-and-source-code-of-government-malware-published/">Gamma FinFisher hacked: 40 GB of internal documents and source code of government malware published</a>*" (netzpolitik.org) ainsi que "*<a href="https://wikileaks.org/spyfiles4/index.html">SpyFiles 4</a>*" concernant les malwares eux-mêmes.
Il y n'a normalement aucun lien entre la NSA et FinFisher, mais les URLs vous permettront de mieux appréhender ce que l'on peut faire avec des malwares sur des téléphones.


## Serveurs et firewalls

* [SWAP](https://www.nsa-observer.net/SWAP) exploite le bios de la carte-mère et le *Host Protected Area* des disques durs pour obtenir des fenêtres d'exécution avant que le système ne charge. Cette technique fonctionne sur des systèmes Windows, Linux, FreeBSD, et Solaris,
* [JETPLOW](https://www.nsa-observer.net/JETPLOW) est un implant persistant (sous la forme d'un firmware) ciblant la série des **Cisco** PIX et des firewalls ASA (Adaptive Security Appliance) et permettant la mise en place d'une backdoor. À noter que [BANANAGLEE](https://www.nsa-observer.net/BANANAGLEE) fonctionne aussi sur ce type de matériel (et pas seulement sur du Juniper),
* [DEITYBOUNCE](https://www.nsa-observer.net/DEITYBOUNCE) - implant logiciel exploitant le bios de la carte mère des serveurs **Dell** PowerEdge,
* Les programmes se finissant par "MONTANA" ([SCHOOLMONTANA](https://www.nsa-observer.net/SCHOOLMONTANA) / [SIERRAMONTANA](https://www.nsa-observer.net/SIERRAMONTANA) / [STUCCOMONTANA](https://www.nsa-observer.net/STUCCOMONTANA)) visent les serveurs **Juniper** alors que les programmes finissant par TROUGH ([GOURMETTROUGH](https://www.nsa-observer.net/GOURMETTROUGH) / [SOUFFLETROUGH](https://www.nsa-observer.net/SOUFFLETROUGH) / [FEEDTHROUGH](https://www.nsa-observer.net/FEEDTHROUGH)) visent spécifiquement les firewalls du même constructeur,
* [IRONCHEF](https://www.nsa-observer.net/IRONCHEF) fournit un accès persistant sur les serveurs **HP** Proliant 380DL G5 en exploitant le bios de la carte-mère et le *System Management Mode* (SMM) via un implant matériel qui permet une communication via des ondes radio,
* Les programmes se finissant par "WATER" ([HALLUXWATER](https://www.nsa-observer.net/HALLUXWATER) / [HEADWATER](https://www.nsa-observer.net/HEADWATER)) visent les firewalls *Eudemon* du constructeur **Huawei** en installant une backdoor persistante via une "mise à jour" de la ROM du boot.

## Implants matériels

[ANGRYNEIGHBOR](https://www.nsa-observer.net/ANGRYNEIGHBOR) regroupe différents programmes d'écoutes fonctionnant à distance avec des ondes, que ce soit radar ou radio. Ils fonctionnent le plus souvent avec une balise et fonctionne même si l'ordinateur n'est pas branché à un réseau. Un générateur d'ondes (*[CW](https://www.nsa-observer.net/CW)*) illumine (comprendre "envoie des ondes vers") la balise, qui utilise alors ces ondes pour renvoyer ses informations. On parle alors d'ondes RF pour les fréquences radios et d'ondes radars pour le reste.

* [COTTONMOUTH](https://www.nsa-observer.net/COTTONMOUTH) est un implant USB (matériel) avec un lien RF qui permet l'infiltration ou l'exfiltration de données. Compatible avec [GENIE](https://www.nsa-observer.net/GENIE),
* [BULLDOZER](https://www.nsa-observer.net/BULLDOZER) et [GINSU](https://www.nsa-observer.net/GINSU) sont des malwares visant le bus PCI, installé via [INTERDICTION](https://www.nsa-observer.net/INTERDICTION), ils peuvent être connectés sur une carte wifi ou sur un routeur pour collecter des métadonnées et du contenu,
* [FIREWALK](https://www.nsa-observer.net/FIREWALK) est un implant pour l'ethernet (rj45/usb ?) permettant un pont RF
* [LOUDAUTO](https://www.nsa-observer.net/LOUDAUTO) est un rétro-réflecteur utilisant les ondes radar et du post-traitement basique pour récupérer l'audio d'une pièce.
* [SURLYSPAWN](https://www.nsa-observer.net/SURLYSPAWN) permet de collecter les frappes au clavier sans devoir avoir un logiciel installé sur le système cible, il faut qu'il soit seulement touché une fois.
* [PHOTOANGLO](https://www.nsa-observer.net/PHOTOANGLO) (comme le [CTX4000](https://www.nsa-observer.net/CTX4000)), est un générateur d'ondes en continu (CW), 
* [RAGEMASTER](https://www.nsa-observer.net/RAGEMASTER) fournit la "balise" qui sera illuminée par un générateur d'ondes. Il est installé dans un câble VGA standard et permet la récupération du signal vidéo. La description du catalogue rapporte que [RAGEMASTER](https://www.nsa-observer.net/RAGEMASTER) ne prend en compte que la "sortie" [rouge du VGA](http://fr.wikipedia.org/wiki/Connecteur_VGA) car c'est celui qui fournit le meilleur retour vidéo.
* [TAWDRYYARD](https://www.nsa-observer.net/TAWDRYYARD) sert de balise permettant de géolocaliser et d'identifier un périphérique implémentant RAGEMASTER.

En lisant le catalogue, on peut parfois voir qu'un programme est fait de composants "COTS" ("*[Commercial Off-The-Shelf](http://en.wikipedia.org/wiki/Commercial_off-the-shelf)*), ce qui signifie que l'on peut le trouver librement sur le marché et donc que l'on ne peut pas remonter à la NSA si quelqu'un le trouve. Si vous souhaitez plus de détails, vous pouvez voir les liens suivants : les [48 pages du catalogues](https://www.libwalk.so/files/TAO/), "*[Inside TAO: Documents Reveal Top NSA Hacking Unit](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969.html)*", "*[N.S.A. Devises Radio Pathway Into Computers](http://www.nytimes.com/2014/01/15/us/nsa-effort-pries-open-computers-not-connected-to-internet.html?smid=tw-nytimestech&seid=auto&_r=1)*" ou encore "*[Jacob Appelbaum: Futuristic-Sounding "Radar Wave Devices](http://www.democracynow.org/2013/12/31/jacob_appelbaum_futuristic_sounding_radar_wave)*".


# Et les drones ?

[NIGHTSTAND](https://www.nsa-observer.net/NIGHTSTAND), permet d'injecter des exploits sur une ou plusieurs cibles via  un réseau wifi. Il est typiquement utilisé lorsque la cible n'est pas accessible via un réseau câblé, et l'attaque est indétectable par l'utilisateur. Les documents indiquent que dans un environnement idéal, il est possible d'attaquer un réseau wifi à une distance de ~12 kilomètres. NIGHTSTAND est aussi [utilisable à partir d'un drone](http://www.dailydot.com/technology/nsa-wifi-hack-eight-miles/). 

Sur le même thème, vous pouvez aussi regarder les programmes suivants : [SPARROW-II](https://www.nsa-observer.net/SPARROW-II) (ordinateur embarqué sur du Linux, permet d'exploiter un réseau wifi à distance), [SHENANIGANS](https://www.nsa-observer.net/SHENANIGANS) (permet la captation massive de données, provenant aussi bien de routeurs wifi que de smartphones), [GILGAMESH](https://www.nsa-observer.net/GILGAMESH ) (localisation d'une carte SIM à partir d'un drone) et [VICTORYDANCE](https://nsa-observer.net/VICTORYDANCE) (coopération NSA/CIA : empreinte des réseaux wifi de quasiment toutes les grandes villes yéménites).
 
# Mais comment font-ils ?

Comment font-ils pour accéder physiquement au serveur s'ils doivent mettre en place des implants matériels ? Ou pour casser de la crypto ? Où encore pour accéder aux différentes réseaux ?

Il existe un programme, appelé [INTERDICTION](https://nsa-observer.net/INTERDICTION) qui consiste à intercepter un colis pendant son transfert pour y installer ce que vous voulez, le tout est renvoyé rapidement et discrètement (et avec le bon emballage bien entendu) à la cible. Le simple fait de commander du matériel en ligne est donc potentiellement un danger. La parade est plutôt simple, il suffit d'aller en magasin. Et comme conclut à juste titre [the register](http://www.theregister.co.uk/2013/12/31/nsa_weapons_catalogue_promises_pwnage_at_the_speed_of_light/?page=), il n'y a apparemment aucun "programme" qui possède une période de validité (à par [SEASONEDMOTH](https://nsa-observer.net/SEASONEDMOTH)). Ce qui veut dire une chose : méfiez-vous de qui vous achetez votre matériel d'occasion :]

Pour le chiffrement, c'est [BULLRUN](https://www.nsa-observer.net/BULLRUN) qui rentre en jeu : il vise à supprimer le chiffrement dans un environnement donné : CNE (*[Computer Network Exploitation](http://en.wikipedia.org/wiki/Computer_network_operations)*), [INTERDICTION](https://nsa-observer.net/INTERDICTION) (et donc pose d'implants), collecte de données (comme les [rapports d'erreurs de Windows](http://community.websense.com/blogs/securitylabs/archive/2013/12/29/dr-watson.aspx)) relation avec les entreprises : au hasard [RSA](http://www.cnet.com/news/security-firm-rsa-took-millions-from-nsa-report/) ou encore Microsoft (cf. [PRISM](https://nsa-observer.net/PRISM)) ,

![Skype_SSO](/images/2015/09/Skype_prism_sso.png)

ou avec d'autres services (par exemple le FBI pour Microsoft)

![microsoft_FBI](/images/2015/09/microsoft_FBI.png)

> À ce sujet, je m'interroge sur <a href="http://googleblog.blogspot.fr/2010/05/wifi-data-collection-update.html">Google</a> et les <a href="http://www.wired.com/2012/05/google-wifi-fcc-investigation/">Google cars</a> scannant les <a href"http://www.wired.com/2012/05/google-wifi-fcc-investigation/">réseaux WiFi</a> en 2010. L'information importante n'était pas que Google capte des données sur des réseaux ouverts, mais qu'il a "mappé" TOUS les <a href="http://en.wikipedia.org/wiki/Service_set_%28802.11_network%29#Service_set_identification_.28SSID.29">SSID</a> des réseaux wifi ainsi que leurs localisations : exactement la même chose que ce que la NSA a fait au Yémen avec <a href="https://www.nsa.-observer.net/VICTORYDANCE">VICTORY DANCE</a>. Le fait de savoir où se trouve un réseau wifi permet de géolocaliser une cible en ayant accès à son téléphone ou à son ordinateur sans même qu'il ne soit connecté à un réseau, son wifi doit juste être allumé. S'il est connecté, il devient alors possible de faire un lien entre une adresse IP et un SSID.

Continuons avec [BULLRUN](http://www.nytimes.com/interactive/2013/09/05/us/documents-reveal-nsa-campaign-against-encryption.html?_r=1&) : il ne faut pas oublier que la NSA a certains des meilleurs mathématicien-ne-s au monde qui travaillent sur de la cryptographie pour améliorer ou au contraire affaiblir des protocoles (comme [IPsec](http://en.wikipedia.org/wiki/IPsec#Alleged_NSA_interference) par exemple), mais aussi pour trouver des failles à exploiter avant les autres. Ainsi, en novembre 2013, [Jacob Appelbaum](http://en.wikipedia.org/wiki/Jacob_Appelbaum) (qui a eu accès à des documents classifiés d'une autre source que Snowden) disait que "*[RC4 is broken in real time by the NSA - stop using it](https://twitter.com/ioerror/status/398059565947699200).*" (**RC4 est cassé en temps réel par la NSA, arrêtez de l'utiliser**).

Il peut aussi y avoir des attaques plus simples , par exemple sur l'utilisation de [STARTTLS](http://en.wikipedia.org/wiki/STARTTLS) au lieu de [SSL/TLS](http://forums.mozillazine.org/viewtopic.php?f=39&t=2730845) ; son utilisation permet <u>dans certains cas</u> de forcer une connexion en clair au lieu d'avoir un chiffrement quelconque (d'où l'intérêt de mettre <code>ssl = required</code> et non <code>ssl = yes</code>).

# Oui et alors ?

La NSA peut alors utiliser des outils comme [XKEYSCORE](https://nsa-observer.net/XKEYSCORE) (dont j'avais parlé dans le [premier article](https://www.libwalk.so/2014/06/28/NSA-temps-de-faire-le-premier-point.html)) qui permet de rechercher en quasi temps réel dans [les différentes bases de données](https://www.nsa-observer.net/category/program/family/database) de la NSA et d'autres services  : ça peut aller de la nationalité, au sexe, la géolocalisation, l'IP, aux résultats de [DISCOROUTE](https://nsa-observer.net/DISCOROUTE) (écoute passive et planétaire du protocole [Telnet](http://en.wikipedia.org/wiki/Telnet)), en passant par les personnes qui [téléchargent le logiciel Tor](http://rt.com/news/170208-nsa-spies-tor-users/) jusqu'aux [sessions VPN](http://arstechnica.com/tech-policy/2013/08/nsas-internet-taps-can-find-systems-to-hack-track-vpns-and-word-docs/), [etc...](http://en.wikipedia.org/wiki/XKeyscore).

Une fois une cible définie (comme un administrateur système et/ou réseau, cf. [I Hunt sysadmin](https://www.nsa-observer.net/IHUNTSYSADMINS)), la NSA peut alors rediriger une cible vers un serveur [FOXACID](https://www.nsa-observer.net/category/program/family/target/name/FOXACID) (pour injecter du code dans son navigateur en utilisant des [0-day](http://fr.wikipedia.org/wiki/Vuln%C3%A9rabilit%C3%A9_jour_z%C3%A9ro)), la NSA réalise un [Man-in-the-Middle](http://en.wikipedia.org/wiki/Man-in-the-middle_attack) via des fausses pages web ([Linkedin, slashdot](http://arstechnica.com/tech-policy/2013/11/uk-spies-continue-quantum-insert-attack-via-linkedin-slashdot-pages/), ...) lors de la redirection en utilisant ses serveurs [QUANTUM](https://www.nsa-observer.net/QUANTUM) et du *[Deep Packet Inspection](http://en.wikipedia.org/wiki/Deep_packet_inspection)* ([TURBINE](https://www.nsa-observer.net/TURBINE) /[TURMOIL](https://www.nsa-observer.net/TURMOIL)).

Pour plus de détails sur cette méthode, vous pouvez lire les articles suivants :

* "*[How the NSA Plans to Infect ‘Millions’ of Computers with Malware](https://firstlook.org/theintercept/2014/03/12/nsa-plans-infect-millions-computers-malware/)*" par Ryan Gallagher et Glenn Greenwald
* "*[How the NSA Attacks Tor/Firefox Users With QUANTUM and FOXACID](https://www.schneier.com/blog/archives/2013/10/how_the_nsa_att.html)*" par [Bruce Schneier](https://www.schneier.com/about.html)
* "*[Our Government Has Weaponized the Internet. Here’s How They Did It](http://www.wired.com/2013/11/this-is-how-the-internet-backbone-has-been-turned-into-a-weapon/)*" par [Nicholas Weaver](http://www.wired.com/author/ncweaver/).

Pour la petite histoire, la société française [VUPEN a vendu des 0-day à la NSA](http://www.zdnet.com/nsa-purchased-zero-day-exploits-from-french-security-firm-vupen-7000020825/), et aussi à [FinFisher](https://netzpolitik.org/2014/gamma-finfisher-hacked-40-gb-of-internal-documents-and-source-code-of-government-malware-published/).

Certains documents rapportent [231 opérations en 2011](http://www.washingtonpost.com/world/national-security/us-spy-agencies-mounted-231-offensive-cyber-operations-in-2011-documents-show/2013/08/30/d090a6ae-119e-11e3-b4cb-fd7ce041d814_story.html), dont les 3/4 étaient en priorité contre l'Iran, la Russie, la Chine, la Corée du nord et la prolifération nucléaire. Voici quelques autres cibles :

* Le [*Secretariat of Public Security* mexicain](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969-2.html) qui est responsable de la police nationale, de l'anti-terrorisme, des prisons, et de la police aux frontières.
* L'entreprise allemande [Stellar PCS](https://firstlook.org/theintercept/2014/09/14/nsa-stellar/) de communications satellite (ainsi que Cetel et IABG)
* Le Fournisseur d'Accès à Internet belge [Belgacom](http://www.spiegel.de/international/world/ghcq-targets-engineers-with-fake-linkedin-pages-a-932821.html) en coopération avec le GCHQ.  
* Une tentative sur un routeur en Syrie qui a fait tombé l'accès à Internet de tout le pays : [The NSA, not Assad, took Syria off the Internet in 2012](http://arstechnica.com/tech-policy/2014/08/snowden-the-nsa-not-assad-took-syria-off-the-internet-in-2012/),
* [STUXNET](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969-2.html), en coopération avec les services Israëlien, est un ver qui visait les [systèmes SCADA des centrales nucléaires en Iran](http://en.wikipedia.org/wiki/Stuxnet).

Pour finir, [un document](http://www.washingtonpost.com/world/national-security/us-spy-agencies-mounted-231-offensive-cyber-operations-in-2011-documents-show/2013/08/30/d090a6ae-119e-11e3-b4cb-fd7ce041d814_story.html) indique que TAO est en train de travailler (en 2011) sur des implants pouvant identifier des "conversations intéressantes" dans un réseau ciblé et d'exfiltrer des morceaux.

# Sources

## TAO - [catalogue ANT](https://www.nsa-observer.net/tags/ant)

* les 48 pages du catalogues sont visibles sur [www.libwalk.so/files/TAO/](https://www.libwalk.so/files/TAO/) et plus facilement téléchargeables avec ce fichier : [NSA-TAO-ANT-48pages.tar.gz](https://www.libwalk.so/files/TAO/NSA-TAO-ANT-48pages.tar.gz),   
* Une vue rapide et bien faite du catalogue ANT sur [http://nsa.gov1.info](http://nsa.gov1.info/dni/nsa-ant-catalog/index.html)
* spiegel.de - [Inside TAO: Documents Reveal Top NSA Hacking Unit](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969.html)
 * page 2 -  [Targeting Mexico](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969-2.html)
 * page 3 -  [The NSA's Shadow Network](http://www.spiegel.de/international/world/the-nsa-uses-powerful-toolbox-in-effort-to-spy-on-global-networks-a-940969-3.html)
* spiegel.de - [NSA's Secret Toolbox: Unit Offers Spy Gadgets for Every Need](http://www.spiegel.de/international/world/nsa-secret-toolbox-ant-unit-offers-spy-gadgets-for-every-need-a-941006.html)
* spiegel.de - [Shopping for Spy Gear: Catalog Advertises NSA Toolbox](http://www.spiegel.de/international/world/catalog-reveals-nsa-has-back-doors-for-numerous-devices-a-940994.html) (slides)
* spiegel.de - [NSA-Dokumente: So knackt der Geheimdienst Internetkonten](http://www.spiegel.de/fotostrecke/nsa-dokumente-so-knackt-der-geheimdienst-internetkonten-fotostrecke-105326.html)
* spiegel.de - [vue interactive des programmes](http://www.spiegel.de/international/world/a-941262.html) (en anglais)
* washingtonpost.com - [U.S. spy agencies mounted 231 offensive cyber-operations in 2011, documents show](http://www.washingtonpost.com/world/national-security/us-spy-agencies-mounted-231-offensive-cyber-operations-in-2011-documents-show/2013/08/30/d090a6ae-119e-11e3-b4cb-fd7ce041d814_story.html) (GENIE)
* washingtonpost.com - [NSA uses Google cookies to pinpoint targets for hacking](http://www.washingtonpost.com/blogs/the-switch/wp/2013/12/10/nsa-uses-google-cookies-to-pinpoint-targets-for-hacking/)
* cryptome.org - [Jacob Appelbaum on Der Spiegel NSA/GCHQ Reports](http://cryptome.org/2014/01/appelbaum-der-spiegel.htm) on liberationtech.
* leaksource.info - [NSA’s ANT Division Catalog of Exploits](http://leaksource.info/2013/12/30/nsas-ant-division-catalog-of-exploits-for-nearly-every-major-software-hardware-firmware/)
* engadget.com - [NSA can hack WiFi devices from eight miles away](http://www.engadget.com/2013/12/30/nsa-can-hack-wifi-devices-from-eight-miles-away/)
* iclarified.com - [The NSA Has a Backdoor Called 'DROPOUTJEEP' for Nearly Complete Access to the Apple Iphone](http://www.iclarified.com/37195/the-nsa-has-a-backdoor-called-dropoutjeep-for-nearly-complete-access-to-the-apple-iphone)

> "**encryption works. Properly implemented strong crypto systems are one of the few things that you can rely on**".

> <small>Edward Snowden</small>

## Tag [ANT](https://www.nsa-observer.net/tags/ant) (NSA) sur nsa-observer.net

[BULLDOZER](https://www.nsa-observer.net/BULLDOZER) - [CANDYGRAM](https://www.nsa-observer.net/CANDYGRAM) - [COTTONMOUTH-I](https://www.nsa-observer.net/COTTONMOUTH-I) - [COTTONMOUTH-II](https://www.nsa-observer.net/COTTONMOUTH-II) - [COTTONMOUTH-III](https://www.nsa-observer.net/COTTONMOUTH-III) - [CROSSBEAM](https://www.nsa-observer.net/CROSSBEAM) - [CTX4000](https://www.nsa-observer.net/CTX4000) - [CYCLONE](https://www.nsa-observer.net/CYCLONE) - [DEITYBOUNCE](https://www.nsa-observer.net/DEITYBOUNCE) - [DROPOUTJEEP](https://www.nsa-observer.net/DROPOUTJEEP) - [ENTOURAGE](https://www.nsa-observer.net/ENTOURAGE) - [FEEDTROUGH](https://www.nsa-observer.net/FEEDTROUGH) - [FIREWALK](https://www.nsa-observer.net/FIREWALK) - [GENESIS](https://www.nsa-observer.net/GENESIS) - [GOPHERSET](https://www.nsa-observer.net/GOPHERSET) - [GOURMETTROUGH](https://www.nsa-observer.net/GOURMETTROUGH) - [HALLUXWATER](https://www.nsa-observer.net/HALLUXWATER) - [HEADWATER](https://www.nsa-observer.net/HEADWATER) - [HOWLERMONKEY](https://www.nsa-observer.net/HOWLERMONKEY) - [IRONCHEF](https://www.nsa-observer.net/IRONCHEF) - [JETPLOW](https://www.nsa-observer.net/JETPLOW) - [JUNIORMINT](https://www.nsa-observer.net/JUNIORMINT) - [LOUDAUTO](https://www.nsa-observer.net/LOUDAUTO) - [MAESTRO-II](https://www.nsa-observer.net/MAESTRO-II) - [MONKEYCALENDAR](https://www.nsa-observer.net/MONKEYCALENDAR) - [NEBULA](https://www.nsa-observer.net/NEBULA) - [NIGHTSTAND](https://www.nsa-observer.net/NIGHTSTAND) - [NIGHTWATCH](https://www.nsa-observer.net/NIGHTWATCH) - [PHOTOANGLO](https://www.nsa-observer.net/PHOTOANGLO) - [PICASSO](https://www.nsa-observer.net/PICASSO) - [RAGEMASTER](https://www.nsa-observer.net/RAGEMASTER) - [SCHOOLMONTANA](https://www.nsa-observer.net/SCHOOLMONTANA) - [SIERRAMONTANA](https://www.nsa-observer.net/SIERRAMONTANA) - [SOMBERKNAVE](https://www.nsa-observer.net/SOMBERKNAVE) - [SOUFFLETROUGH](https://www.nsa-observer.net/SOUFFLETROUGH) - [SPARROW-II](https://www.nsa-observer.net/SPARROW-II) - [STUCCOMONTANA](https://www.nsa-observer.net/STUCCOMONTANA) - [SURLYSPAWN](https://www.nsa-observer.net/SURLYSPAWN) - [SWAP](https://www.nsa-observer.net/SWAP) - [TAWDRYYARD](https://www.nsa-observer.net/TAWDRYYARD) - [TOTECHASER](https://www.nsa-observer.net/TOTECHASER) - [TOTEGHOSTLY](https://www.nsa-observer.net/TOTEGHOSTLY) - [TRINITY](https://www.nsa-observer.net/TRINITY) - [WATERWITCH](https://www.nsa-observer.net/WATERWITCH) - [WISTFULTOLL](https://www.nsa-observer.net/WISTFULTOLL)

