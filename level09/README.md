# level09

1. Fichiers trouvés dans `~/` :

```bash
token  level09
```

2) Analyse du fichier `token`

Le fichier `token` contient la chaine suivante `f4kmm6p|=<82>^?p<82>n<83><82>DB<83>Du{^?<8c><89>`.
Le token est obfusqué. 

Chaque octet du fichier a été décalé de son index (on a ajouté i au i-ème caractère). On le devine en executant :
```bash
level09@SnowCrash:~$ ./level09 aaaaaa
abcdef # +0, +1, +2, etc...
level09@SnowCrash:~$ ./level09 111111
123456
```
Un autre indice en faisant : `level09@SnowCrash:~$ strings level09`
```bash
level09@SnowCrash:~$ strings level09
[...]
You should not reverse this #alors nous allons le faire
[...]
```

3) Exploitation pour récupérer le token :

* On applique le traitement inverse pour obtenir le `mdp` de `flag09` :

```bash
level09@SnowCrash:~$ python -c 'import sys; d=bytearray(open("token","rb").read()); sys.stdout.write("".join(chr((b-i)&0xff) for i,b in enumerate(d)))'
f3iji1ju5yuevaus41q1afiuq #mot de passe de flag09
```

4) Token découvert :

Le token est `s5cAJpM8ev6XHw998pRWG728z`.
