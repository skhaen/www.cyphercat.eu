+++
author = "Skhaen"
date = 2017-05-20T12:02:04Z
description = ""
draft = false
slug = "puppet-partie-2"
title = "[Puppet] [2] : run puppet et cron"

+++

# Lancer un run puppet avec cron

Lancement automatique du run puppet sur les nodes avec [cron](https://docs.puppet.com/puppet/latest/types/cron.html) toutes les heures :

> **note** : chaque tache cron ajouté nativement par puppet sera mis dans `crontab`, vous pouvez les **l**ister  avec `crontab -l` sur vos nodes. 


```yaml
  cron { "puppet":
    ensure  => present,
    command => 'puppet agent -t --http_read_timeout 2m --logdest /var/log/puppetlabs/puppet/puppet`/bin/date +\\%Y\\%m\\%d\\%H\\%M`.log 1>/dev/null 2>&1',
    user    => 'root',
    minute  => fqdn_rand(60),
    require => File['/var/log/puppet'],
  }
```

Si vous souhaites voir le détail des options, c'est sur [configuration.html](https://docs.puppet.com/puppet/latest/configuration.html#httpreadtimeout). Le `$fqdn_rand(60)` (cf. [#fqdnrand](https://docs.puppet.com/puppet/latest/function.html#fqdnrand), permet d'indiquer aléatoirement une valeur de 0 à 60 (la valeur max étant non inclus selon la documentation) pour la configuration du cron.

Cette méthode permet d'éviter d'avoir tout les runs qui se lancent au meme moment, ce qui pourrait surchager le master.


> **ATTENTION** : Si vous des anciennes versions de Puppet sur vos nodes (2.7 sur Debian wheezy sans les backports par exemple), certaines options peuvent ne pas passer : n'hésitez pas à vérifier ce qui est valide/obsolète et à tester. 




* On n'oublie pas de vérifier que le [dossier](https://docs.puppet.com/puppet/latest/type.html#file) `/var/log/puppet` est bien présent (les [logs, c'est la vie](https://docs.puppet.com/puppet/4.8/services_agent_unix.html#logging)) :


```
  file { "/var/log/puppetlabs/puppet":	
    ensure => directory,
    owner  => puppet, 
    group  => puppet,
    mode   => '0750',
  }
```

* on ajoute une tache planifié pour supprimer automatiquement les logs après 3 jours toutes les nuits à 1h :

```
cron { "remove-puppet-log":0
  ensure  => present,
  command => "find /var/log/puppetlabs/puppet/ -type f -iname \"puppetd*log\" -mtime +3 -delete",
  user    => 'root',
  hour    => 1,
  minute  => fqdn_rand(60);
    }
```

* puis on vérifie bien que le `service` puppet est arreté lors de chaque run (on souhaite une gestion des run via cron, et non via le daemon) :


```
  service { puppet:
    ensure    => stopped, 
    hasstatus => true
  }

```

