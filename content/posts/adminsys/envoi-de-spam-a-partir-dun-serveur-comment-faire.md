+++
author = "Skhaen"
categories = ["adminsys", "postfix", "spam"]
date = 2015-08-04T20:11:00Z
description = ""
draft = false
slug = "envoi-de-spam-a-partir-dun-serveur-comment-faire"
tags = ["adminsys", "postfix", "spam"]
title = "Envoi de spam à partir d'un serveur, comment faire ?"

+++

Au menu aujourd'hui, accueillir comme il se doit sur votre serveur un envoi de spam. Pour changer, on partira du principe que l'on est sur une Debian avec un postfix. Notre serveur s'appelle *exemple.octopuce.fr*, et notre site de référence installé sur ce serveur s'appelle *exemple.com*.

On partira aussi du principe que l'on n'aime pas beaucoup le spam... voir même qu'on lui mettrait bien un bon coup de clavier en pleine gueule.

<iframe width="560" height="315" src="https://www.youtube.com/embed/7mmuQteX1qA" frameborder="0" allowfullscreen></iframe>


## Au commencement était...

Une liste d'envoi de mails (`mailq`) qui se remplit beaucoup trop rapidement, on parle de plusieurs centaines, voir plusieurs milliers de mails en quelques minutes/heures (selon la technique que l'attaquant utilise). Vous pouvez voir en bas de cet article comment être alerté automatiquement.

On commence par regarder la liste d'attente des mails (*mail queue*) avec la commande `mailq`. La commande `less` est là pour éviter d'afficher plusieurs centaines de mails, on aura ainsi seulement les premiers de la liste :

```language-bash
mailq | less
```

Vous devriez voir une suite de blocs qui ressemble à ceci :

```
E35BA5318CA83     1389 Fri Jul 31 10:25:10  MAILER-DAEMON
(delivery temporarily suspended: connect to yahoo.com[63.250.192.45]:25: Connection refused)
                                         darcy.bolden75@yahoo.com

F21DC4093C551     1400 Fri Jul 31 09:36:40  MAILER-DAEMON
(delivery temporarily suspended: connect to yahoo.com[63.250.192.45]:25: Connection refused)
                                         nicki.forth76@yahoo.com

F125553301B52     1409 Fri Jul 31 18:15:21  MAILER-DAEMON
(delivery temporarily suspended: connect to yahoo.com[63.250.192.45]:25: Connection refused)
                                         renato_kalman62@yahoo.com
```

Détaillons le premier bloc :

* `E35BA5318CA83` correspond à l'ID du mail
* et `1389` correspond à la taille du mail en octet
* `Fri Jul 31 10:25:10` correspond à la date de l'envoi (idéal pour le retrouver dans les logs ;-))
* `MAILER-DAEMON` est l'expéditeur (peut être www-data, une adresse mail...)
* `delivery temporarily suspended` explique pourquoi le mail est toujours là (dans notre cas, les serveurs de yahoo ne veulent pas de lui)
* `darcy.bolden75@yahoo.com` représente le destinataire

## Si ça ressemble à du spam, que ça a le goût du spam (et que ça tente de vous vendre du viagra...)...

Je choisis dans mon tas de mail celui avec l'ID B58B18A1C2140 et je "l'ouvre" avec la commande `postcat` (à noter que cette commande affiche par défaut la totalité du mail (header, body, envelope content) :


```
postcat -q B58B18A1C2140|less
```

Et voici le résultat :

