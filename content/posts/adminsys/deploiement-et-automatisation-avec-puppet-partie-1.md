+++
author = "Skhaen"
categories = ["puppet", "adminsys", "ssl"]
date = 2017-02-12T19:28:25Z
description = ""
draft = false
slug = "deploiement-et-automatisation-avec-puppet-partie-1"
tags = ["puppet", "adminsys", "ssl"]
title = "[Puppet] [1] : Déploiement et automatisation avec Puppet 4.9"

+++

Puppet est l'outil de gestion de configuration le plus connu, il a vu le jour en 2005, bien avant ses principaux concurrents (2009 pour [Chef](https://en.wikipedia.org/wiki/Chef_(software)), 2011 pour [Salt](https://en.wikipedia.org/wiki/Salt_(software)), 2012 pour [Ansible](https://en.wikipedia.org/wiki/Ansible_(software))). Il propose depuis maintenant longtemps une solution open-source en parallèle de la version entreprise (qui est évidemment payante) et il a eu le temps de grandir pour atteindre une taille plus que respectable.

Au contraire d'Ansible où ça reste plutôt simple pour le moment, les premières recherches que vous allez faire ont toutes les chances de vous laisser quelque part entre "mais je commence par quoi bordel ?!" et "mais... mais... c'est horriblement compliqué". Parce que oui, évidemment, en plus de 10 ans le logiciel a eu le temps de se développer, sa communauté aussi et quand on parle maintenant de puppet certains termes comme  [control-repo](https://github.com/puppetlabs/control-repo), [R10K](https://github.com/puppetlabs/r10k), [puppetdb](https://docs.puppet.com/puppetdb/), [hiera](https://docs.puppet.com/hiera/), [rspec](https://puppet.com/blog/unit-testing-rspec-puppet-for-beginners), [modules](https://forge.puppet.com/), [librarian-puppet](http://librarian-puppet.com/) reviennent souvent.

Si vous vous posez la question, oui, c'est le bon moment pour se faire un thé (et je veux bien un [rubis birman](https://www.tripadvisor.com/Restaurant_Review-g187147-d1174565-Reviews-Maison_des_trois_thes-Paris_Ile_de_France.html) pendant que l'on y est).

Avant de continuer, n'oubliez pas que :

