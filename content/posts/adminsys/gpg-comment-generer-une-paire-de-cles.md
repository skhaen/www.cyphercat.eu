+++
author = "Skhaen"
categories = ["adminsys", "gpg"]
date = 2015-05-19T14:30:00Z
description = ""
draft = false
slug = "gpg-comment-generer-une-paire-de-cles"
tags = ["adminsys", "gpg"]
title = "GPG - Comment générer une paire de clés"

+++

> j'avais publié cet article à l'origine sur [cyphercat.eu](http://www.cyphercat.eu/tuto_gpg.php) il y a quelques temps. Ça servira sans doute plus si ça traine sur ici.


La génération d'une clé asymétrique GPG sous un système Unix / Gnu/Linux est particulièrement simple (une fois que l'on sait le faire ;-)), et c'est justement ce que nous allons voir ici. Si les détails vous intéressent, ou si vous avez besoin d'explications plus détaillées, vous pouvez aller voir sur [gnupg.org](http://www.gnupg.org/) (le site officiel), [wikipedia.fr](http://fr.wikipedia.org/wiki/GNU_Privacy_Guard) (qu'est ce que GPG), ou encore [wikibooks.org](http://fr.wikibooks.org/wiki/GPG) (pour quelques détails). La commande `man gpg` est bien entendu toujours disponible en cas de besoin.

* Créer une clé GPG sous Microsoft Windows
* Créer une clé GPG sous GNU/Linux / Unix
* Générer un certificat de révocation
* Exporter (envoyer) sa clé publique sur un serveur
* Chercher quelqu'un et importer sa clé
* pour aller plus loin


### Créer une clé GPG sous Microsoft Windows

Vous pouvez utiliser le logiciel [GPG4win](http://www.gpg4win.org/license.html) (sous licence [GNU GPL](http://www.gpg4win.org/license.html)).

### Créer une clé GPG sous Mac 

Vous pouvez utiliser le logiciel [gpgtools](https://gpgtools.org/index.html) (sous un [mix de licence](https://gpgtools.org/opensource.html) :/)

### Créer une clé GPG sous GNU/Linux / Unix

Pour commencer, ouvrez un [terminal](http://fr.wikipedia.org/wiki/Shell_Unix) et écrivez (ou copiez/collez) la commande suivante : 

```
gpg --gen-key
```

Le programme vous demande le type de clé que vous voulez :

```
Sélectionnez le type de clé désiré:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (signature seule)
   (4) RSA (signature seule)
Votre choix ?
```

mettre le choix par défaut, c'est à dire **1**.

> Pourquoi RSA et pas DSA ? Si vous n'avez aucune idée de ce que vous faites, vous pouvez soit voir quelques infos ici : [RSA](http://fr.wikipedia.org/wiki/Rivest_Shamir_Adleman) / [DSA](http://fr.wikipedia.org/wiki/Digital_Signature_Algorithm) / [Elgamal](http://fr.wikipedia.org/wiki/Cryptosyst%C3%A8me_de_ElGamal),

Le programme vous demande ensuite la taille de votre clé, le choix par défaut est [2048 bits](http://fr.wikipedia.org/wiki/RSA-2048), mais nous allons mettre *4096* à la place (pour simplifier à l'extrême : plus long = plus de sécurité)

```
les clés RSA peuvent faire entre 1024 et 4096 bits de longueur.
Quelle taille de clé désirez-vous ? (2048)
```

Le choix suivant concerne le temps de validité de la clé, sauf si vous avez un besoin spécifique ou des impératifs en terme de sécurité (renouvellement ponctuel obligatoire, …), le choix par défaut (*0*) sera un bon choix.

```
Spécifiez combien de temps cette clé devrait être valide.
 0 = la clé n'expire pas
   = la clé expire dans n jours
 w = la clé expire dans n semaines
 m = la clé expire dans n mois
 y = la clé expire dans n années
La clé** est valide pour ? (0)
```

Le programme vous demande alors "*La clé n'expire pas du tout. Est-ce correct ? (o/N)*", la réponse est donc "*o*".

Voici la partie "intéressante" : le nom, l'adresse mail et le commentaire. Voici un exemple :

```
Vous avez besoin d'un nom d'utilisateur pour identifier votre clé; le 
programme le construit à partir du nom réel, d'un commentaire et d'une
adresse e-mail de cette manière:
Nom réel: Nobody 
Adresse e-mail: nobody@cyphercat.eu
Commentaire: it's a trap
Vous avez sélectionné ce nom d'utilisateur:
"Nobody (it's a trap) <nobody@cyphercat.eu>"

Changer le (N)om, le (C)ommentaire, l'(E)-mail ou (O)K/(Q)uitter ?
```

Si toutes les infos vous semblent bonnes, vous pouvez valider comme précédemment (avec la touche "*o*", puis valider avec la touche entrée, mais vous avez l'habitude maintenant). Vous pouvez bien entendu mettre ce que vous voulez pour "*nom réel*" : prénom et/ou nom, pseudo, …, vous n'êtes pas obligé de remplir "*commentaire*, et encore une fois, vous pouvez y mettre ce que vous voulez, comme un complément d'information, ou une citation (du moment que ce n'est pas trop long).

Et là, soudainement, l'angoisse ! Le programme vous demande votre *phrase de passe*

```
Entrez la phrase de passe:
```
> **Attention** : vous avez sans doute noté, on parle ici de *phrase de passe* et non de *mot de passe*, je laisse [XKCD](https://xkcd.com/) vous l'expliquer en dessin, ça sera plus rapide : 

[<img src="http://imgs.xkcd.com/comics/password_strength.png">](https://xkcd.com/936/)

Après avoir validé votre mot de passe, le système vous indique que vous devez générer de l'[entropie](http://fr.wiktionary.org/wiki/entropie)

```
Un grand nombre d'octets aléatoires doit être généré. Vous devriez faire 
autre-chose (taper au clavier, déplacer la souris, utiliser les disques) 
pendant la génération de nombres premiers; cela donne au générateur de 
nombres aléatoires une meilleure chance d'avoir assez d'entropie.
```

La solution est simple, mettez en route un film, un ou deux transferts de fichiers sur votre poste, allez vous faire un thé pendant que votre chat joue avec la souris, n'importe quoi produisant une activité sur votre système. Si vous n'en faites pas assez, vous aurez le message suivant :

```
Il n'y a pas assez d'octets aléatoires disponibles. Faites autre chose
pour que l'OS puisse amasser plus d'entropie ! (il faut **** octets de plus)
```

Si, au contraire, vous avez réussi à passer cette étape (ce qui ne demande normalement pas trop d'effort tout de même), vous aurez le plaisir de voir un message ressemblant à celui-là :

```
gpg: clé EBEB2084 marquée comme ayant une confiance ultime.
les clés publique et secrète ont été créées et signées.

gpg: vérifier la base de confiance
gpg: 3 marginale(s) nécessaires, 1 complète(s) nécessaires, modèle
de confiance PGP
gpg: profondeur: 0  valide:   2  signé:   0
confiance: 0-. 0g. 0n. 0m. 0f. 2u
pub   4096R/EBEB2084 2012-07-23
  Empreinte de la clé = E506 DAEC FE0F 7FD0 A413 C974 C075 0814 EBEB 2084
uid  John Doe  <exemple@cyphercat.eu>
sub   4096R/39CCB14A 2012-07-23
```

Il annonce que votre clé GPG est créée. Et nous allons faire une petite pause pour expliquer certaines choses :


* **pub** : 4096R/EBEB2084 2012-07-23

On apprend ici 3 choses : la taille de la clé (4096 bits), l'ID de la clé : EBEB2084 (utile pour la référence et donc pour l'*exporter* ou la *télécharger*) et pour finir sa date de création.

* **L'empreinte** (*fingerprint*) de la clé

```
E506 DAEC FE0F 7FD0 A413 C974 C075 0814 EBEB 2084
```

> **Attention** : il est préférable de prendre le "*longID*" (16 derniers caractères de l'empreinte) et non le "*shortID*" (8 derniers caractères de l'empreinte) pour gérer une clé (ou encore l'empreinte elle-même). Pourquoi ? Tout simplement que depuis 2011, une *collision* est possible avec les *shortID*. Pour plus de détails, lire [asheesh.org](http://www.asheesh.org/note/debian/short-key-ids-are-bad-news.html), [slashdot](http://yro.slashdot.org/story/11/12/27/0044242/gnupg-short-id-collision-has-occurred) ou encore [seclist](http://seclists.org/fulldisclosure/2011/Sep/207).


* E506 DAEC FE0F 7FD0 A413 C974 C075 0814 EBEB 2084   --> fingerprint
* **E506DAECFE0F7FD0A413C974C0750814EBEB2084** --> fingerprint (long mais parfait ID)
* E506DAECFE0F7FD0A413C974C0750814**EBEB2084**   --> short ID
* E506DAECFE0F7FD0A413C974i**C0750814EBEB2084**  --> long ID



L'empreinte sert à vérifier l'authenticité de la clé que vous possédez. Vous pouvez la vérifier avec la commande `gpg --fingerprint KeyID`. C'est l'empreinte que vous donnez/vérifiez quand vous communiquez votre clé publique ou que l'on vous en donne une.

> **NOTE** : le *shortID* (par habitude l'ID, lire l'avertissement juste en dessous) de la clé consiste aux 8 derniers caractères de l'empreinte ;-)

* **uid** : John Doe  <exemple@cyphercat.eu>

Nom ou pseudo (John Doe), ainsi que le commentaire si il y en a un.






### Générer un certificat de révocation

Le [certificat de révocation](http://www.gnupg.org/gph/fr/manual.html#REVOCATION) sert à invalider une clé. Vos correspondants ne pourront alors plus l'utiliser. Il doit être utilisé lorsque quelqu'un est susceptible d'avoir obtenu votre clef privée ou votre mot de passe. Ou si vous avez perdu votre mot de passe par exemple.

```
gpg --gen-revoke KeyID
```

Par exemple, si ont veut générer un certificat de révocation pour la clé créée précédemment, on utilisera la commande suite :

```
gpg --gen-revoke EBEB2084
```

### Exporter (envoyer) sa clé publique sur un serveur

L'exportation de la clé publique sur un annuaire public permet de trouver la clé via les commandes `gpg --search-keys` et `gpg --recv`. Si vous voulez que l'on vous "trouve" facilement, c'est indispensable. La commande, sans option, enverra par défaut sur *keys.gnupg.net*

```
gpg --send-key keyID
```

Exemple :

```
gpg --send-key EBEB2084
```

Pour envoyer une clé sur un serveur précis (vous pouvez en mettre plusieurs en même temps), il faut utiliser la commande suivante :

```
gpg --keyserver serv1 [serv2 serv3 …] --send-keys KeyID1 [KeyID2 ...]
```

Exemple :

```
gpg --keyserver pgp.mit.edu subkeys.pgp.net keys.gnupg.net --send-keys EBEB2084
```

### Chercher quelqu'un et importer sa clé

La commande pour chercher quelqu'un est la suivante : `gpg --search-keys`, voici un exemple (avec *rms* pour [Richard Stallman](http://fr.wikipedia.org/wiki/Richard_Stallman)) :

```
gpg --search-keys rms 

(40)Richard Stallman (Chief GNUisance) <rms@gnu.org>
1024 bit DSA key 135EA668, créé: 2001-03-05
```
À la ~40ème occurence, on trouve donc un Richard Stallman, possédant une adresse rms@gnu.org, ça doit être notre personne. On trouve aussi l'identité de sa clé : 135EA668. 

> **Attention** : ça **peut** être notre personne, n'oubliez pas de vérifier les [signatures](http://www.debian.org/events/keysigning.fr.html) de sa clé et la [chaîne de confiance](http://www.debian.org/events/keysigning.fr.html) en général.

La commande pour importer une clé est tout aussi simple : `gpg --recv`. Voici un exemple (toujours avec *[rms](http://fr.wikipedia.org/wiki/Richard_Stallman)*). Nous venons de voir que l'identité de sa clé est 135EA668 : 

```
gpg --recv 135EA668
```


ce qui donnera :

```
gpg: requête de la clé 135EA668 du serveur hkp keys.gnupg.net
gpg: clé 135EA668: clé publique "Richard Stallman (Chief GNUisance) <rms@gnu.org>" importée
gpg: 3 marginale(s) nécessaires, 1 complète(s) nécessaires, modèle
de confiance PGP
gpg: profondeur: 0  valide:   2  signé:   0
confiance: 0-. 0g. 0n. 0m. 0f. 2u
gpg: Quantité totale traitée: 1
gpg:   importée: 1
```
Et hop, c'est bon \0/

### MOAR

#### Quelques commandes utiles

* Générer une paire de clés : `gpg --gen-key`
* Lister les clés présentes : `gpg --list-keys`
* Exporter une clé publique : `gpg --armor --export keyID`
* Calculer l'empreinte (*fingerprint*) d'une clé : `gpg --fingerprint keyID`
* Envoyer une clé sur serveur de clés : `gpg --send-key keyID`
* Importer une clé : `gpg --import nomFichier`

Vous en voulez encore plus ? Vous pouvez directement aller voir la [documentation de GPG](http://www.gnupg.org/documentation/manuals/gnupg-devel/GPG-Commands.html#GPG-Commands), ça sera le plus rapide.