```
*** ENVELOPE RECORDS deferred/B/B58B18A1C2140 ***
message_size:             826             195               1               0             826
message_arrival_time: Fri Jul 24 06:08:12 2015
create_time: Fri Jul 24 06:08:12 2015
named_attribute: rewrite_context=local
sender: mechant@exemple.octopuce.fr
*** MESSAGE CONTENTS deferred/B/B58B18A1C2140 ***
Received: by exemple.octopuce.fr (Postfix, from userid 2001)
	id B58B18A1C2140; Fri, 24 Jul 2015 06:08:12 +0200 (CEST)
DKIM-Signature: v=1; a=rsa-sha256; c=simple/simple; d=exemple.com;
	s=alternc; t=1437710892;
	bh=jovdSJ/Kn2/7639VFyVqNVkUC4F7d1BbnAP59auo7XI=;
	h=To:Subject:From:Reply-To:Date;
	b=EkH7RR1L6vi0gkvl/AXhJChbwtOvMTB6KFS/tuoxoo6+O3UINMgxpLqp45vY9E+ED
	 Dl0rOINTF8YDAGFmv6VCU4SDt1FTmWyjcJyefj1Mc9wl3TGRQVPfUDkUhVrrDB3yau
	 Boz8MjYyfBIz28gXWB680XdkC1E5LQwnsmmay//8=
To: anhselagiolong_2710@yahoo.com
Subject: RE:  Your Top Affordable Vagra solutions
X-PHP-Originating-Script: 2001:dir.php
From: "Melba Bauer" <melba_bauer@exemple.com>
Reply-To:"Melba Bauer" <melba_bauer@exemple.com>
X-Priority: 3 (Normal)
MIME-Version: 1.0
Content-Type: text/html; charset="iso-8859-1"
Content-Transfer-Encoding: 8bit
X-RealFrom:
Message-Id: <20150724040812.B58B18A1C2140@exemple.octopuce.fr>
Date: Fri, 24 Jul 2015 06:08:12 +0200 (CEST)


<div>
Your Top Affordable Vagra solutions &ndash; <a href="http://strobeton.ru/assets/plugins/tinymce/jscripts/tiny_mce/plugins/contextmenu/defines.html">check it out</a>
</div>

*** HEADER EXTRACTED deferred/B/B58B18A1C2140 ***
named_attribute: encoding=8bit
original_recipient:
recipient: anhselagiolong_2710@yahoo.com
*** MESSAGE FILE END deferred/B/B58B18A1C2140 ***
```

Les points importants :

