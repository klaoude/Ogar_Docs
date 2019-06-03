# Ogar_Docs
Documentation for Ogar

Pour lancer le serveur :

```shell
$ cd newogar/Ogar-master/src
$ nodejs index.js
```

#### index.js ou ./index.js ?

Vous n'avez pas besoin d'écrire ./index.js (mais vous pouvez le faire) car on donne le fichier "index.js" en paramètre à la fonction nodejs. Elle part du principe que le fichier est dans le répertoire courant.

A l'inverse, quand vous voulez exécuter un exécutable (comme visualise par exemple) là il faut écrire "./executable" pour que l'interpréteur comprenne qu'on parle d'un fichier du répertoire courant et non d'un exécutable installé ailleurs sur la machine.

La différence : le fichier est index.js est interprété par nodejs, où nodejs est exécuté par la machine (ce n'est donc pas index.js qui est exécuté). Le fichier visualise, lui, est exécuté par la machine.


Ctrl+c * 2 pour arrêter le serveur

Dans le répertoire home
```shell
$ ./visualise -p 1443 localhost
```
