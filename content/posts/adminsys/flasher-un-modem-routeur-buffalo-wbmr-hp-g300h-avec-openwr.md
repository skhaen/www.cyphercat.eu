+++
author = "Skhaen"
categories = ["adminsys", "openwrt"]
date = 2014-03-04T17:58:00Z
description = ""
draft = false
slug = "flasher-un-modem-routeur-buffalo-wbmr-hp-g300h-avec-openwr"
tags = ["adminsys", "openwrt"]
title = "Flasher un modem routeur buffalo WBMR HP g300h avec OpenWRT"

+++

Petit tuto pour flasher un Buffalo WBMR-HP-G300H sur OpenWRT 12.09 - Attitude Adjustment.

* EDIT 2014-05-24 : rajout des 2 liens pour régler le bug du switch (tout à la fin de cet article)



Pour cette recette, il vous faudra :

* un **Buffalo WBMR-HP-G300H** (disponible par exemple sur [monclick.fr](http://www.monclick.fr/product.aspx?codice=WBMR-HP-G300H) pour 72 € ou chez [Amazon](http://www.amazon.fr/Buffalo-AirStation-Nfiniti-HighPower-Broadband/dp/B005584W2E/ref=sr_1_1?ie=UTF8&qid=1327222358&sr=8-1#productDetails) à partir de ~80 €),
* un ordinateur avec un [OS acceptable](http://debian.org/),
* une connexion [Internet qui fonctionne](http://www.ffdn.org/),
* de la (bonne) vodka, du gin, de l'amaretto, du triple sec, du schnapps, ... ne vous en faites pas, vous aurez la liste complète un peu plus loin son utilisation aussi.
* un switch ainsi que les câbles ethernet qui vont avec.

### Au commencement était le TFTP

On part du [wiki de OpenWRT](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#how.to.recover.from.bricking) avec les explications qui vont bien avec :
 
Commencons par installer le serveur TFTP, sur Debian ça donne ceci :

```
aptitude update && aptitude install tftpd-hpa
```

On télécharge l'image (*ramdisk*) d'openWRT qui va bien (vous pouvez le compiler vous même si le coeur vous en dit) :

```
wget http://wiki.openwrt.org/_media/toh/buffalo/openwrt-lantiq-ar9-wbmr-uimage.tar00.zip && wget http://wiki.openwrt.org/_media/toh/buffalo/openwrt-lantiq-ar9-wbmr-uimage.tar01.zip
```

On réassemble le ramdisk (ce qui nous donne **openwrt-lantiq-ar9-WBMR-uImage**)

```
cat openwrt-lantiq-ar9-wbmr-uimage.tar* | tar xzpvf -
```
    
On le renomme en **firmware.ram**, puis on le déplace dans **/srv/tftp** qui est normalement le répertoire par défaut pour le TFTP (vous pouvez le vérifier dans */etc/default/tftpd-hpa*).

```
mv openwrt-lantiq-ar9-WBMR-uImage firmware.ram
mv firmware.ram /srv/tftp/
```

Il faut que notre ordinateur (où se situe le TFTP) possède l'IP **192.168.11.2** car le routeur s'y connecte par défaut dans ce genre de situation, si vous le faite de manière graphique, ça donne quelque chose comme ceci :

![ip_tftp_openwrt](/images/2015/09/ip_tftp_openwrt.png)

**flashopenwrt** est ici le nom de ma configuration avec cette adresse, vous pouvez mettre ce que vous voulez.

## Et maintenant, le flash !

Non, je ne ferais pas la blague sur le héros de comics.

**IMPORTANT** : en suivant [cette procédure](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#how.to.recover.from.bricking), nous allons commencer par charger une image en **RAM**, puis nous allons « écrire » cette image. 

Si vous redémarrez le routeur avant la fin de la seconde étape, il faudra donc recommencer à partir d'ici.


### Étape 1 - [charger une image en ram(disk)](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#how.to.recover.from.bricking)
#### [facultatif]
Histoire de vérifier que le routeur tente bien d'avoir le firmware, nous allons installer [wireshark](https://www.wireshark.org/) :

```
aptitude install wireshark
sudo wireshark
```

Après son ouverture, il faut cliquer sur la première icône de la barre des tâches, son étiquette est « *list the available capture interfaces* », une fenêtre s'ouvre, on sélectionne la ligne avec l'ip 192.168.1.2 (sans doute *eth0*), puis on clique sur *start*.

Vous pouvez alors voir tout ce qui passe sur ce port, écrivez *tftp* dans « *filtrer* » en haut à gauche (puis entrée), ça permettra de filtrer les informations :

![selection tftp wireshark](/images/2015/09/selection_tftp_wireshark.png)

#### [/facultatif]

Reprenons.

On débranche électriquement le routeur, on appuie sur le bouton AOSS (au dessus des LEDs, c'est marqué à côté), puis on rebranche en le gardant appuyé.

 Vous devriez alors voir quelque chose comme ce qui suit sur votre wireshark :
 
![wireshark tftp](/images/2015/09/wireshark_tftp-1.png)

Si c'est le cas : Bravo ! vous ne vous êtes pas foiré ! :D

C'est un peu compliqué de savoir quand c'est « bon » car les LEDs ne fonctionnent plus (c'est normal, il faut les configurer par la suite), un moyen de savoir quand c'est bon et de pinguer le routeur :

```
ping 192.168.1.1
```
    
Quand il commence à répondre, c'est que c'est fini ;-)

### Première connexion en [telnet/ssh](http://wiki.openwrt.org/doc/howto/firstlogin)

```
telnet 192.168.1.1
```
    
Vous avez maintenant le reste de la liste, ainsi que le pourquoi (ne buvez pas trop, on est encore loin de la fin ;-))

Vous allez alors voir apparaitre :
    
```
Trying 192.168.1.1...
Connected to 192.168.1.1.
Escape character is '^]'.
=== IMPORTANT ============================
Use 'passwd' to set your login password
this will disable telnet and enable SSH
```

On met en place un mot de passe :

```
passwd
```
    
Le telnet est maintenant désactivé et il faut utiliser ssh pour se connecter.

### Étape 2 - persistance, je crie ton nom !

On télécharge l'image que l'on veut écrire « en dur » sur le routeur (attention, ce n'est pas le même fichier que le précédent) : 

```
wget http://downloads.openwrt.org/attitude_adjustment/12.09/lantiq/ar9/openwrt-lantiq-ar9-WBMR-squashfs.image
```

On transfère le fichier sur le routeur :
    
```
scp openwrt-lantiq-ar9-WBMR-squashfs.image 192.168.1.1:/tmp/
```

Puis à partir du routeur (il faut donc s'y connecter avant) :

```
sysupgrade -v /tmp/openwrt-lantiq-ar9-WBMR-squashfs.image
```
    
Et voilà !

**Note** : il peut être nécessaire de redémarrer électriquement le routeur **après** pour que ce soit pris en compte.

### Et maintenant, configurons !

Lors de la réinstallation, openssh regénère des nouvelles clés, il est donc parfaitement normal d'avoir ce message lors de sa connexion :

```
skhaen@tech:~$ ssh root@192.168.1.1
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    
Add correct host key in /home/skhaen/.ssh/known_hosts to get rid of this message.
Offending RSA key in /home/skhaen/.ssh/known_hosts:755
    
Host key verification failed.
```

Il faut alors éditer son fichier **/home/skhaen/.ssh/known_hosts** et effacer la ligne correspondante. Dans mon cas, c'est la ligne numéro 755, je peux donc faire :

```
vim /home/skhaen/.ssh/known_hosts +755
```

* pour effacer une ligne sous vim : **dd**
* pour sauvegarder et quitter : **:wq**
* pour [apprendre le reste](http://formation-debian.via.ecp.fr/vim.html).

## WE NEED INTERNET!

C'est le bon moment pour brancher votre routeur à Internet d'une facon ou d'une autre (sauf avec une tronçonneuse), nous allons avoir besoin d'une connexion pour installer des paquets. Il suffit de la brancher en ethernet sur votre box, sur un switch... 

Un test rapide pour savoir si c'est bon, c'est de lancer la commande **opkg update** sur le routeur, si ça marche, c'est que vous pouvez continuer.

### Du réseauuuuuuuu /o/

Voici un exemple de configuration à mettre dans **/etc/config/network** (pour une ligne [FDN](http://www.fdn.fr/) en [PPPoE](https://en.wikipedia.org/wiki/Point-to-point_protocol_over_Ethernet))

```
config interface 'loopback'
   option ifname 'lo'
   option proto 'static'
   option ipaddr '127.0.0.1'
   option netmask '255.0.0.0'
    
config interface 'lan'
   option ifname 'eth0'
   option proto 'static'
   option ipaddr '192.168.1.254'
   option netmask '255.255.255.0'
    
config adsl-device 'adsl'
   option fwannex 'a'
   option annex 'a2p'
    
config atm-bridge 'atm'
   option unit '0'
   option vpi '8'
   option vci '35'
   option encaps 'llc'
   option payload 'bridged'
    
config interface 'wan'
   option ifname 'nas0'
   option _orig_bridge 'false'
   option encaps 'llc'
   option proto 'pppoe'
   option username 'skhaen@fdn.nerim'
   option password 'azerty123456'
   option vpi '8'
   option vci '35'
   option atmdev '0'
```            
### l'[ADSL](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#turn.on.adsl.nas0.interface)

On vérifie quelle version du firmware ADSL est installé :

```
opkg list-installed | grep kmod-ltq-dsl-firmware
```

Si le résultat est *kmod-ltq-dsl-firmware-**b**-ar9*, c'est pas de bol, ce n'est pas le bon, il faut donc l'enlever et installer le bon ([annex "a" pour la France](https://en.wikipedia.org/wiki/G.992.3)) :

```
opkg remove kmod-ltq-dsl-firmware-b-ar9
opkg install kmod-ltq-dsl-firmware-a-ar9
```

Ensuite il faut juste s'assurer que le module *br2684ctl* est activé :

```
/etc/init.d/dsl_control start
/etc/init.d/br2684ctl enable
/etc/init.d/br2684ctl start
```

on vérifie l'état de l'ADSL : 

```
/etc/init.d/dsl_control status
```
    
ce qui donne (si ça marche) quelque chose comme :

```
Chipset:			Ifx-AR9 1.2
Line State:			UP [0x801: showtime_tc_sync]
Data Rate:			14.807 Mb/s / 962 Kb/s
Line Attenuation:	21.3dB / 10.6dB
Noise Margin:		10.0dB / 10.6dB
Line Uptime:		0h 2m 24s
```

et si ça ne marche pas :

```
Chipset:                Ifx-AR9 1.2
Line State:             DOWN [0x200: silent]
Data Rate:              0 b/s / 0 b/s
Line Attenuation:       0.0dB / 0.0dB
Noise Margin:           0.0dB / 0.0dB
Line Uptime:            down
```

Si vous avez la chance d'être dans une association de la [Fédération FDN](http://ffdn.org/), vous pouvez essayer de demander à un adminsys de jeter un coup d'oeil à vos logs de connexions (côté FAI), vous pouvez ainsi mieux définir le (ou l'absence de) problème.

### et le [wifi](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#configure.wifi)

On installe le paquet nécessaire :
    
```
opkg install kmod-ath9k
```
    
Puis on rajoute la ligne qu'il faut dans le bon fichier :
    
```
wifi detect >> /etc/config/wireless
```

### Rallumons les Lumières 

Nous allons [configurer les LEDs](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#leds) tout de suite, ça peut être pratique pour la suite. L'exemple de configuration suivant doit être mis dans  **/etc/config/system**.

```
config led
    option default '0'
    option name 'power'
    option sysfs 'soc:green:power'
    option trigger 'default-on'
    
config led
    option default '0'
    option name 'power2'
    option sysfs 'soc:red:power'
    option trigger 'none'
    
config led
    option default '0'
    option name 'wifi'
    option sysfs 'soc:green:wlan'
    option trigger 'phy0tpt'
    
config led
    option default '0'
    option name 'security'
    option sysfs 'soc:red:security'
    option trigger 'phy0tpt'
    
config led
    option default '0'
    option name 'dsl'
    option sysfs 'soc:green:adsl'
    option trigger 'netdev'
    option dev 'nas0'
    option mode 'link tx rx'
    
config led
    option default '0'
    option name 'online'
    option sysfs 'soc:green:internet'
    option trigger 'none'
    
config led
    option default '0'
    option name 'online2'
    option sysfs 'soc:red:internet'
    option trigger 'netdev'
    option dev 'nas0'
    option mode 'tx rx'
    
config led
    option default '0'
    option name 'usb'
    option sysfs 'soc:green:usb'
    option trigger 'default-on'
    
config led
    option default '0'
    option name 'movie'
    option sysfs 'soc:blue:movie'
    option trigger 'timer'
    option delayon '1000'
    option delayoff '1000'
```

Il peut être nécessaire de redémarrer le routeur pour que ce soit pris en compte.

### MOAR


* La [page dediée](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h) sur le site d'OpenWRT.
* si vous avez un bug sur le switch (les machines ne peuvent pas communiquer entre elle, voir [ici](http://wiki.openwrt.org/toh/buffalo/wbmr-hp-g300h#wired.stations.cannot.ping.each.other) et [là](https://forum.openwrt.org/viewtopic.php?pid=184969#p184969).