* **create_time**: Fri Jul 24 06:08:12 2015 (date de création)
* **Received**: by exemple.octopuce.fr (Postfix, from **userid** 2001) (reçu par)
* **sender**: exemple@exemple.octopuce.fr (si c'est une de vos adresses mails qui existe réellement, c'est simple : changement de mot de passe immédiat).
* **X-PHP-Originating-Script**: 2001:dir.php (origine de l'envoi (fichier vérolé, formulaire sans captcha...))

Et puis bon, celui là il est facile, il tente de nous vendre du viagra... je pouvais pas avoir plus stéréotypé comme exemple...


## Viens, on joue à cache-cache \o/

À partir de là, la marche à suivre est la même que dans l'article "[remonter une attaque avec les logs d'apache2](https://www.libwalk.so/2015/04/29/analyse-log.html)". Nous allons commencer notre chasse dans le fichier `/var/log/mail.log`.

On commence par aller voir au moment de l'attaque :

> astuce : `/` permet de faire une [recherche dans `less`](http://www.tuteurs.ens.fr/unix/exercices/solutions/less-sol.html), comme par exemple `2015:06:0` si vous n'avez qu'une seule journée par fichier


```
less /var/log/mail.log

176.9.139.10 - - [24/Jul/2015:06:07:18 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 382 "-" "Mozill
a/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 5
129.195.0.205 - - [24/Jul/2015:06:07:27 +0200] "GET /wp-includes/js/comment-reply.min.js?ver=4.1.4 HTTP/1.1" 304 - "-" "Mozilla/4.0 (compatible;)" 0
89.184.77.50 - - [24/Jul/2015:06:07:26 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 466 "-" "Mozill
a/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 2
89.184.77.50 - - [24/Jul/2015:06:07:34 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 219 "-" "Mozill
a/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 1
176.9.139.10 - - [24/Jul/2015:06:07:28 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 64 "-" "Mozilla
/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 10
89.184.77.50 - - [24/Jul/2015:06:07:41 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 541 "-" "Mozill
a/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 2
89.184.77.50 - - [24/Jul/2015:06:07:48 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 370 "-" "Mozill
a/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 1
176.9.139.10 - - [24/Jul/2015:06:08:12 +0200] "POST /wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php HTTP/1.1" 200 374 "-" "Mozill
a/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.7.6)" 9
```

> <Mode débutant> oh bah si c'est [google analytics] ça doit pas être ça...

OU PAS ! [Comme nous l'avions vu](https://www.libwalk.so/2015/04/29/analyse-log.html), nous recherchons des requêtes POST dans les logs, et là nous en avons une jolie fournée ! Les "pirates" étant des animaux fourbes et rusés, ils n'hésitent pas à mettre des faux noms, comme des fichiers wordpress dans l'arborescence d'un drupal...

Allons voir de plus près ce fichier !

```
less /var/www/alternc/m/monjolisite/wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php
```

On y trouve quelque chose comme ceci :

![virusvirusvirus.png](/images/2015/09/virusvirusvirus.png)

Et là, on appelle ça un fichier vérolé :]

> Note : si vous n'avez que ça dans votre fichier, vous pouvez l'effacer directement après avoir remonté entièrement l'attaque. Si vous avez du code "valide" dans le fichier, vous pouvez enlever seulement la partie incompréhensible.


Juste pour savoir, j'ai eu combien de requêtes sur ce fichier depuis 4 jours ?
```
cat access-20150721.log access-20150722.log access-20150723.log access-20150724.log|grep -c "/wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php"
3204
```

Ah oui quand même... Et avec combien d'IPs différentes ?

```
cat access-20150721.log access-20150722.log access-20150723.log access-20150724.log|grep "/wp-content/plugins/wsanalytics-google-analytics-and-dashboards/images/dir.php"|awk '{print $1}'|sort -gu|wc -l
123
```
Comme vous vous en doutez, la "traque" ne s'arrête pas là. Je vous renvoie à l'article [remonter une attaque avec les logs d'Apache2](https://www.libwalk.so/2015/04/29/analyse-log.html), vu que c'est là que ça va se passer. Je vous conseille de finir de trouver la faille avant de passer à la suite.

### KILL EVERYTHING WITH FIRE

<img src="/images/2015/colin-furze-xmen.jpg">

Mais avant, on va quand même faire une sauvegarde, parce que l'on est jamais à l'abri d'une connerie... (la commande suivante vous permet de copier votre mailq dans le dossier `/var/tmp/svg-mail`) :

```
mkdir /var/tmp/svg-mail
rsync -avP /var/spool/mail/ /var/tmp/svg-mail
```

Reprenons notre lance-flammes. Comme vous l'avez vu au début de cet article, la commande `mailq` est assez verbeuse, et nous avons juste besoin de l'ID des mails pour les effacer. La commande qu'il nous faut est la suivante (en changeant MOTIF, bien entendu...):

```
mailq|grep MOTIF|awk '{print $1}'|postsuper -d -
```
* `mailq` -> affiche la liste des mails en attente
* `grep` MOTIF --> recherche les lignes avec le *motif* que nous voulons (ici ce serait `MAILER-DAEMON`)
* `awk '{print $1}'` -> affiche la première partie de la ligne, ça correspond ici à l'ID des mails
* `postsuper -d -` -->  indique à la commande `postsuper` d'effacer (`-d` pour *delete*) les mails qu'on lui donne.

Et ça nous donnera quelque chose ressemblant à ceci :

```
mailq|grep MOTIF|awk '{print $1}'|postsuper -d -
[...]
postsuper: F21198830C2B1: removed
postsuper: F108B894D4171: removed
postsuper: F22718A48D820: removed
postsuper: F11EB8FB2A9E7: removed
postsuper: F02B78A14E94D: removed
postsuper: F0B5B8D42374F: removed
postsuper: F02BB8D422BEC: removed
postsuper: Deleted: 17047 messages
```

### Spamhaus

Si votre serveur a envoyé (beaucoup) de spam, il est sans doute dans les listes de différents organismes (comme les RBL, pour *Realtime blacklist*), qui fournissent eux mêmes des systèmes antispams. Les plus célèbres d'entre eux sont sans doute [anti-abuse.org](http://www.anti-abuse.org/multi-rbl-check/) et [spamhaus](https://www.spamhaus.org/) qui a le mérite (en plus de faire du très bon travail) d'avoir un site plutôt bien foutu, sur lequel on peut **vérifier** et **débloquer** son serveur s'il est considéré comme spammeur : [spamhaus.org/lookup/](https://www.spamhaus.org/lookup/).

## Améliorons encore

### Supervision

Il existe des outils pour vous aider à superviser et à vous alerter en cas de problèmes. Ils peuvent se présenter sous la forme de [script "à la main"](https://stackoverflow.com/questions/226699/how-to-monitor-postfix-mta-status/226913#226913) (merci [stackoverflow](http://geekandpoke.typepad.com/geekandpoke/2013/01/stackoverflow.html)). Des checks pour nagios (que ce soit sur les [RBLs](https://exchange.nagios.org/directory/Plugins/Security/check_dnsbl/details)) ou la [taille de la mailq](https://exchange.nagios.org/directory/Plugins/Email-and-Groupware/Postfix/check_postfix_queue/details). On peut aussi parler de [mailgraph](http://mailgraph.schweikert.ch/) et de [pflogsumm](http://linux.die.net/man/1/pflogsumm).

À noter que [anti-abuse.org](http://www.anti-abuse.org/multi-rbl-check/) fournit un service en ligne, [rblmon.com](http://www.rblmon.com/) qui peut vous alerter automatiquement quand une de vos IPs arrivent dans une RBL.

### PHP / mail.log

Pour retrouver plus facilement les fichiers ayant servi à envoyer du spam, n'hésitez pas à les logger via l'option `mail.log` de votre `php.ini` :

```
vim /etc/php5/apache2/php.ini

## trouver la ligne "mail.log" et la compléter comme ceci :
mail.log = /var/log/mail.php.log
```

Puis on crée le fichier (n'hésitez pas à modifier les permissions si le coeur vous en dit):

```
touch /var/log/mail.php.log && chmod 777 /var/log/mail.php.log
```

### SPF, DKIM et DMARC

Au fur et à mesure du temps, des techniques ont été inventées pour lutter contre le spam et authentifier les expéditeurs "légaux". On parle ici de 3 choses :

* [SPF](https://fr.wikipedia.org/wiki/Sender_Policy_Framework), pour *Sender Policy Framework*,
* [DKIM](https://fr.wikipedia.org/wiki/DomainKeys_Identified_Mail) pour *DomainKeys Identified Mail*,
* [DMARC](https://fr.wikipedia.org/wiki/DMARC) pour *Domain-based Message Authentication, Reporting and Conformance*.

Je vous conseille **vivement** de les configurer sur vos serveurs de mails. Vous pouvez lire à ce sujet [les articles](https://www.debian-administration.org/tag/spf) de Steve Kemp.

Des outils le font par défaut, comme [AlternC](https://github.com/AlternC/AlternC).

### antivirus

Si vous avez un antivirus, par exemple [clamav](http://www.clamav.net/doc/install.html) (idéalement en y ajoutant les signatures de [maldet](https://www.rfxn.com/projects/linux-malware-detect/)) sur votre serveur, il peut vous aider à (re)trouver des fichiers vérolés :


```
clamscan -ri /var/www/

/var/www/search.php: Php.Trojan.StopPost FOUND
/var/www/help.php: Php.Trojan.StopPost FOUND

----------- SCAN SUMMARY -----------
Known viruses: 3912137
Engine version: 0.98.5
Scanned directories: 550
Scanned files: 78469
Infected files: 2
Data scanned: 1382.68 MB
Data read: 1509.90 MB (ratio 0.92:1)
Time: 132.390 sec (2 m 12 s)
```

