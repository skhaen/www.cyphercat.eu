+++
author = "Skhaen"
categories = ["adminsys", "ansible"]
date = 2017-01-01T18:04:58Z
description = ""
draft = false
slug = "deploiement-et-automatisation-avec-ansible-partie-1"
tags = ["adminsys", "ansible"]
title = "Déploiement et automatisation avec Ansible - partie 1"

+++

Au programme de cette année : l'automatisation ! Il existe plusieurs outils connus pour ça, vous en avez sans aucun doute entendu parler si vous êtes adminsys : [Puppet](https://puppet.com), [Chef](https://www.chef.io/chef/), [Salt](https://saltstack.com/) et le petit dernier : [Ansible](https://www.ansible.com/). 

Ansible a la réputation d'être le plus "accessible" avec une courbe d'apprentissage assez basse. Il peut être pertinent de l'utiliser à partir d'un seul serveur pour faciliter des déploiements selon les besoins (configuration des outils basique, serveur web, BDD...).

Au contraire des trois autres, il n'utilise pas d'agents (*[agentless](https://www.ansible.com/benefits-of-agentless-architecture)*), c'est à dire que rien n'a besoin de tourner côté serveur pour le faire fonctionner, ce qui le rend *de facto* plus facile à mettre en place. **Il requiert seulement un accès SSH et python sur les serveur**.

Toujours concernant Ansible (bien entendu), on parle souvent d'**[idempotance](https://fr.wikipedia.org/wiki/Idempotence)**, ce qui signifie qu'une opération a le même effet qu'on l'applique une ou plusieurs fois.

Les recettes utilisent du [YAML](https://docs.ansible.com/ansible/YAMLSyntax.html) et [Jinja2](https://docs.ansible.com/ansible/playbooks_filters.html) pour les templates. L'utilisation de YAML permet d'avoir des recettes normalement facile à lire et à comprendre. À noter aussi que grâce au (ou à cause du) YAML, l'indentation est primordiale et source de bug si elle n'est pas respecté comme on le verra plus tard.

Au programme aujourd'hui :

* installation d'Ansible (**v2.2.0.0**)
* tests de connexions aux serveurs
* installation de quelques paquets
* debug et tests

## Installation

Si vous avez besoin d'autre chose, ça devrait être dans [la documentation](https://docs.ansible.com/ansible/intro_installation.html).

### sur [Debian](https://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-debian)

Vous pouvez l'installer directement via les dépôts, mais ce sont évidemment des versions anciennes (cf. [packages.debian.org](https://packages.debian.org/search?keywords=ansible)), pour avoir une version plus récente, ajouter la ligne suivante à `/etc/apt/sources.list.d/ansible.list` :

```
deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main
```

Puis il suffira de lancer les commandes suivantes :

```
sudo apt-get update
sudo apt-get install ansible
```
 
### sur[ Ubuntu](https://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu) 

```
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

Il suffit ensuite d'aller dans `/etc/ansible/` pour la suite :

```
cd /etc/ansible/
```

nous avons normalement 2 fichiers et un dossier :

* **[ansible.cfg](https://docs.ansible.com/ansible/intro_configuration.html)** : la configuration d'ansible, celle par défaut nous convient parfaitement bien pour le moment, mais ça peut valoir le coup d'y jeter un coup d'oeil pour voir ce que l'on peut faire.
* **[hosts](https://docs.ansible.com/ansible/intro_inventory.html)** : c'est dans ce fichier que nous allons indiquer nos serveurs, étape obligatoire pour la suite.
* **[roles](https://docs.ansible.com/ansible/playbooks_roles.html)** : ce dossier nous servira surtout par la suite quand on commencera à avoir plusieurs rôles à nos *[playbooks](https://docs.ansible.com/ansible/playbooks_roles.html)*.

## Inventory / hosts

Dans `/etc/ansible/`, donc, éditons notre fichier `hosts`. Nous pouvons y déclarer nos serveurs individuellement ou par groupe, par exemple :

```
serveur1.exemple.org
serveur2.exemple.org
preprod.elysee.org
8.253.9.125

[web]
skhn-web-nginx-01
skhn-web-nginx-02

[seedbox]
skhn-seedbox-01

```

Voici ma configuration actuelle, à noter que j'ai modifié mon `/etc/hosts` et mon `.ssh/config` pour que ça pointe vers les bons serveurs **avec les bonnes clés SSH** :

```
[scaleway]
skhn-001
skhn-002

[seedbox]
sb
```

Par la suite, je pourrai ainsi lancer des tâches sur tout les serveurs avec l'option générique `all` ou par groupe selon vos besoins (web, bdd, wordpress, etc...). Si vous vous posez la question, le premier groupe s'appelle [scaleway](https://www.scaleway.com/) car j'ai pris 2 serveurs chez eux pour tester Ansible ^^).

### ping

* Avant toute chose, est ce que l'on arrive bien à joindre les serveurs :

```yaml
ansible -m ping all --one-line
```
Ce qui me donne :

```
skhn-002 | SUCCESS => {"changed": false, "ping": "pong"}
skhn-001 | SUCCESS => {"changed": false, "ping": "pong"}
sb | SUCCESS => {"changed": false, "ping": "pong"}
```

### test SSH

Mais est ce que l'on arrive à se connecter au groupe `scaleway` ? On va en profiter pour récupérer quelques infos :

```
ansible scaleway -m setup --tree /tmp/facts_servers/
```
La sortie de cette commande sera recopiée pour chaque serveur dans `/tmp/facts_servers/`, vous devriez avoir toutes les informations que vous souhaitez avoir sur vos serveurs ;-)


### et mon uptime ?

```
ansible all -m command -u root --args "uptime" --one-line

```
résultat :

```
sb | SUCCESS | rc=0 | (stdout)  17:33:14 up 410 days, 21:59,  1 user,  load average: 0.21, 0.16, 0.14
skhn-001 | SUCCESS | rc=0 | (stdout)  16:33:14 up 1 day,  2:05,  1 user,  load average: 0.00, 0.00, 0.00
skhn-002 | SUCCESS | rc=0 | (stdout)  16:33:14 up 1 day,  2:05,  1 user,  load average: 0.00, 0.00, 0.00

```
## Installation d'une liste de paquets

J'aime bien avoir certains paquets installé sur tout mes serveurs, Ansible me permet de l'automatiser facilement. On commence par éditer notre nouveau fichier `/etc/ansible/roles/skhn_common.yml` pour y mettre le bloc suivant :


```yaml
---
- hosts: all
  remote_user: root

  tasks:
  - name: install common packages for all servers
    apt: 
      update_cache=yes
      state=latest
      name={{item}}
    with_items: 
    - curl
    - htop
    - ncdu
    - pwgen
    - strace
    - sudo
    - tar
    - unzip
    - vim
    - wget
    - whois
    - screen
```

Après l'avoir enregistré, il suffira de lancer la commande suivante pour installer cette liste de paquets sur tout les serveurs :

```
ansible-playbook -i hosts /etc/ansible/roles/skhn_common.yml
```

Et c'était donc notre premier rôle, yay \o/

Vous avez peut être remarqué 2/3 trucs :

* un rôle commence toujours par `---`,
* `hosts: all` est ici directement dans le fichier et non dans la commande,
* on nomme ensuite nos tâches (`tasks`) avec `name`, ça permet de s'y retrouver plus facilement et d'avoir un libellé humainement compréhensible à lire pendant le déroulement du rôle (on sait donc plus facilement où on en est et ce qui est en train de se passer).
* `update_cache=yes` permet de faire un `apt-get update`
* j'ai choisi de mettre `state=latest` et non `state=present`, j'ai envie que ce genre de paquet soit à jour sans que j'ai besoin de m'en occuper.

## Debug

Le plus gros problème que j'ai eu pour le moment concerne l'indentation qui est vicieuse et la syntaxe YAML.

### L'indentation

Si vous voulez vous éviter des prises de têtes qui vous font perdre quelques heures facilement, **faites des indentations de DEUX espaces** ! Pourquoi ? Parce que :

* ça, c'est **VALIDE** (DEUX espaces) :

```
---
- hosts: all
  remote_user: root

  tasks:
  - name: install common packages for all servers
    apt: 
      update_cache=yes
      state=latest
      name={{item}} 
    with_items: 
    - curl
    - htop
```

* ça, c'est **INVALIDE** (QUATRE espaces) :

```
---
- hosts: all
    remote_user: root

    tasks:
    - name: install common packages for all servers
      apt: 
        update_cache=yes
        state=latest
        name={{item}} 
      with_items: 
      - curl
      - htop
```

* et ça, c'est **VALIDE** (QUATRE espaces) :

```
---
-   hosts: all
    remote_user: root

    tasks:
    - name: install common packages for all servers
      apt: 
        update_cache=yes
        state=latest
        name={{item}} 
      with_items: 
      - curl
      - htop
```
VOUS LA VOYEZ LA DIFFÉRENCE ? C'EST CE #### D'ESPACEMENT ET D'ALIGNEMENT AVEC LE RESTE DE `hosts: all` ! Bon, une fois qu'on le sait, pourquoi pas, mais à cause de ça j'ai perdu plus d'une heure hier après-midi, merci le message `ERROR! Syntax Error while loading YAML.` absolument pas parlant...

### La syntaxe YAML

Si vous avez un doute sur ce que vous avez fait, ou si vos collègues sont des adeptes de la fameuse méthode "gruiiiiiik et je me casse en vacances sans vérifier si ça marche en prod", l'utilisation de [yamllint](https://yamllint.readthedocs.io/en/latest/) est recommandée (ça, et le non moins fameux [coup-de-clavier-dans-la-gueule](https://www.youtube.com/watch?v=7mmuQteX1qA)). Idéal en pré-commit (yamllint, pas le clavier).

Ansible propose aussi un check : `--syntax-check`, que l'on peut utiliser de cette manière là : 

```
ansible-playbook  --syntax-check example-playbook.yml
ansible-playbook  --syntax-check roles/*
```

Oh, et évidemment, je vous conseille beaucoup de lire les **[best practices](https://docs.ansible.com/ansible/playbooks_best_practices.html)**.

