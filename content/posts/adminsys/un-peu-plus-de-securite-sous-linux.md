+++
author = "Skhaen"
categories = ["adminsys"]
date = 2015-02-15T16:10:00Z
description = ""
draft = false
slug = "un-peu-plus-de-securite-sous-linux"
tags = ["adminsys"]
title = "Un peu plus de sécurité sous Linux"

+++

Au programme de cet article (testé sur Debian Wheezy) :

* Mises à jour : `cron-apt` et `checkrestart`
* Sécurité 
 * virus : `clamav`, 
 * malwares `maldet`,
 * rootkits `rkhunter`, `chkrootkit`, `lynis`
 * vérification de l'intégrité des paquets : `debsums`
* nettoyage : `deborphan`, ...
* backups


###### IMPORTANT : 
* la plupart des commandes ci-dessous nécessitent d'être root ou d'utiliser la commande `sudo`,
* tous ces logiciels ont normalement [des logs](https://www.debian-fr.org/consulter-les-logs-quoi-ou-que-rechercher-t27590.html) dans `/var/log`, et c'est TRÈS utile pour trouver [pourquoi ça ne marche pas](https://www.debian-fr.org/consulter-les-logs-quoi-ou-que-rechercher-t27590.html),
* il faut avoir un logiciel de mail configuré (comme `postfix`) pour pouvoir envoyer des mails,
* n'oubliez pas de RELANCER le programme après une modification pour la prendre en comptes?


## mises à jour : [cron-apt](https://packages.debian.org/fr/wheezy/cron-apt)

[cron-apt](https://packages.debian.org/fr/wheezy/cron-apt) permet d'alerter quand une ou plusieurs mises à jour sont disponible pour le serveur mais aussi de les faires automatiquement en envoyant ou non un mail pour prévenir (à noter que [apticron](https://packages.debian.org/wheezy/apticron) permet aussi d'alerter en cas de mises à jours disponibles).

Pour commencer, le plus important : avoir des sources de mises à jour propres. Pour bien comprendre comment fonctionnent les [sources.list](https://wiki.debian.org/fr/SourcesList), vous pouvez vous référer à la [documentation de Debian](https://wiki.debian.org/fr/SourcesList), vous pouvez aussi vous aider du "[Debian Sources List Generator](http://debgen.simplylinux.ch/)" pour générer votre fichier.

En cas de mises à jour automatiques, il est VITAL d'avoir des sources "propres", il faut éviter au maximum des conflits éventuels entre des paquets ou une configuration trop exotique. Mieux vaut réfléchir avant de mettre en place un système de MàJ auto sur un système en production, en particulier si il y a une supervision avec une astreinte (et encore plus si c'est vous d'astreinte).


* installer `cron-apt` :

```language-bash
aptitude install cron-apt
```

* éditer le fichier de configuration `/etc/cron-apt/config` : 
 * `APTCOMMAND` définit le gestionnaire de paquets que vous voulez utilisez (`apt-get` ou `aptitude`)
 * `MAILTO` l'adresse à laquelle le système va envoyer les mails de notifications.

```language-bash
APTCOMMAND=/usr/bin/aptitude
MAILTO="mail@exemple.org" 
```

Par défaut, `cron-apt` est lancé chaque jour à 4 heure du mat'. Libre à vous de [modifier ce comportement](https://www.debian-administration.org/article/56/Command_scheduling_with_cron) dans le fichier `/etc/cron.d/cron-apt`.


#### upgrade automatiques


Pour installer automatiquement les mises à jour disponibles, il est nécessaire de créer un fichier nommé `5-install` dans le répertoire `/etc/cron-apt/action.d/` puis d'y ajouter la ligne suivante :

```language-bash
dist-upgrade -y -o APT::Get::Show-Upgraded=true
```

Pour limiter le process aux mises à jour de sécurité, il faut créer un fichier nommé `/etc/apt/security.list` pour y mettre ceci :


```language-bash
deb http://security.debian.org/ wheezy/updates main contrib non-free
deb-src http://security.debian.org/ wheezy/updates main contrib non-free
```

Il suffit alors de décommenter dans le fichier `/etc/cron-apt/config` la ligne suivante :

```language-bash
OPTIONS="-o quiet=1 -o Dir::Etc::SourceList=/etc/apt/security.list"
```

Dorénavant `cron-apt` utilisera uniquement le fichier `security.list` pour vérifier la présence de mises à jour, et ignorera donc les mises à jour autres que celles de sécurité.




## checkrestart ([debian-goodies](https://packages.debian.org/fr/wheezy/debian-goodies))

Le paquet [debian-goodies](https://packages.debian.org/fr/wheezy/debian-goodies) installe sur votre machine quelques scripts dont `checkrestart`, qui permet de trouver des processus utilisant de vieilles versions de bibliothèques logicielles. Ce genre de vérification est particulièrement importante après un patch sécurité important, comme pour libc6 en Janvier 2015 ([CVE-2015-0235](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-0235)). 

Pour installer le paquet `debian-goodies` (la commande `checkrestart` suffit pour le lancer par la suite) :

```
aptitude install debian-goodies
```


Le script est donc particulièrement utile, mais comme on peut souvent le voir, il ne sait pas reconnaitre et/ou relancer certains processus,  on peut donc ajouter des fonctionnalités supplémentaires, c'est ce qu'a fait [Octopuce](https://www.octopuce.fr/checkrestart-outil-pratique-de-debian-goodies-et-version-octopuce/) via un script qui utilise `checkrestart` en y ajoutant des fonctionnalités. 

Ce script est disponible sur [github](https://github.com/octopuce/octopuce-goodies/tree/master/checkrestart) et l'on peut trouver un [article introductif](https://www.octopuce.fr/checkrestart-outil-pratique-de-debian-goodies-et-version-octopuce/) sur le site d'Octopuce.

Pour le mettre en place :

```
cd /usr/local/bin/
wget https://www.octopuce.fr/octopuce-goodies/checkrestart/checkrestart.octopuce
chmod +x checkrestart.octopuce
```

On peut alors lancer le script de n'importe où sur le système (n'hésitez pas à [remonter des bugs](https://github.com/octopuce/octopuce-goodies/issues) ou à [modifier le code](https://github.com/octopuce/octopuce-goodies/tree/master/checkrestart)) :


```
checkrestart.octopuce
```



## [clamav](https://packages.debian.org/fr/wheezy/clamav) (antivirus) et [maldet](https://www.rfxn.com/projects/linux-malware-detect/) (antimalwares)

L'antivirus libre [clamav](https://packages.debian.org/fr/wheezy/clamav) est connu dans le monde du logiciel libre et son efficacité peut être améliorée en y ajoutant la base de signatures du [Linux Malware Detector](https://www.rfxn.com/projects/linux-malware-detect/).

### [clamav](https://packages.debian.org/fr/wheezy/clamav)

Pour l'installer : 

```
aptitude install clamav clamav-freshclam
```

* mettre à jour `clamav` :

éditer le fichier de configuration `/etc/clamav/freshclam.conf` selon votre choix (vous pouvez d'ailleurs voir [une description rapide des options sur ce site](http://linux.die.net/man/5/freshclam.conf)) :

```
DatabaseOwner clamav
UpdateLogFile /var/log/clamav/freshclam.log
LogVerbose false
LogSyslog false
LogFacility LOG_LOCAL6
LogFileMaxSize 0
LogTime true
Foreground false
Debug false
MaxAttempts 5
DatabaseDirectory /var/lib/clamav
DNSDatabaseInfo current.cvd.clamav.net
AllowSupplementaryGroups false
PidFile /var/run/clamav/freshclam.pid
ConnectTimeout 30
ReceiveTimeout 30
TestDatabases yes
ScriptedUpdates yes
CompressLocalDatabase no
Bytecode true
# Check for new database 12 times a day
Checks 12
DatabaseMirror database.clamav.net
```

Vous pouvez ensuite lancer la mise à jour avec la commande `freshclam`.

* lancer un scan

Vous pouvez créer un second fichier de configuration pour `clamav` sous le nom de `/etc/clamav/clamd.conf`. Ce fichier sert à donner une configuration par défaut pour les scans. Vous trouverez ci-dessous un EXEMPLE de configuration, vous pouvez le modifier selon vos désirs :


```
LogVerbose false
PidFile /var/run/clamav/clamd.pid
DatabaseDirectory /var/lib/clamav
#OfficialDatabaseOnly true
SelfCheck 3600
Foreground false
Debug false
ScanPE true
MaxEmbeddedPE 10M
ScanOLE2 true
ScanPDF true
ScanHTML true
MaxHTMLNormalize 10M
MaxHTMLNoTags 2M
MaxScriptNormalize 5M
MaxZipTypeRcg 1M
ScanSWF true
DetectBrokenExecutables false
ExitOnOOM false
LeaveTemporaryFiles false
AlgorithmicDetection true
ScanELF true
IdleTimeout 30
CrossFilesystems true
PhishingSignatures true
PhishingScanURLs true
PhishingAlwaysBlockSSLMismatch false
PhishingAlwaysBlockCloak false
PartitionIntersection false
DetectPUA false
ScanPartialMessages false
HeuristicScanPrecedence false
StructuredDataDetection false
CommandReadTimeout 5
SendBufTimeout 200
MaxQueue 100
ExtendedDetectionInfo true
OLE2BlockMacros false
ScanOnAccess false
AllowAllMatchScan true
ForceToDisk false
DisableCertCheck false
DisableCache false
MaxScanSize 100M
MaxFileSize 25M
MaxRecursion 10
MaxFiles 10000
MaxPartitions 50
MaxIconsPE 100
StatsEnabled false
StatsPEDisabled true
StatsHostID auto
StatsTimeout 10
StreamMaxLength 25M
LogFile /var/log/clamav/clamav.log
LogTime true
LogFileUnlock false
LogFileMaxSize 0
Bytecode true
BytecodeSecurity TrustSigned
BytecodeTimeout 60000
```

### [Linux Malware detector](https://www.rfxn.com/projects/linux-malware-detect/) (maldet)

Installer maldet : 

```bash
wget http://www.rfxn.com/downloads/maldetect-current.tar.gz
tar xfz maldetect-current.tar.gz
cd maldetect-*
./install.sh
```

Pour mettre la base de signatures à jour :

```
maldet -u
```

Pour affiner vos réglages, avoir des informations par mail, configurer la mise en quarantaine, le fichier de configuration se trouve dans `/usr/local/maldetect/conf.maldet`

#### ajouter la base de signatures de MalDet à Clamav : 

Il est possible de rajouter la base de signatures de `maldet` à `clamav`, ce qui permet de scanner une seule fois avec les deux bases de signatures. Après avoir installé `maldet`, vous pouvez faire la manipulation suivante :

```bash
cd /var/lib/clamav
ln -s /usr/local/maldetect/sigs/rfxn.hdb rfxn.hdb
ln -s  /usr/local/maldetect/sigs/rfxn.ndb rfxn.ndb
/etc/init.d/clamd restart
```

Vous pouvez utiliser la commande suivante pour lancer un scan, `--recursive` permet de descendre dans l'arborescence, `--infected` permet de n'afficher que les résultats positifs lors du scan pour éviter une sortie particulièrement verbeuse, et `--log` permet de définir un fichier ou le résultat du scan sera écrit  :

```
clamscan --recursive --infected --log="/var/log/clamscan"
```
## [rkhunter](https://packages.debian.org/fr/wheezy/rkhunter)

Pour l'installer puis le mettre à jour : 
```
aptitude install rkhunter
rkhunter --update
```

Pour dire à `rkhunter` de regarder l'état de certains fichiers de configurations et de les stocker pour détecter des altérations par la suite (à faire idéalement sur un système neuf) :

```
rkhunter --propupd
```

Pour lancer un test (n'oubliez pas que le résultat sera aussi dans `/var/log/rkhunter.log`) : 

```
rkhunter --check --skip-keypress --report-warnings-only
```


Le fichier de configuration se trouve dans `/etc/rkhunter.conf`, vous pouvez y configurer, entre autre, votre adresse mail pour recevoir les alertes. `rkhunter` peut générer des faux positifs, vous pouvez trouver dans la [documentation d'Ubuntu ](http://doc.ubuntu-fr.org/rkhunter) comment l'éviter.

Plus de détails dans [cet article très complet sur sublimigeek.com](http://www.sublimigeek.fr/installer-paquet-rkhunter-debian).


## [chkrootkit](https://packages.debian.org/fr/wheezy/chkrootkit)


chkrootkit est un anti-rootkit connu sous Linux, mais il faut noter que la version disponible dans wheezy est la [0.49](https://packages.debian.org/fr/wheezy/chkrootkit) qui date de 2008, la [0.50](https://packages.debian.org/fr/jessie/chkrootkit), sorti en 2014 est disponible dans Jessie.

```
aptitude install chkrootkit
```

Le fichier de configuration `/etc/chkrootkit/chkrootkit.conf` :

```
RUN_DAILY="true"
RUN_DAILY_OPTS="-q" # -q=quiet mode
DIFF_MODE="true" # garde un /var/cache/chkrootkit/log.old pour comparer la prochaine fois
REPORT_MAIL=mail@example.net
```

Pour le lancer :

```
chkrootkit -q
```


## [lynis](https://packages.debian.org/wheezy/lynis)

Développé par le créateur de [rkhunter](http://www.sublimigeek.fr/installer-paquet-rkhunter-debian), Lynis est un outil d'audit pour Unix. Il parcourt la configuration du système et crée un résumé des informations système et des problèmes de sécurité, utilisable par des auditeurs professionnels. Il peut aider à des audits automatisés.

```
aptitude install lynis
```

Pour lancer une vérification du système avec seulement l'affichage des warnings :

```
lynis --check-all --quick
```

Vous trouverez le rapport dans `/var/log/lynis-report.dat` et sa [documentation](https://cisofy.com/documentation/lynis/) pour aller plus loin sur le site officiel.

## [debsums](https://packages.debian.org/fr/wheezy/debsums)

 Debsums permet de vérifier l'intégrité des fichiers des paquets installés avec les sommes de contrôle MD5 installées par le paquet ou générées à partir d'une archive ".deb". 


Pour l'installer :

```
aptitude install debsums

```

Pour le lancer (vérifie les fichiers de configurations `--all` et n'affiche que les erreurs : `--silent`) :

```
debsums --all --silent
```

Vous avez bien entendu différentes options que vous pouvez voir dans les pages man (`man debsums`). Pour génèrer les sommes de contrôle MD5 à partir des fichiers .deb  pour  les  paquets qui n'en fournissent pas : `debsums --generate=missing` (équivalent de `-g`). Cette commande est bien entendu à utiliser sur un système neuf.




## Nettoyer

Quand vous désinstallez un paquet de votre machine, [il arrive qu'il soit enlevé mais que les fichiers de configurations restent](http://www.fernandoike.com/2014/09/12/purge-debian-packages-marked-with-rc-status/), il est alors marqué "[rc](http://www.fernandoike.com/2014/09/12/purge-debian-packages-marked-with-rc-status/)" dans `dpkg`, pour supprimer définitivement ces paquets, vous pouvez lancer les commandes suivantes :

* Afficher les paquets étant dans un état "rc" :

```
dpkg -l |grep ^rc | awk '{print $2}'
```

* Supprimer ces paquets :

```
dpkg -l | grep ^rc | awk '{print $2}' | xargs dpkg -P
```
À noter que cette commande peut éteindre MySQL pour effacer d'anciens fichiers de configuration, n'oubliez pas de de le relancer par la suite.


### [deborphan](https://packages.debian.org/fr/wheezy/deborphan)

[Deborphan](https://packages.debian.org/fr/wheezy/deborphan) recherche les paquets orphelins sur votre système en déterminant quels paquets n'ont aucune dépendance sur eux et vous affiche la liste de ces paquets.

```
aptitude install deborphan
```

Je vous conseille grandement de vérifier ce qu'il propose d'enlever avant de supprimer les paquets :

```
deborphan
```

Et si ça vous semble correct, vous pouvez supprimer les paquets avec la commande `dpkg` :

```
deborphan| xargs dpkg -P
```

Il peut être nécessaire de le relancer plusieurs fois (en vérifiant ou non ce qu'il veut effacer à chaque fois).


## BACKUUUUUUUUUUUUUUUUUUPS


C'est sans aucun doute la partie la plus importante de cet article :

FAITES DES SAUVEGARDES !

[Octopuce](https://www.octopuce.fr) a mis en ligne un petit script qui vous aidera à faire rapidement et simplement des backups sur un disque dur externe (ou une clé USB avec assez de place) : [faire des backups facilement](https://www.octopuce.fr/faire-des-backups-facilement/).

Plus d'excuses, au boulot !


## Pour aller un peu plus loin

faire écouter vos programmes en local s'ils n'ont pas besoin d'être atteint de l'extérieur. C'est normalement la configuration par défaut dans Debian, mais si vous installez des paquets externes, vous pouvez le vérifier facilement avec la commande `netstat -lptn`.

Pour continuer cet article, n'oubliez surtout pas de vérifier et d'[améliorer vos configurations](http://www.alsacreations.com/tuto/lire/622-Securite-firewall-iptables.html) régulièrement.

N'oubliez pas non plus de vous tenir au courant :

 * [oss-security Mailing List](http://oss-security.openwall.org/subscribe) (attention, c'est verbeux)
 * [Informations de sécurité](https://www.debian.org/security/) concernant Debian

La configuration de ces différents utilitaires dépends évidemmment de vos besoins et de votre modèle de menaces. Envoyer des informations par mail ou télécharger des paquets sur un canal non sécurisé peut donner des informations sur l'état de votre système. N'hésitez jamais à utiliser un outil comme [Tor](https://torproject.org/).

Idéalement, les différents processus devraient être lancé automatiquement, régulièrement et envoyer un email immédiatement en cas de problème pour vous alerter.

Cet article vise à donner des idées de pistes à suivre pour la maintenance d'un poste personnel ou d'un serveur, rien de plus.

