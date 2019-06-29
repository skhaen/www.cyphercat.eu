+++
author = "Skhaen"
categories = ["gpg", "postfix", "adminsys"]
date = 2014-09-09T16:00:00Z
description = ""
draft = false
slug = "utiliser-caff-avec-postfix"
tags = ["gpg", "postfix", "adminsys"]
title = "Utiliser caff avec postfix"

+++

J'ai eu il y a peu un problème avec <code>caff</code> qui ne voulait plus utiliser <code>exim</code> pour envoyer ses mails. Voici comment faire pour le remplacer par <code>postfix</code>.

#### Configuration de postfix

On commence par installer <code>postfix</code> : 

```
aptitude install postfix
```

On édite le fichier <code>/etc/postfix/main.cf</code> pour rajouter ce qui suit à la fin : 

```
relayhost = [IP_DE_VOTRE_SERVEUR]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_auth
smtp_sasl_security_options = noplaintext, noanonymous
smtp_sasl_tls_security_options = noanonymous
smtp_sasl_mechanism_filter = plain, login
smtp_use_tls = yes
```


Puis on crée le fichier <code>/etc/postfix/sasl_auth</code> avec la ligne suivante :

```
[IP_DE_VOTRE_SERVEUR]:587  smtp@libwalk.so:IeNguco4ae
```

* **IP_DE_VOTRE_SERVEUR** étant l'adresse de votre serveur de messagerie,
* **smtp@libwalk.so** étant une adresse de messagerie existante et utilisable,
* **IeNguco4ae** étant le mot de passe pour accéder à l'adresse mail de la ligne précédente. Pour créer un mot de passe rapidement, vous pouvez utiliser la commande [`pwgen`](https://packages.debian.org/wheezy/admin/pwgen) après l'avoir installé sur votre système (`aptitude install pwgen`) avec par exemple la commande `pwgen 20 1`.

