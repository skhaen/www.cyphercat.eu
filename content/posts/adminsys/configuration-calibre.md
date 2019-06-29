+++
author = "Skhaen"
date = 2015-12-01T17:21:53Z
description = ""
draft = false
slug = "configuration-calibre"
title = "Et si on parlait de Calibre ?"

+++

Bon, après (ne pas) avoir utilisé le logiciel libre Calibre pour gérer ma bibliothèque d'ebooks, j'arrive à un moment où elle commence à être un peu trop grosse pour le faire à la main.

N'attendez pas d'avoir une bibliothèque conséquente avant d'utiliser Calibre, c'est le meilleur moyen de s'en mordre les doigts et d'y passer quelques heures le temps de modifier des métadonnées de livres incohérentes : mieux vaut le faire au fur et à mesure.

Calibre permet de gérer votre bibliothèque de différentes manières : par **auteurs** ([David Weber](https://www.goodreads.com/author/show/10517.David_Weber), [Alain Damasio](https://www.goodreads.com/author/show/660947.Alain_Damasio), [Mercedes Lackey](https://www.goodreads.com/author/show/8685.Mercedes_Lackey), [Anne McCaffrey](https://www.goodreads.com/author/show/26.Anne_McCaffrey)...), **langues**, **séries** ([Black Tides Rising](https://www.goodreads.com/series/110176-black-tide-rising), [Honor Harrington](https://www.goodreads.com/series/40419-honor-harrington), [Fondation](https://www.goodreads.com/series/59386-foundation-publication-order)... <3), **formats** (epub, mobi, djvu...), **éditeurs** ([Baen](http://www.baen.com/)...), **notes** (de 1 à 5) ou encore par **étiquettes** (lu, à lire, SciFi, fantasy, Apocalyptic & Post-Apocalyptic...) que vous pouvez bien entendu créer. 

Autant vous dire que ça permet de trier et de mieux gérer sa bibliothèque. Surtout avec les nombreux [plugins](http://plugins.calibre-ebook.com/) qui existent, dont certains pour [enlever les DRMs](https://benjamin.sonntag.fr/Comment-convertir-vos-fichiers-Kindle-en-ePub-et-enlever).

À ce point là de la lecture, vous vous posez normalement deux questions :

* comment donc puis je faire pour l'installer ?
* est ce que je commence à lire [Black Tides Rising](https://www.goodreads.com/series/110176-black-tide-rising) avant ou après l'installation ?

### Installation

La documentation d'installation pour Linux se trouve sur [calibre-ebook.com/download_linux](http://calibre-ebook.com/download_linux), on peut le faire de plusieurs façon.

###### méthode 1 (défaut)

La méthode la plus simple consiste à installer la version se trouvant dans les [dépôts de Debian](https://packages.debian.org/jessie/calibre) avec la commande suivante :

```
sudo apt-get install calibre
```

**NOTE** : si vous savez comment faire et que c'est possible, n'hésitez pas à installer la version de backports pour avoir une version plus récente.

###### méthode 2

Si vous souhaitez absolument avoir la dernière version, vous pouvez utiliser la commande fournit [directement sur le site](http://calibre-ebook.com/download_linux) dans la partie "*Binary install*" (si vous utilisez cette méthode, vous n'aurez pas d'éventuelles mise à jour de sécurité). 

### Configuration

*  Pour commencer, vous pouvez lancer l'**assistant de bienvenue** dans le menu déroulant des **Préférences** pour vous aider à configurer votre logiciel.
*  Pour **ajouter des ebooks**, il suffit de cliquer sur le menu déroulant de l'icône "*Ajouter des livres*" en haut à gauche de Calibre pour importer vos fichiers (que ce soit livre par livre ou par lots).
* Après avoir importer quelques livres, vous pouvez maintenant **gérer votre bibliothèque** comme vous le souhaitez en modifiant les métadonnées de chaque livre. Pour cela, sélectionner un ou plusieurs livres, faites un clique droit et cliquez sur "*éditer les métadonnées*". Vous pouvez aussi directement en double cliquant (pas trop vite) sur ce que vous voulez éditer.

#### Gérer intelligemment l'export vers sa liseuse

Maintenant que nous avons fini l'introduction, voici le véritable pourquoi de cet article : comment exporter intelligemment ses ebooks sur sa liseuse ? Avec quel tri (alphabétique, auteurs, séries, notes...) ? Et surtout comment s'y retrouver après ?  

Quand votre liseuse est branchée à votre ordinateur, vous pouvez cliquer sur un livre et "*l'envoyer vers le dispositif*" (i.e. la liseuse). Si vous envoyez plusieurs dizaines de livres, provenant de plusieurs auteurs et appartenant à plusieurs séries, retrouver un livre sur votre liseuse peut vite devenir un cauchemar, tout comme [retrouver l'ordre des tomes d'une série](https://www.goodreads.com/series/66507-honorverse).

Calibre vous permet de gérer automatiquement le "tri" des livres lors de l'export sur la liseuse : 

1.  Après avoir branché votre liseuse, cliqué sur le menu déroulant de l'icône "dispositif" puis sur "configurer votre appareil"

![dispositif](/content/images/2015/12/S-lection_119.png)

2. Vous pouvez alors configurer un modèle de tri par défaut pour ce périphérique :

![configurer Odyssey](/content/images/2015/12/Configurer-Odyssey_118.png)

Vous pouvez faire des tests avec les lignes suivantes pour voir ce qui vous convient le plus. N'hésitez pas à partager si vous avez autres choses (les explications sont en dessous) :  

* Par [@mat_a](https://twitter.com/mat_a) : si le livre fait partie d'une série, il crée un sous-dossier au nom de la série avec l'ensemble des livres dedans, sinon il crée un dossier au nom de l'auteur (attention : ne prend pas encore la place du livre dans la série).

```
{series:.1}/{series:ifempty(0/{author_sort:.1}/{author_sort:.30})}/{series_index:0>3.2f|| - }{title} - {authors}
```

* mon actuelle (dossiers a-z / auteurs / séries / livres): 

```
{author_sort:.1}/{author_sort:.30}/{series}/{series_index:0>3.2f|| - }{title}
```

* **author\_sort** : la chaine de tri pour l’auteur. Pour utiliser seulement la première lettre du nom utiliser <code>{author_sort[0]}</code>
* **authors** : les auteurs
* **id** : l’identifiant interne calibre
* **isbn** : l'ISBN (<small><a href="https://en.wikipedia.org/wiki/International_Standard_Book_Number">International Standard Book Number</a></small>)
* **languages** : la/les langue(s) du livre
* **last_modified** : la date à laquelle les métadonnées de l’enregistrement pour ce livre a été modifiée
* **pubdate** : la date de publication
* **publisher** : l’éditeur
* **rating** : son classement
* **series** : les séries
* **series_index** : le numéro de la série. Pour obtenir des zéros avant le numéro, utilisez <code>{series_index:0>3s}</code> ou <code>{series_index:>3s}</code> pour des espaces.
* **tags** : les étiquettes
* **timestamp** : la date
* **title** : le titre
* **[champ]** : le nom de recherche d’un champ personnalisé (ces noms commencent par <code>#</code>)


Si vous voulez que je rajoute des choses, n'hésitez pas à me contacter, je mettrais l'article à jour au fur et à mesure :]

