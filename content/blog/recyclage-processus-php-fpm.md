---
title: "Recyclage des processus php-fpm"
date: 2026-05-28T17:36:00+01:00
draft: false
---

Bon ! Voilà ! Je suis de retour.
Je ne vais pas la blâmer et dire que c'était sa faute, mais je crois qu'on a enfin pu sortir de cette période tumultueuse du bébé qui démarre la crèche en pleine période hivernale et... qui tombe malade toutes les semaines et... le papa avec :) (non, j'avoue, cette fois ci je l'étais moins qu'avec le premier, et c'est un win pour moi !)

Alors, après cette belle intro qui n'a rien a voir avec le sujet : parlons du recyclage des processus php-fpm, qui, en soit, n'est pas compliqué, mais je crois assez méconnu. Et des fois, des sujets comme ça, ne devrait pas l'être !

Dernièrement, dans mon boulot, j'ai a nouveau dû revoir comment ça fonctionne, car je l'avais vu au moment de configurer mes pools, puis comme à chaque fois, une fois qu'on a fait, on oublie !

Alors, pourquoi on recycle un processus php-fpm ? 
Et bien, avec le temps, le processus peut avoir des fuites de mémoires (car des variables static restent entre les cycles) ou simplement avoir des données en cache (également des variables statiques, par exemple). Un processus traitant des centaines voir des milliers de requêtes va donc, potentiellement, empiler tout ça.

Le principal moyen de forcer un recyclage de processus est d'indiquer `max_requests` dans la configuration du pool.
Ce paramètre va permettre d'indiquer au service que, après 500 cycles, on kill le processus et on en refait un autre.
Via cette commande, le processus va finir son dernier cycle, tuer la processus et le remplacer.

Il existe néanmoins un autre paramètre, plus brutal, qui est `request_terminate_timeout`, celui ci, qui prends des valeurs comme `15s` pour 15secondes, va lancer un `SIGKILL` sur le processus, si celui ci dépasse le temps alloué au processus.

Et voilà, that's it !
On reprend tranquillement l'édition d'articles.
