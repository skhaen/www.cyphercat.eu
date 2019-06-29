+++
author = "Skhaen"
date = 2017-01-20T19:37:30Z
description = ""
draft = false
tags = ["adminsys"]
slug = "comment-se-debarrasser-de-son-ecran-fantome"
title = "Comment se débarrasser de son écran fantôme"

+++

Note à moi même pour ne pas perdre une heure à cause de cette saloperie si ça recommence : 

Pour une Debian / Ubuntu et un #### d'écran VGA fantôme :

Éditer  `/etc/default/grub` pour y ajouter `video=VGA-1:d` entre les quotes de   `GRUB_CMDLINE_LINUX`.

```
GRUB_CMDLINE_LINUX="video=VGA-1:d"
```

Si vous avez un doute, se référer à `xrandr -q` ou à `ls /sys/class/drm` pour la liste de vos périphériques. Exemple sur mon ordi : 

```
ls /sys/class/drm -1
   card0
   card0-DP-1
   card0-eDP-1
   card0-HDMI-A-1
   card0-VGA-1
   controlD64
   renderD128
```
On retrouve donc cette $$$##$$ de VGA fantôme (`card0-VGA-1`), on enlève le début, on garde la partie qui nous intéresse (`VGA-1`) et on sait quoi rajouter à sa config. 

Pour finir :

```
sudo update-grub
sudo reboot
```

