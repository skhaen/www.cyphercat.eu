+++
author = "Skhaen"
categories = ["mua", "mails"]
date = 2016-08-10T17:06:51Z
description = ""
draft = false
slug = "a-la-recherche-dun-client-mail-sous-linux"
tags = ["mua", "mails"]
title = "À la recherche d'un client mail sous Linux"

+++

Mettons tout de suite les choses au clair : on veut un client mail **[moderne](http://www.rainloop.net/screenshots/)**, avec une **[jolie interface](http://www.rainloop.net/screenshots/)** (qui fait [wooosh wooosh](https://twitter.com/Skhaen/status/763371377802043392), exactement) et **orienté grand public**. Dans cette optique, on va d'abord vouloir quelque chose d'**ergonomique** et d'**utilisable dès le premier démarrage** sans devoir installer un plugin ou aller bidouiller un fichier de config. Et l'importance de la gestion de [GPG](https://xkcd.com/364/) arrivera donc nettement après...

![XKCD](https://imgs.xkcd.com/comics/manuals.png)

Donc, histoire que l'on se soit bien compris : on cherche un client mail pour la famille / les ami-e-s / un-e inconnu-e dans la rue, et pas pour un adminsys/dév/agnutollah qui n'arrive à pas comprendre que l'ergonomie et une jolie interface c'est important (et le pire, c'est que je sais très bien que même en écrivant ça, je vais avoir des retours me disant que Mutt c'est trop bien).

Et à voir certaines propositions/réactions sur twitter, il y a encore beaucoup trop de personnes qui n'arrivent pas à comprendre pourquoi [Linux est aussi loin derrière sur le marché du *desktop*](http://itvision.altervista.org/why.linux.is.not.ready.for.the.desktop.current.html#WorksForMe).

Faisons donc un tour de l'existant (si j'ai oublié quelque chose d'intéressant, n'hésitez pas à me contacter par [mail/twitter](https://www.cyphercat.eu/about/)) :

## Logiciels

* **[Astroid](https://github.com/astroidmail/astroid)**
 * Interface pas top (mais GPG)
* **[Bitmask](https://bitmask.net/en)**
 * VPN et GPG
* **[Claws mail](http://www.claws-mail.org/)**
 * Interface pas top (mais GPG)
* **[Evolution](https://wiki.gnome.org/Apps/Evolution)**
 * lourd O_o (mais GPG)
* **[Geary](https://wiki.gnome.org/Apps/Geary)** ([source](https://git.gnome.org/browse/geary/))
 * ça à l'air pas mal du tout, même si apparemment pas beaucoup de choix de configuration
* **[Kontact](https://www.kde.org/applications/internet/kmail/)** (Kmail, intégré dans KDE) 
 * pas testé, donc pas de retour
 * GPG
* **[Mutt](http://www.mutt.org/)**
 * ah pardon, on cherche une interface grand public.
 * mais GPG
* **[Nylas N1](https://www.nylas.com/)** ([source](https://github.com/nylas/N1))
 * Ça a aussi l'air pas mal du tout. Dommage, il faut obligatoirement un [ID Nylas](https://support.nylas.com/hc/en-us/articles/220974588-How-is-a-Nylas-ID-different-from-my-email-address-) pour l'utiliser, et je ne sais pas vraiment ce qu'ils vont faire avec les données.
* **Pantheon Mail** (fork de Geary - [source](https://launchpad.net/pantheon-mail))
* **[Thunderbird](https://www.mozilla.org/en-US/thunderbird/)**
 * *sight*... apparemment toujours la référence sur Linux (j'avoue que mon expérience avec FirefoxOS pendant un an me donne envie de ###### Mozilla...)

## Webmail

* **[Mail-in-a-box](https://mailinabox.email/)**
* **[Mailpile](https://www.mailpile.is/)** : 
 * J'ai entendu pas mal de mauvais échos pendant un moment, si vous l'avez testé je veux bien des retours.
* **[Rainloop](http://www.rainloop.net)**
 * Awwwww <3
 * et GPG
* **[Roundcube](https://roundcube.net/)**
 * la référence du webmail libre, pas mal de thèmes disponibles, dont des sympas
* **[SoGo](https://sogo.nu/)**
 * mails + CalDAV + CardDAV + GroupDAV + Microsoft ActiveSync, plutôt orienté pro.
* **[Zimbra](https://www.zimbra.com)**
 * l'autre référence du (pas que) webmail libre, mais seulement côté serveur (et beaucoup, beaucoup plus gros)


C'est intéressant de voir le nombre de logiciels avec une ergonomie poussive ou un design (un peu) vieux mais avec du support pour GPG. C'est quoi sa part de marché en dehors de notre petit écosystème déjà ?

#### Edit

* 2016-08-22 - ajout de Mail-in-a-box
* 2016-08-11 - ajout de Bitmask et de SoGo

