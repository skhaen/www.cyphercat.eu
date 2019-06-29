+++
author = "Skhaen"
categories = ["SIGINT", "leaks", "adminsys"]
date = 2014-07-07T16:37:00Z
description = ""
draft = false
slug = "gchq-partie-1-jtrig"
tags = ["SIGINT", "leaks", "adminsys"]
title = "[Snowden] GCHQ - partie 1 : JTRIG"

+++

> Ne trouvant aucune base de données des différents programmes provenant des fuites de Snowden, [nous](https://github.com/nsa-observer) avons travaillé à la création d'une [base de données](https://www.nsa-observer.net) concernant la NSA et le GCHQ. L'article qui suit est une synthèse d'une partie de nos travaux.

<hr />

> *Une dystopie, également appelée contre-utopie, est un récit de fiction dépeignant une société imaginaire organisée de telle façon qu'elle empêche ses membres d'atteindre le bonheur. Une dystopie peut également être considérée comme une utopie qui vire au cauchemar et conduit donc à une contre-utopie*.
> <small>Wikipedia - [Dystopie](https://fr.wikipedia.org/wiki/Dystopie)</small>

Nous allons parler dans cet article du [JTRIG](https://www.nsa-observer.net/tags/jtrig) (pour *[Joint Threat Research Intelligence Group](https://en.wikipedia.org/wiki/Joint_Threat_Research_Intelligence_Group)*), qui est l'équivalent pour le [GCHQ](https://fr.wikipedia.org/wiki/Government_Communications_Headquarters) (les services de renseignement anglais) du département [TAO](https://en.wikipedia.org/wiki/Tailored_Access_Operations) de la NSA. La mission du JTRIG consiste entre autre à détruire ou à empêcher d'agir les ennemis en les discréditant via de fausses informations ou en empêchant leurs communications de fonctionner : Déni de Service (DoS) via appels ou sms, suppression de la présence en ligne d'une cible, changement de photos sur les réseaux sociaux (pour discréditer une cible ou augmenter drastiquement sa parano), captation des mails (par exemple pour apporter de la crédibilité lors de l'infiltration d'un groupe)... la liste est longue.

![snowden-jtrig-slide](/content/images/2015/09/snowden-jtrig-slide.png)

<center>Le GCHQ possède un rayon d'action étendu, portant d'une cible individuelle à l'échelle d'un pays.</center>


#### Sommaire

* <a href="#gchqvsanonymous">GCHQ vs Anonymous</a>
* <a href="#ddos">Que font les terroristes déjà ? Des DDoS ?</a>
* <a href="#spam">Spam</a>
* <a href="#honeytraps">Honey traps</a>
* <a href="#reseauxsociaux">Réseaux sociaux</a>
* <a href="#bonus">Vous en voulez encore ?</a>
* <a href="#conclusion">Pour finir</a>
* <a href="#liens">Quelques liens en plus</a>


###<a id="gchqvsanonymous"></a>GCHQ vs Anonymous

Dans un article du 5 Février 2014, Mark Schone (NBC News) racontait comment le JTRIG lançait des [DDoS contre les anonymous](http://www.nbcnews.com/feature/edward-snowden-interview/exclusive-snowden-docs-show-uk-spies-attacked-anonymous-hackers-n21361) avec le programme [ROLLING THUNDER](https://www.nsa-observer.net/ROLLINGTHUNDER) (DDoS via P2P/*[syn flood](https://en.wikipedia.org/wiki/SYN_flood)* selon le journal).

Le [GCHQ](https://fr.wikipedia.org/wiki/Government_Communications_Headquarters) était aussi présent sur des chans IRC [Anons](https://en.wikipedia.org/wiki/Anonymous_%28group%29), ce qui lui a permis d'arrêter quelques personnes : [Edward Pearson](http://www.dailymail.co.uk/news/article-2124114/Computer-hacker-Edward-Pearson-Lendale-York-stole-million-people-s-personal-details-jailed-half-years.html) a.k.a [GZero](http://garwarner.blogspot.fr/2012/04/uk-zeus-user-g-zero-sentenced.html). [Jake "Topiary" Davis](https://en.wikipedia.org/wiki/Topiary_%28hacktivist%29) (porte-parole du groupe [Lulzsec](https://en.wikipedia.org/wiki/LulzSec)), [Mustafa “*Tflow*" Al-Bassam](https://en.wikipedia.org/wiki/Mustafa_Al-Bassam), aussi du groupe Lulzsec, ainsi que quelques autres.

Quelques infos supplémentaires sur les condamnations :

* [Edward "*GZero*" Pearson](http://www.nbcnews.com/id/46955000/ns/technology_and_science-security/t/uk-hacker-sentenced-stealing-million-identities/) : condamné à l'âge de 23 ans à 26 mois pour le vol de "millions d'identitées et d'informations sur ~200 000 comptes Paypal", ils ont (sa copine et lui) payé des chambres d'hôtels et des factures téléphonique avec des CB volées.
* [Jake "Topiary" Davis](https://en.wikipedia.org/wiki/Topiary_%28hacktivist%29) (Lulzsec) : condamné à l'âge de 18 ans à 24 mois dans un centre de détention pour jeune (il est apparemment sorti au bout de 5 mois)
* [Mustafa “*Tflow*" Al-Bassam](https://en.wikipedia.org/wiki/Mustafa_Al-Bassam) (Lulzsec) : condamné à l'âge de 16 ans à 500 heures de travail d'intérêt général et interdiction de se connecter à Internet pendant 2 ans
* [Ryan "Kayla" Ackroyd](https://en.wikipedia.org/wiki/Ryan_Ackroyd) (Lulzsec) : condamné à 30 mois à l'âge de 26 ans.
* [Ryan Cleary](http://www.telegraph.co.uk/technology/news/9354188/LulzSecs-Ryan-Cleary-admits-hacking-into-CIA-and-the-Pentagon.html) (Lulzsec) : condamné à 32 mois à l'âge de 21 ans. Il a aussi plaidé coupable pour la détention d'images à caractères pédopornographiques

Lire aussi "*[LulzSec hackers handed down prison terms, suspended sentence in Britian](http://rt.com/news/lulzsec-sentence-jail-davis-376/)*" à propos de Ryan Ackroyd, Jake Davis, Mustafa Bassam et Ryan Cleary, ainsi que l'article [Wikipedia](https://en.wikipedia.org/wiki/LulzSec#Former_members_and_associates) à propos du reste de l'équipe.



* [GZero](http://www.nbcnews.com/id/46955000/ns/technology_and_science-security/t/uk-hacker-sentenced-stealing-million-identities/) a sans aucun doute cherché les emmerdes : vol de comptes paypal, utilisation de CB volées, discussion en ligne avec un agent se faisant passer pour un Anon à propos d'attaques..., le blog [garwarner.blogspot.fr](http://garwarner.blogspot.fr/2012/04/uk-zeus-user-g-zero-sentenced.html) raconte des choses très intéressantes : son compte SoundCloud aurait eu comme *Userid* GZero et comme nom "*Edward Pearson*". Selon [la slide 7 de ce document](http://msnbcmedia.msn.com/i/msnbc/sections/news/snowden_anonymous_nbc_document.pdf), un *whois* sur un de ses liens échangés sur IRC aurait rapporté des informations... même pas sûr que le GCHQ ait eu à utiliser le programme [PHOTON TORPEDO](https://www.nsa-observer.net/category/program/family/collect/name/PHOTONTORPEDO) qui permet de récupérer l'adresse IP d'un utilisateur de MSN : oui... il avait aussi son [adresse en clair sur un site, lié à son pseudo](http://garwarner.blogspot.fr/2012/04/uk-zeus-user-g-zero-sentenced.html)...
 
Un autre anon, *[p0ke](http://www.wired.co.uk/news/archive/2014-02/05/gchq-ddos-attack-anonymous)*, a cliqué sur un lien menant vers l'article de la BBC  “*[Who loves the hacktivists](http://www.bbc.com/news/technology-13872755)*" qu'un agent lui a envoyé. Je pense que vous devinez ce qui s'est passé, le GCHQ a été capable de récupérer l'adresse IP qu'il avait derrière son VPN. 

Ça marche avec des liens (rappelez vous de [QUANTUM](https://www.nsa-observer.net/QUANTUM) et de [FOXACID](https://www.nsa-observer.net/FOXACID)), mais ça marche aussi très bien avec des pièces jointes, comme avec un document Office : [TRACER FIRE](https://www.nsa-observer.net/TRACERFIRE) permet de récupérer des infos sur la machine ciblée, comme des fichiers ou des logs), [TORNADO ALLEY](https://www.nsa-observer.net/TORNADOALLEY) fait plus ou moins la même chose sous la forme d'un document excel. Ou encore [GURKHAS SWORD](https://www.nsa-observer.net/GURKHASSWORD) qui permet de faire remonter l'adresse IP de la cible. Le lien sur lequel il a cliqué était sans doute piégé, mais il existe une autre hypothèse : une surveillance passive des connexions à l'article via des programmes comme [TEMPORA](https://www.nsa-observer.net/TEMPORA) et [ANTICRISIS GIRL](https://www.nsa-observer.net/category/program/family/collect/name/ANTICRISISGIRL) et qui aurait pu permettre par ricochet de remonter à sa navigation, comme son compte Facebook, et ses adresses mails.

> Pour la petite histoire, les [Gurkhas](https://en.wikipedia.org/wiki/Gurkha) sont des soldats/mercenaires Népalais que l'on trouve dans l'[armée Britannique](http://www.bbc.com/news/uk-10782099) depuis ~200 ans, ils sont entre autre réputés pour leurs poignards, les [Khukuri](https://fr.wikipedia.org/wiki/Khukuri), ou kukri).

Ça aurait aussi pu venir de l'utilisation d'un programme comme, [GLASS BACK](https://www.nsa-observer.net/GLASSBACK) (permet de récupérer l'adresse IP d'une cible en la spammant), ou tout simplement une requête en bonne et due forme au fournisseur du VPN en question (on en oublierait presque les bonnes vieilles méthodes).

###<a id="ddos"></a>Que font les terroristes déjà ? Des DDoS ?

Il est toujours agréable d'apprendre que le GCHQ sait très, très bien utiliser le déni de service sous toutes ses formes : DoS sur des serveurs web ([PREDATORS FACE](https://www.nsa-observer.net/PREDATORSFACE)), sur les téléphone via [call bombing](https://en.wikipedia.org/wiki/Mobile_phone_spam) ([SCARLET EMPEROR](https://www.nsa-observer.net/SCARLETEMPEROR)), contre SSH ([SILENT MOVIE](https://www.nsa-observer.net/SILENTMOVIE)) ou encore de manière silencieuse sur les téléphone satellites / GSM avec [VIPERS TONGUE](https://www.nsa-observer.net/VIPERSTONGUE) (par silencieux, on entend que la cible ne voit rien du DoS ; les messages n'arrivent pas réellement sur le périphérique mais bloquent les communications).

[AMBASSADORS RECEPTION](https://www.nsa-observer.net/AMBASSADORSRECEPTION) a été utilisé dans différentes situations : quand il est utilisé sur une machine cible, il se chiffre lui-même, efface tous les emails, chiffre tous les fichiers, fait trembler l'écran et empêche l'utilisateur de s'identifier. [STEALTH MOOSE](https://www.nsa-observer.net/STEALTH MOOSE) cible spécifiquement les machines Windows.

Une autre opération [visant spécifiquement les Talibans en Afghanistan](http://www.nbcnews.com/feature/edward-snowden-interview/exclusive-snowden-docs-show-british-spies-used-sex-dirty-tricks-n23091
) a consisté en une tempête de fax, d'appels et de SMS programmée pour arriver toutes les minutes sur les terminaux cibles.

[CANNONBALL](https://www.nsa-observer.net/CANNONBALL) permet de spammer via SMS une cible, [BURLESQUE](https://www.nsa-observer.net/BURLESQUE) permet d'envoyer des SMS modifiés et [CONCRETE DONKEY](https://www.nsa-observer.net/CONCRETEDONKEY) permet l'envoi d'un message audio à de nombreux téléphones ou de "spammer" une cible avec le même message (on notera la référence à [Worms Armageddon](http://worms.wikia.com/wiki/Concrete_Donkey) <3).

###<a id="spam"></a>Spam

Et le GCHQ n'hésite pas à envoyer via les réseaux sociaux (Facebook, Twitter, Skype) le message "*[DDOS and hacking is illegal, please cease and desist](http://www.wired.co.uk/news/archive/2014-02/05/gchq-ddos-attack-anonymous)*" pour "dissuader" des activistes de participer à des DDoS. Une des slides indique que 80% des personnes à qui avait été envoyé le message n'étaient plus sur les chans IRC un mois après.

On parle ici de programme comme [BADGER](https://www.nsa-observer.net/BADGER) ou [WARPATH](https://www.nsa-observer.net/WARPATH) qui permettent d'envoyer massivement (qui a dit spammer ?) du mail.

> Pour information, [MINIATURE HERO](https://www.nsa-observer.net/MINIATUREHERO)  vise spécifiquement le logiciel [Skype](https://fr.wikipedia.org/wiki/Skype) et permet de collecter et d'enregistrer des conversations (aussi bien skypeOut que en skype to skype), les messageries instantanées et les listes de contacts

###<a id="honeytraps"></a> Honey traps

[ROYAL CONCIERGE](https://www.nsa-observer.net/ROYALCONCIERGE) exploite les réservations d'hôtels pour tracker les diplomates étrangers (et sans doute pas que les diplomates). Le GCHQ utilise ce programme pour essayer de mener les cibles vers des hôtels "*SIGINT friendly*" : plus facilement espionnables, aussi bien électroniquement que humainement.

La mise en place de honey traps fait aussi partie du catalogue du JTRIG, et il arrive qu'une opération soit mise en place pour leurrer une cible en lui faisant miroiter une possibilité de relation amoureuse et/ou sexuelle et ainsi l'amener à faire quelque chose (le BA.B.A des services de renseignement quoi...). C'est typiquement ce qui s'est passé en 1986 avec [Mordechai Vanunu](https://en.wikipedia.org/wiki/Mordechai_Vanunu) et l'agente du [Mossad](https://en.wikipedia.org/wiki/Mossad) (le service de renseignement israéliens) [Cheryl Bentov](https://en.wikipedia.org/wiki/Cheryl_Bentov).


> Le cas Julian Assange

> [Julian Assange](https://fr.wikipedia.org/wiki/Julian_Assange), fondateur de Wikileaks est accusé de viol(s) (réellement de "[sex by surprise](http://gizmodo.com/5705614/wikileaks-julian-assange-is-not-accused-of-rape-updated)") le 21 août 2010 par la justice suédoise qui lève l'avis de recherche quelques heures plus tard.

> * Le 18 novembre 2010, le parquet suédois relance un mandat d'arrêt contre lui.
* Le 1er décembre 2010, La Suède autorise Interpol à rendre publique la [notice rouge](http://www.interpol.int/fr/Centre-des-m%C3%A9dias/Nouvelles/2010/PR101) le concernant.
* Il est arrêté le 7 décembre 2010 par la police britannique, puis est mis en liberté surveillée (avec bracelet électronique) une semaine après avec une caution de 282 000 €.
* Depuis le 19 juin 2012 il est réfugié à l'ambassade d'Équateur où il a reçu l'asile politique.
* Le 16 août 2012, Interpol indique que [la notice rouge le concernant est toujours en vigueur](http://www.interpol.int/fr/Centre-des-m%C3%A9dias/Nouvelles/2012/PR065).

> La Suède a refusé de garantir qu'il ne serait pas extradé vers les USA s'il était présent sur le sol suédois pour répondre de ses actes. Elle a aussi refusé d'interroger Assange à l'ambassade ou à distance.

> Pour plus de détails sur cette affaire, je vous conseille la lecture de l' article suivant :

> > The Guardian : [10 days in Sweden: the full allegations against Julian Assange](http://www.theguardian.com/media/2010/dec/17/julian-assange-sweden) (Miss A et Miss W)
> > *Assange's defence team had so far been provided by prosecutors with only incomplete evidence, he said. "There are many more text and SMS messages from and to the complainants which have been shown by the assistant prosecutor to the Swedish defence lawyer, Bjorn Hurtig, which suggest motivations of malice and money in going to the police and to Espressen and raise the issue of political motivation behind the presentation of these complaints. He [Hurtig] has been precluded from making notes or copying them.*
> > "We understand that both complainants admit to having initiated consensual sexual relations with Mr Assange. They do not complain of any physical injury. The first complainant did not make a complaint for six days (in which she hosted the respondent in her flat [actually her bed] and spoke in the warmest terms about him to her friends) until she discovered he had spent the night with the other complainant.

> > "*The second complainant, too, failed to complain for several days until she found out about the first complainant: she claimed that after several acts of consensual sexual intercourse, she fell half asleep and thinks that he ejaculated without using a condom – a possibility about which she says they joked afterwards.*

> > "*Both complainants say they did not report him to the police for prosecution but only to require him to have an STD test. However, his Swedish lawyer has been shown evidence of their text messages which indicate that they were concerned to obtain money by going to a tabloid newspaper and were motivated by other matters including a desire for revenge.*"

> Si seulement la Justice était toujours aussi prompte à agir contre les violeurs...


###<a id="reseauxsociaux"></a>Réseaux sociaux

> *La propagande est à la Démocratie ce que la violence est aux dictatures.*
<small>[Noam Chomsky](https://fr.wikipedia.org/wiki/Noam_Chomsky)</small>

Et ça le GCHQ l'a trop bien compris, ils peuvent accroître artificiellement le trafic d'un site ([GATEWAY](https://www.nsa-observer.net/GATEWAY)), amplifier un message (= augmenter le nombre de vues et donc le référencement), comme une vidéo sur Youtube ([GESTATOR](https://www.nsa-observer.net/GESTATOR)), ou encore **modifier un mur facebook aussi bien pour une seule personne que pour un pays entier** ([CLEANSWEEP](https://www.nsa-observer.net/CLEANSWEEP)).

Ils ont aussi bien la possibilité de modifier les photos d'utilisateurs sur les réseaux sociaux que d'envoyer des mails ou des sms à vos collègues ou à vos voisins à votre place, il est indiqué dans les leaks que ce type de méthode a aidé la police Britannique à "arrêter des criminels".

Donc, maintenant, si (par exemple) vous trompez votre conjoint-e et arrive le moment où vous vous trompez de destinataire lors de l'envoi d'un message, vous avez maintenant l'excuse "c'est une opération des services de renseignement anglais pour me discréditer !". Pas mal non ?

###<a id="bonus"></a> Quelques programmes bonus

![](/content/images/2015/09/jtrig_d3js.png)

* [BABYLON](https://www.nsa-observer.net/BABYLON) permet de faire une requête sur une adresse mail Yahoo ou Hotmail (à la date du document) pour savoir quand elle est utilisée ou non ce qui permet de savoir quand s'y connecter.
* [DANCING BEAR](https://www.nsa-observer.net/DANCINGBEAR) permet d'obtenir la localisation d'un point d'accès wifi.
* [HACIENDA](https://www.nsa-observer.net/HACIENDA) permet de scanner une ville ou un pays entier
* [SWAMP DONKEY](https://www.nsa-observer.net/SWAMPDONKEY)  localise des types de fichiers prédéfinis et les chiffre.
* [DEEPSTALKER](https://www.nsa-observer.net/DEEPSTALKER) aide à la géolocalisation des téléphones satellites et des GSM en faisant un "appel silencieux" (invisible pour la cible).
* [SQUEAKY DOLPHIN](https://www.nsa-observer.net/SQUEAKYDOLPHIN) permet une supervision en temps-réel des vues sur Youtube, des Likes, sur Facebook ou encore des visites sur Blogspot/Blogger. Ce qui permet de récupérer des informations pertinentes sur des [tendances](http://msnbcmedia.msn.com/i/msnbc/Sections/NEWS/snowden_youtube_nbc_document.pdf) (slide 29 à 32), et donc potentiellement "prévoir" des événements (l'exemple dans les slides concerne les tags "14FEB", "Bahrain" et "March rally" le 13 février 2012, c'est à dire la veille du premier anniversaire du "[day of rage](https://en.wikipedia.org/wiki/Day_of_Rage_%28Bahrain%29)" au Bahrein).

##<a id="conclusion"></a>Pour finir

<blockquote>"*Nul ne sera l'objet d'immixtions arbitraires dans sa vie privée, sa famille, son domicile ou sa correspondance, ni d'atteintes à son honneur et à sa réputation. Toute personne a droit à la protection de la loi contre de telles immixtions ou de telles atteintes.*" <small>[Article 12](http://www.un.org/fr/documents/udhr/#a12) de la déclaration universelle des droits de l'homme</small></blockquote>

Que pouvons nous faire ? La réponse est simple à écrire, mais plus difficile à mettre en oeuvre, il faut commencer par [faire son modèle de menace](https://en.wikipedia.org/wiki/Threat_model), après ça, vous avez le choix : chiffrer, tout, tout le temps. Utiliser [Tor](http://torproject.org/), dont les [hidden services](https://www.torproject.org/docs/hidden-services.html.en) le plus souvent possible. Mettre en place des [machines virtuelles](https://fr.wikipedia.org/wiki/Machine_virtuelle) dédiées (par exemple KVM ou virtualbox --adminsys : SSH over Tor seulement par exemple, Navigation web), utiliser des logiciels libre (vous méritez de vous faire prendre sinon ;-)), SSL/TLS par défaut pour TOUT, OTR sur les messageries, ouvrir les liens que quelqu'un vous donne sur une machine distante (sans lien avec vous) ou sur une VM... etc etc. 

Et évidemment, ne pas faire de conneries : toute la crypto du monde ne vous protégera jamais si vous faites des erreurs bêtes, comme permettre des passerelles entre votre pseudo et votre nom (comme une adresse email). Le fondateur de [SilkRoard](https://en.wikipedia.org/wiki/Silk_Road_%28marketplace%29), [William Ulbricht](http://motherboard.vice.com/blog/everything-the-silk-road-founder-did-to-get-caught) (aka Dread Pirate Robert's) peut en témoigner.

##<a id="liens"></a>Quelques liens

#### Documents :

* nbcnews.com - [Psychology - A new kind of SIGDEV](http://msnbcmedia.msn.com/i/msnbc/Sections/NEWS/snowden_youtube_nbc_document.pdf) (pdf - SQUEAKY DOLPHINS, OCEAN)
* nbcnews.com - [Hacktivism - Online covert action](http://msnbcmedia.msn.com/i/msnbc/sections/news/snowden_anonymous_nbc_document.pdf) (pdf - Gzeo et p0ke)
* nbcnews.com - [Full-Spectrum cyber effects](http://msnbcmedia.msn.com/i/msnbc/sections/news/snowden_cyber_offensive2_nbc_document.pdf) (pdf)
* nbcnews.com - [SIGDEV conference 2012 | The art of the possible](http://msnbcmedia.msn.com/i/msnbc/sections/news/snowden_cyber_offensive1_nbc_document.pdf) (pdf)
* theintercept.coom - [The art of Deception](https://firstlook.org/theintercept/document/2014/02/24/art-deception-training-new-generation-online-covert-operations/) (pdf en ligne, 50 pages)

#### Articles :

* nbcnews.com - [Snowden Docs Show British Spies Used Sex and 'Dirty Tricks'](http://www.nbcnews.com/feature/edward-snowden-interview/exclusive-snowden-docs-show-british-spies-used-sex-dirty-tricks-n23091)
* nbcnews.com - [Snowden Docs Show UK Spies Attacked Anonymous, Hackers](http://www.nbcnews.com/feature/edward-snowden-interview/exclusive-snowden-docs-show-uk-spies-attacked-anonymous-hackers-n21361)
* nbcnews.com - [Snowden docs reveal British spies snooped on YouTube and Facebook](http://investigations.nbcnews.com/_news/2014/01/27/22469304-snowden-docs-reveal-british-spies-snooped-on-youtube-and-facebook)
* msnbcmedia.msn.com - [snowden_cyber_offensive1_nbc_document.pdf](http://msnbcmedia.msn.com/i/msnbc/sections/news/snowden_cyber_offensive1_nbc_document.pdf)
* msnbcmedia.msn.com - [snowden_cyber_offensive2_nbc_document.pdf](http://msnbcmedia.msn.com/i/msnbc/sections/news/snowden_cyber_offensive2_nbc_document.pdf)
* theintercept.com - [The Art of Deception: Training for a New Generation of Online Covert Operations](https://firstlook.org/theintercept/document/2014/02/24/art-deception-training-new-generation-online-covert-operations/)

####  Tag [JTRIG](https://www.nsa-observer.net/tags/jtrig) (GCHQ) sur nsa-observer.net

[AIRBAG](https://www.nsa-observer.net/AIRBAG) - [AIRWOLF](https://www.nsa-observer.net/AIRWOLF) - [ALLIUMARCH](https://www.nsa-observer.net/ALLIUMARCH) - [ANCESTRY](https://www.nsa-observer.net/ANCESTRY) - [ANGRYPIRATE](https://www.nsa-observer.net/ANGRYPIRATE) - [ARSONSAM](https://www.nsa-observer.net/ARSONSAM) - [ASTRALPROJECTION](https://www.nsa-observer.net/ASTRALPROJECTION) - [AXLEGREASE](https://www.nsa-observer.net/AXLEGREASE) - [BABYLON](https://www.nsa-observer.net/BABYLON) - [BADGER](https://www.nsa-observer.net/BADGER) - [BEARSCRAPE](https://www.nsa-observer.net/BEARSCRAPE) - [BEARTRAP](https://www.nsa-observer.net/BEARTRAP) - [BERRYTWISTER](https://www.nsa-observer.net/BERRYTWISTER) - [BERRYTWISTER+](https://www.nsa-observer.net/BERRYTWISTER+) - [BIRDSONG](https://www.nsa-observer.net/BIRDSONG) - [BIRDSTRIKE](https://www.nsa-observer.net/BIRDSTRIKE) - [BOMBAYROLL](https://www.nsa-observer.net/BOMBAYROLL) - [BOMBBAY](https://www.nsa-observer.net/BOMBBAY) - [BRANDYSNAP](https://www.nsa-observer.net/BRANDYSNAP) - [BUGSY](https://www.nsa-observer.net/BUGSY) - [BUMBLEBEEDANCE](https://www.nsa-observer.net/BUMBLEBEEDANCE) - [BUMPERCAR+](https://www.nsa-observer.net/BUMPERCAR+) - [BURLESQUE](https://www.nsa-observer.net/BURLESQUE) - [BYSTANDER](https://www.nsa-observer.net/BYSTANDER) - [CERBERUS](https://www.nsa-observer.net/CERBERUS) - [CERBERUSSTATISTICSCOLLECTION](https://www.nsa-observer.net/CERBERUSSTATISTICSCOLLECTION) - [CHANGELING](https://www.nsa-observer.net/CHANGELING) - [CHINESEFIRECRACKER](https://www.nsa-observer.net/CHINESEFIRECRACKER) - [CLEANSWEEP](https://www.nsa-observer.net/CLEANSWEEP) - [CLUMSYBEEKEEPER](https://www.nsa-observer.net/CLUMSYBEEKEEPER) - [CONCRETEDONKEY](https://www.nsa-observer.net/CONCRETEDONKEY) - [CONDUIT](https://www.nsa-observer.net/CONDUIT) - [CONNONBALL](https://www.nsa-observer.net/CONNONBALL) - [COUNTRYFILE](https://www.nsa-observer.net/COUNTRYFILE) - [CRYOSTAT](https://www.nsa-observer.net/CRYOSTAT) - [CYBERCOMMANDCONSOLE](https://www.nsa-observer.net/CYBERCOMMANDCONSOLE) - [DANCINGBEAR](https://www.nsa-observer.net/DANCINGBEAR) - [DEADPOOL](https://www.nsa-observer.net/DEADPOOL) - [DEERSTALKER](https://www.nsa-observer.net/DEERSTALKER) - [DEVILSHANDSHAKE](https://www.nsa-observer.net/DEVILSHANDSHAKE) - [DIALD](https://www.nsa-observer.net/DIALD) - [DIRTYEVIL](https://www.nsa-observer.net/DIRTYEVIL) - [DOGHANDLER](https://www.nsa-observer.net/DOGHANDLER) - [DRAGON'SSHOUT](https://www.nsa-observer.net/DRAGON'SSHOUT) - [ELATE](https://www.nsa-observer.net/ELATE) - [EXCALIBUR](https://www.nsa-observer.net/EXCALIBUR) - [EXPOW](https://www.nsa-observer.net/EXPOW) - [FATYAK](https://www.nsa-observer.net/FATYAK) - [FORESTWARRIOR](https://www.nsa-observer.net/FORESTWARRIOR) - [FRUITBOWL](https://www.nsa-observer.net/FRUITBOWL) - [FUSEWIRE](https://www.nsa-observer.net/FUSEWIRE) - [GAMBIT](https://www.nsa-observer.net/GAMBIT) - [GATEWAY](https://www.nsa-observer.net/GATEWAY) - [GESTATOR](https://www.nsa-observer.net/GESTATOR) - [GLASSBACK](https://www.nsa-observer.net/GLASSBACK) - [GLITTERBALL](https://www.nsa-observer.net/GLITTERBALL) - [GODFATHER](https://www.nsa-observer.net/GODFATHER) - [GOODFELLA](https://www.nsa-observer.net/GOODFELLA) - [GURKHASSWORD](https://www.nsa-observer.net/GURKHASSWORD) - [HACIENDA](https://www.nsa-observer.net/HACIENDA) - [HAVOK](https://www.nsa-observer.net/HAVOK) - [HOMEPORTAL](https://www.nsa-observer.net/HOMEPORTAL) - [HUSK](https://www.nsa-observer.net/HUSK) - [ICE](https://www.nsa-observer.net/ICE) - [IMPERIALBARGE](https://www.nsa-observer.net/IMPERIALBARGE) - [INSPECTOR](https://www.nsa-observer.net/INSPECTOR) - [JAZZFUSION](https://www.nsa-observer.net/JAZZFUSION) - [JAZZFUSION+](https://www.nsa-observer.net/JAZZFUSION+) - [JEDI](https://www.nsa-observer.net/JEDI) - [JILES](https://www.nsa-observer.net/JILES) - [JTRIGRADIANTSPLENDOUR](https://www.nsa-observer.net/JTRIGRADIANTSPLENDOUR) - [LANDINGPARTY](https://www.nsa-observer.net/LANDINGPARTY) - [LONGSHOT](https://www.nsa-observer.net/LONGSHOT) - [LUMP](https://www.nsa-observer.net/LUMP) - [MINIATUREHERO](https://www.nsa-observer.net/MINIATUREHERO) - [MIRAGE](https://www.nsa-observer.net/MIRAGE) - [MOBILEHOOVER](https://www.nsa-observer.net/MOBILEHOOVER) - [MOLTEN-MAGMA](https://www.nsa-observer.net/MOLTEN-MAGMA) - [MOUTH](https://www.nsa-observer.net/MOUTH) - [MUSTANG](https://www.nsa-observer.net/MUSTANG) - [NAMEJACKER](https://www.nsa-observer.net/NAMEJACKER) - [NEVIS](https://www.nsa-observer.net/NEVIS) - [NEWPIN](https://www.nsa-observer.net/NEWPIN) - [NIGHTCRAWLER](https://www.nsa-observer.net/NIGHTCRAWLER) - [NUTALLERGY](https://www.nsa-observer.net/NUTALLERGY) - [OUTWARD](https://www.nsa-observer.net/OUTWARD) - [PHOTONTORPEDO](https://www.nsa-observer.net/PHOTONTORPEDO) - [PISTRIX](https://www.nsa-observer.net/PISTRIX) - [PITBULL](https://www.nsa-observer.net/PITBULL) - [PODRACE](https://www.nsa-observer.net/PODRACE) - [POISONARROW](https://www.nsa-observer.net/POISONARROW) - [POISONEDDAGGER](https://www.nsa-observer.net/POISONEDDAGGER) - [PREDATORSFACE](https://www.nsa-observer.net/PREDATORSFACE) - [PRIMATE](https://www.nsa-observer.net/PRIMATE) - [QUINCY](https://www.nsa-observer.net/QUINCY) - [RANA](https://www.nsa-observer.net/RANA) - [REAPER](https://www.nsa-observer.net/REAPER) - [RESERVOIR](https://www.nsa-observer.net/RESERVOIR) - [ROLLINGTHUNDER](https://www.nsa-observer.net/ROLLINGTHUNDER) - [SCARLETEMPEROR](https://www.nsa-observer.net/SCARLETEMPEROR) - [SCRAPHEAPCHALLENGE](https://www.nsa-observer.net/SCRAPHEAPCHALLENGE) - [SCREAMINGEAGLE](https://www.nsa-observer.net/SCREAMINGEAGLE) - [SEBACIUM](https://www.nsa-observer.net/SEBACIUM) - [SEPENTSTONGUE](https://www.nsa-observer.net/SEPENTSTONGUE) - [SFL](https://www.nsa-observer.net/SFL) - [SHADOWCAT](https://www.nsa-observer.net/SHADOWCAT) - [SILENTMOVIE](https://www.nsa-observer.net/SILENTMOVIE) - [SILVERBLADE](https://www.nsa-observer.net/SILVERBLADE) - [SILVERFOX](https://www.nsa-observer.net/SILVERFOX) - [SILVERLORD](https://www.nsa-observer.net/SILVERLORD) - [SILVERSPECTER](https://www.nsa-observer.net/SILVERSPECTER) - [SKYSCRAPER](https://www.nsa-observer.net/SKYSCRAPER) - [SLAMMER](https://www.nsa-observer.net/SLAMMER) - [SLIPSTREAM](https://www.nsa-observer.net/SLIPSTREAM) - [SNOOPY](https://www.nsa-observer.net/SNOOPY) - [SODAWATER](https://www.nsa-observer.net/SODAWATER) - [SPACEROCKET](https://www.nsa-observer.net/SPACEROCKET) - [SPICEISLAND](https://www.nsa-observer.net/SPICEISLAND) - [SPRINGBISHOP](https://www.nsa-observer.net/SPRINGBISHOP) - [STEALTHMOOSE](https://www.nsa-observer.net/STEALTHMOOSE) - [SUNBLOCK](https://www.nsa-observer.net/SUNBLOCK) - [SWAMPDONKEY](https://www.nsa-observer.net/SWAMPDONKEY) - [SYLVESTER](https://www.nsa-observer.net/SYLVESTER) - [TANGLEFOOT](https://www.nsa-observer.net/TANGLEFOOT) - [TANNER](https://www.nsa-observer.net/TANNER) - [TECHNOVIKING](https://www.nsa-observer.net/TECHNOVIKING) - [TOPHAT](https://www.nsa-observer.net/TOPHAT) - [TORNADOALLEY](https://www.nsa-observer.net/TORNADOALLEY) - [TRACERFIRE](https://www.nsa-observer.net/TRACERFIRE) - [TWILIGHARROW](https://www.nsa-observer.net/TWILIGHARROW) - [UNDERPASS](https://www.nsa-observer.net/UNDERPASS) - [VIEWER](https://www.nsa-observer.net/VIEWER) - [VIKINGPILLAGE](https://www.nsa-observer.net/VIKINGPILLAGE) - [VIPERSTONGUE](https://www.nsa-observer.net/VIPERSTONGUE) - [WARPATH](https://www.nsa-observer.net/WARPATH) - [WATCHTOWER](https://www.nsa-observer.net/WATCHTOWER) - [WINDFARM](https://www.nsa-observer.net/WINDFARM) - [WURLITZER](https://www.nsa-observer.net/WURLITZER)