* il y a une seule chose importante à retenir pour le moment : la documentation est sur [docs.puppet.com](https://docs.puppet.com/puppet/4.8/install_pre.html).
* mon serveur "master" se nomme `master.cyphercat.eu`
* mon serveur "slave1" ou "node1" s'appelle `slave1.cyphercat.eu`

N'oubliez donc pas de changer les noms des serveurs avez les vôtres au fur et à mesure de cet article. **Je vous déconseille BEAUCOUP de tenter le coup avec des configurations dans `/etc/hosts`**, vous avez toutes les chances que ça foutent une pagaille mémorable avec les certificats (je viens de tester chez [scaleway](https://www.scaleway.com/)).

# Installation

Nous allons installer un `puppet-server` (que l'on pourrait appeler serveur maitre), c'est lui qui s'occupera de tout orchestrer. Puis, sur chaque node, on installera `puppet-agent`, ce qui leur permettra de communiquer avec le master.

PuppetLabs met à disposition [puppet-collection](https://docs.puppet.com/puppet/4.9/puppet_collections.html) pour les installations, c'est un [ensemble d'outils](https://puppet.com/blog/welcome-to-puppet-collections) (par exemple facter, hiera, ruby, openssl et mcollective pour puppet-agent) packagés. En utilisant ceci, nous sommes donc sûr d'avoir des logiciels compatibles entre eux.

## MASTER / [puppetserver](https://github.com/puppetlabs/puppetserver)

Vous pouvez voir les détails sur [puppet-server](https://docs.puppet.com/puppetserver/2.7/install_from_packages.html) et sur [puppet_collections](https://docs.puppet.com/puppet/latest/puppet_collections.html), nous allons faire ici une installation la plus simple possible (ah ah) sur une debian 8 (Jessie).

```bash
root@master:# wget https://apt.puppetlabs.com/puppetlabs-release-pc1-jessie.deb
dpkg -i puppetlabs-release-pc1-jessie.deb
root@master:# apt-get update
root@master:# apt-get install puppetserver
```
Puis on démarre le service (si ça plante, allez voir la section "*[memory allocation](https://docs.puppet.com/puppetserver/2.7/install_from_packages.html#memory-allocation)*") :

```bash
root@master:# service puppetserver start
```



**Attention !** `dpkg -l|grep puppet` nous donne les résultats suivants :

```bash
puppet-agent                 1.9.1-1jessie
puppetlabs-release-pc1       1.1.0-4jessie
puppetserver                 2.7.2-1puppetlabs1  
```
On pourrait donc croire que l'on se retrouve avec un puppet v2.7 (on en est à la v4)... mais point du tout : [Puppet Server 2.x supporte Puppet 4, alors que Puppet Server 1.x support Puppet 3.x et supérieur](https://docs.puppet.com/puppetserver/).

## SLAVE / puppet-agent (node)

Vous pouvez voir les détails sur [puppet-agent](https://docs.puppet.com/puppet/4.8/install_linux.html), l'installation se passe aussi sur une Debian 8 (Jessie) :

```bash
root@slave1:# wget https://apt.puppetlabs.com/puppetlabs-release-pc1-jessie.deb
root@slave1:# dpkg -i puppetlabs-release-pc1-jessie.deb
root@slave1:# apt-get update
root@slave1:# apt-get install puppet-agent
```
# Un peu de configuration

Voici une bonne chose de faite. En lisant la doc vous verrez que les [exécutables de puppet](https://docs.puppet.com/puppet/4.8/install_linux.html#confirm-you-can-run-puppet-executables) sont maintenant dans `/opt/puppetlabs/bin/` (ce qui n'est pas dans notre [PATH](https://fr.wikipedia.org/wiki/Variable_d%27environnement#.3CPATH.3E_pour_l.27emplacement_des_ex.C3.A9cutables) par défaut). Pour régler ce problème pour le moment :

```bash
root@slave1:# PATH=/opt/puppetlabs/bin:$PATH
```

## puppet.conf

> Pour les détails sur le fichier `puppet.conf`, voir [config\_file\_main.html](https://docs.puppet.com/puppet/4.9/config_file_main.html)

> * [exemple de configuration pour un agent](https://docs.puppet.com/puppet/4.9/config_file_main.html#example-agent-config)
> * [exemple de configuration pour un master](https://docs.puppet.com/puppet/4.9/config_file_main.html#example-master-config)

Par [défaut](https://docs.puppet.com/puppet/4.8/config_important_settings.html#settings-for-agents-all-nodes), un node recherche son master sur un serveur qui s’appelle "puppet". Nous allons donc ajouter deux lignes pour spécifier notre configuration dans `/etc/puppetlabs/puppet/puppet.conf`sur le slave pour y ajouter ces deux lignes :

* **sur le MASTER**

```bash
[main]
certname = master.cyphercat.eu
server   = puppetmaster


[master]
dns_alt_names = puppet,puppetmaster,master.cyphercat.eu
vardir        = /opt/puppetlabs/server/data/puppetserver
logdir        = /var/log/puppetlabs/puppetserver
rundir        = /var/run/puppetlabs/puppetserver
pidfile       = /var/run/puppetlabs/puppetserver/puppetserver.pid
codedir       = /etc/puppetlabs/code

```

* **sur le SLAVE**

```bash
[main]
certname     = slave1.cyphercat.eu
server       = master.cyphercat.eu
environment  = production
```

# Ajouter un node au master

Vous êtes prêt-e-s ? Il existe [2 méthodes](https://docs.puppet.com/puppet/4.8/install_linux.html#start-the-puppet-service) pour dire à un node de se connecter au master, la première en "tâche de fond", et la seconde "à la main" que l'on peut mettre en [cron](https://fr.wikipedia.org/wiki/Cron).

## sur le SLAVE

Utilisons la commande manuelle pour voir ce qui se passe :

```bash
root@slave1:# /opt/puppetlabs/bin/puppet agent --test

Info: Creating a new SSL key for slave1.cyphercat.eu
Info: Caching certificate for ca
Info: csr_attributes file loading from /etc/puppetlabs/puppet/csr_attributes.yaml
Info: Creating a new SSL certificate request for slave1.cyphercat.eu
Info: Certificate Request fingerprint (SHA256): 55:A1:EE:A6:1D:D2:85:9D:7F:5F:69:72:7E:BF:54:BA:74:BD:7F:8E:43:0D:AC:A1:D1:1B:4A:FF:4A:F6:54:3B
Info: Caching certificate for ca
Exiting; no certificate found and waitforcert is disabled
```

Ce qui veut dire que :

1. slave1 a créé un certificat pour lui et il arrive à se connecter au master
2. le master ne veut pas de lui vu qu'il ne le connaît pas, nous allons donc régler ça.

## Sur le MASTER

```bash
root@master:# /opt/puppetlabs/bin/puppet cert list
  "slave1.cyphercat.eu" (SHA256) 55:A1:EE:A6:1D:D2:85:9D:7F:5F:69:72:7E:BF:54:BA:74:BD:7F:8E:43:0D:AC:A1:D1:1B:4A:FF:4A:F6:54:3B
```

Bonne nouvelle, le premier contact a vraiment eu lieu ! Le MASTER a bien reçu le certificat de SLAVE, et il nous attend pour le [valider](https://docs.puppet.com/puppet/4.8/install_linux.html#sign-certificates-on-the-ca-master) avec la commande suivante :

```bash
/opt/puppetlabs/bin/puppet cert sign slave1.cyphercat.eu

Signing Certificate Request for:
  "slave1.cyphercat.eu" (SHA256) 55:A1:EE:A6:1D:D2:85:9D:7F:5F:69:72:7E:BF:54:BA:74:BD:7F:8E:43:0D:AC:A1:D1:1B:4A:FF:4A:F6:54:3B
Notice: Signed certificate request for slave1.cyphercat.eu
Notice: Removing file Puppet::SSL::CertificateRequest slave1.cyphercat.eu at '/etc/puppetlabs/puppet/ssl/ca/requests/slave1.cyphercat.eu.pem'
```
Et si on relance notre `puppet agent -t` sur le slave :

```bash
root@slave1:# /opt/puppetlabs/bin/puppet agent --test

Info: Caching certificate for slave1.cyphercat.eu
Info: Caching certificate_revocation_list for ca
Info: Caching certificate for slave1.cyphercat.eu
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Caching catalog for slave1.cyphercat.eu
Info: Applying configuration version '1486920585'
Info: Creating state file /opt/puppetlabs/puppet/cache/state/state.yaml

Notice: Applied catalog in 0.07 seconds

```
# Troubleshooting (install)

Vous avez peut être rencontré des problèmes au cours de cette installation, je vous conseille de regarder les points suivants pour vous aider à trouver une solution :

* [dns\_alt\_names](https://docs.puppet.com/puppet/latest/config_important_settings.html#basics-1) : liste de noms que votre **master** peut utiliser? Le nom que vos nodes utilisent (puppet, puppetmaster, master.cyphercat.eu...) **DOIT** etre inclus dedans ainsi que dans son certificat (voir aussi [dnsaltnames](https://docs.puppet.com/puppet/latest/configuration.html#dnsaltnames)),
* [puppet.conf](https://docs.puppet.com/puppet/4.9/config_file_main.html)
* Configuration: [Short list of important settings](https://docs.puppet.com/puppet/4.9/config_important_settings.html)
* [puppet troubleshooting](https://www.cyphercat.eu/puppet-troubleshooting/) ou l'[original](https://docs.google.com/presentation/d/1AT2j97HV_y2QNH_HFKwZ6ExQDKvfC_lt6qBZaai4ATo/edit#slide=id.p) (bon courage)


# Premier manifest

Personnellement, j'aime bien avoir [mes outils](https://www.puppetcookbook.com/posts/install-multiple-packages.html) sur mes serveurs, ainsi que quelques raccourcis, commençons donc par ça :

## sur le MASTER

```bash
cd /etc/puppetlabs/code/environments/production/manifests
```

* Nous créons le fichier `site.pp` pour [déclarer notre node](https://docs.puppet.com/puppet/4.8/lang_node_definitions.html) et ce qu'il devra utiliser :


```bash
# /etc/puppetlabs/puppet/manifests/site.pp
node 'slave1.cyphercat.eu' {
  include common
}
```

* et nous allons faire notre premier manifest pour installer des paquets dans le fichier `common.pp` que nous créons aussi :


```bash
class common {

    $packages_list = [
      'apt-transport-https',
      'bzip2',
      'curl',
      'deborphan',
      'htop',
      'less',
      'lsof',
      'ncdu',
      'pbzip2',
      'pigz',
      'pwgen',
      'rpl',
      'screen',
      'strace',
      'sudo',
      'tar',
      'unzip',
      'vim',
      'wget',
      'whois',
      'zip'
    ]

    package {
      $packages_list:
        ensure => 'installed'
    }
}
```



Puis nous relançons notre commande `/opt/puppetlabs/bin/puppet agent -t` sur notre slave1. Si vous n'avez pas d'erreurs, vous devriez voir quelque comme ceci qui s'affiche :

```
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Caching catalog for slave1.cyphercat.eu
Info: Applying configuration version '1486926845'
Notice: /Stage[main]/Common/Package[deborphan]/ensure: created
Notice: /Stage[main]/Common/Package[ncdu]/ensure: created
Notice: /Stage[main]/Common/Package[pbzip2]/ensure: created
Notice: /Stage[main]/Common/Package[pigz]/ensure: created
Notice: /Stage[main]/Common/Package[pwgen]/ensure: created
Notice: /Stage[main]/Common/Package[rpl]/ensure: created
Notice: /Stage[main]/Common/Package[strace]/ensure: created
Notice: /Stage[main]/Common/Package[unzip]/ensure: created
Notice: /Stage[main]/Common/Package[whois]/ensure: created
Notice: /Stage[main]/Common/Package[zip]/ensure: created
Notice: Applied catalog in 39.43 seconds
```
Et voilà pour aujourd'hui !

À noter que :

* Vous pouvez utiliser la commande `puppet parser validate common.pp` si vous voulez voir si vous avez fait des erreurs de syntaxe.
* Vous pouvez utiliser [puppet-lint](http://puppet-lint.com/) pour voir si vous avez fait des erreurs de style.

