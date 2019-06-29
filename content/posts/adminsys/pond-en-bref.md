+++
author = "Skhaen"
categories = ["adminsys", "crypto"]
date = 2014-04-19T10:00:00Z
description = ""
draft = false
slug = "pond-en-bref"
tags = ["adminsys", "crypto"]
title = "Pond en bref"

+++

Après avoir relu les différentes raisons de ne pas utiliser PGP (« *[15 reasons not to start using PGP](15 reasons not to start using PGP)* »), et accepter que ce n'est pas faux, voir même plutôt vrai, je suis retombé en fin d'article sur le projet [Pond]( https://pond.imperialviolet.org/). Et il faut le dire : ça donne envie, même si la peinture n'est pas sèche.


Quelques fonctionnalités en vrac :

* obligation de passer par le réseau Tor
* OTR ([Off-the-Record Messaging](https://otr.cypherpunks.ca/)),
* pas de spam (seules les personnes identifiées peuvent communiquer avec vous)
* les messages sont effacés au bout d'une semaine après la réception
* les messages sont effacés au bout de deux semaines sur les serveurs si vous ne les avez pas reçus.
* les serveurs sont des services cachés Tor (*[Tor hidden services](https://www.torproject.org/docs/hidden-services.html.en)*).


Pour plus de détails, vous pouvez voir :

* la [partie technique](https://pond.imperialviolet.org/tech.html),
* le [modèle de menaces](https://pond.imperialviolet.org/threat.html) utilisé.

# Installation

Nous allons installer Pond à partir des [paquets pré-compilés](https://pond.imperialviolet.org/), vous pouvez bien entendu le compiler vous même si le coeur vous en dit. Comme vous pouvez le voir, il existe des paquets pour quelques systèmes : Ubuntu 13.10, Debian Wheezy, Fedora et OSX 10.9.

Cette installation concerne [Debian Wheezy](http://debian.org/).

## préparation
* on commence par installer les paquets nécessaires :

```
aptitude install libtspi1 libgtkspell-3-0

```
        
## pond

* on télécharge le paquet pré-compilé pour debian :


```
wget https://pond.imperialviolet.org/binary/debian/pond.gpg

```

* on vérifie la signature du paquet :


```
gpg pond.gpg

```

Ce qui vous donne quelque chose comme ceci ([en savoir plus sur gpg](http://www.gnupg.org/howtos/fr/index.html)): 


```
gpg: Signature faite le dim. 10 nov. 2013 21:39:30 CET avec la  clef RSA d'identifiant F02C5704
gpg: Bonne signature de « Adam Langley <agl@imperialviolet.org> »
gpg: Attention : cette clef n'est pas certifiée avec une signature de confiance.
gpg: Rien n'indique que la signature appartient à son propriétaire.
Empreinte de clef principale : C921 7238 4F38 7DBA ED4D  4201 65EB 9636 F02C 5704
```

* on rend le fichier exécutable (vous avez maintenant 2 fichiers, **pond.gpg** et **pond**) :


```
chmod +x pond

```

## Tor

Il est nécessaire d'avoir le logiciel Tor lancé pour permettre à Pond de fonctionner, le [Tor Browser Bundle](https://www.torproject.org/projects/torbrowser.html.en#downloads) fait parfaitement l'affaire. On peut le télécharger sur [cette page](https://www.torproject.org/projects/torbrowser.html.en#downloads) en choisissant sa langue et la bonne version selon son architecture (32 ou 64 bits) :

* pour connaitre son architecture ( x86_64 = 64 bits / i686 = 32 bits)

```
uname -m

```

* j'ai une architecture 64 bits, et je prends en français :


```
wget https://www.torproject.org/dist/torbrowser/3.5.3/tor-browser-linux64-x.x.x_fr.tar.xz
```

* on décompresse après le téléchargement


```
tar xvf tor-browser-linux64-x.x.x_fr.tar.xz
cd tor-browser_fr

```

On peut alors voir plusieurs fichiers (par exemple avec la commande **ls**) dont *start-tor-browser* qui nous intéresse pour la suite.

# Utilisation

* on lance Tor (je vous laisse répondre aux éventuelles questions)


```
./start-tor-browser
```
        
* on lance Pond (n'oubliez pas de revenir dans le bon dossier pour ça)


```
./pond
```
        
La fenêtre suivante s'ouvre alors 

![](/content/images/2015/09/Pond_010-1.png)

* On choisit une bonne **[phrase de passe](https://www.keepassx.org/)** puis on valide. Pour info, le fichier de pond se trouve dans *~/.config/pond* (vous pouvez voir son emplacement dans *identity*).

## ajouter un contact

Avant toute chose, on choisit l'alias du nouveau contact, APRÈS on choisit entre le secret partagé où l'échange de clé.

### secret partagé

Consiste simplement en un secret partagé (oui, c'est facile, c'est le titre) : vous définissez une phrase avec votre interlocuteur que vous rentrez chacun de votre côté dans votre pond (avec si vous le voulez les 2 étapes suivantes).

* **facultatif** : secret partagé + un paquet de cartes :

Je vous laisse lire l'explication de [Bruce Schneier]((https://www.schneier.com/solitaire.html)) sur ce sujet.

* **facultatif 2** : secret partagé + un paquet de cartes + la date du jour comme *[salt](https://en.wikipedia.org/wiki/Salt_%28cryptography%29)* :

La même chose, mais en rajoutant une date choisie pour ajouter un facteur aléatoire.

### échange de clés

Il existe une autre solution, qui consiste à échanger deux clés avec ton interlocuteur. Vous pouvez le faire par *Add* puis *Manual Keying*.

Vous avez un bloc de caractères ressemblant à ceci :

```
-----BEGIN POND KEY EXCHANGE-----
	Cs0GCiClnO8gPGj/LTfHORvfWSrAwwnIW9Y19cejMspY3baD4xIgPsUshFslEolA
htRZ78BkKd3/G7XQOQ8u6nkxFUjUTUEaWHBvbmRzZXJ2ZXI6Ly9JQ1lVSFNBWUdJ
WFRLWUtYU0FISUJXRUFRQ1RFRjI2V1VXRVBPVkM3NjRXWUVMQ0pNVVBBQGpiNjQ0
emFwamU1ZHZnazMub25pb24iII59PgPcWl8gX2K6IizjzJXj4smvjKSbKChgwdBL
	[...]
/5dRxErpgNvQZptIqk5KNMCrylE8LDZg5s0Wsgsk3BkSQJqXumIWS7XrtQ+3xYVg
q6Z8seL7srGDOpZNnTfZy/v7O7ebHieZOLLlUj1Afw4/S28QPRpA7sHZQxIjPi01
ewY=
-----END POND KEY EXCHANGE-----
```
   
en dessous, il faut le transmettre à votre interlocteur, qui vous donnera le sien. **Attention** : ce bloc de texte correspond à UNE seule personne. À chaque fois que vous ajoutez un contact le bloc sera différent.

## envoyer un message

Cliquer sur *compose* en haut à gauche, choisir son destinataire (*TO*) qui est déjà authentifié (sinon ce n'est pas possible de lui envoyer quelque chose) écrire son message. Pour finir, appuyer sur *Send*. Simple non ?

## Quelques notes :

* Quand vous envoyez un message, il est rouge si il n'est pas lu, vert si il l'est.
* en allant dans *Activity log* vous pouvez, en appuyant sur *Transact now* (en haut à droite) demander à Pond de faire ses appels serveurs avant la prochaine demande programmée.
* ce n'est pas fait pour faire du tchat (loin de là même), ça serait plutôt l'équivalent du mail dans l'absolu)

C'est prometteur, mais ça reste de la (pré-)béta.

Je crois que je peux difficilement faire plus bref.

