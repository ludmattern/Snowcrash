# level08

1) Fichier binaire `level08` (qui nécessite un paramètre) ainsi qu'un fichier `token` à la racine, nous avons donc essayé de donner le fichier token à l'exécutable :

```bash
./level08 token
You may not access token
```

2) Analyse du binaire :

```bash
strings level08
```

- plusieurs lignes retiennent notre attention :

```bash
__stack_chk_fail
printf
strstr
read
open

%s [file to read]
token
You may not access '%s'
Unable to open %s
Unable to read fd %d
```

- Nous pouvons en déduire qu'il y a un check fail, `__stack_chk_fail`, et qu'il y a de grandes chances que le check soit fait en comparant deux strings avec `strstr`. Nous pouvons voir ensuite que deux chaînes existent dans les logs donc `%s` et `token`. Nous en déduisons donc que le fichier passé en paramètre ne doit pas se nommer `token`.


3) Exploitation : Nous ne possédons aucun droit sur `token`, donc impossible de renommer, ni de copier. Nous avons donc décidé de créer un raccourci vers `token` nommé `lecteur` dans /tmp/

```bash
ln -s /home/user/level08/token /tmp/lecteur
```

4) Exécution : lancer `./level08` avec en paramètre `/tmp/lecteur`, la chaîne comparée ne sera alors pas `token`.

```bash
./level08 /tmp/lecteur
```

5) Password découvert :

- Le password est `quif5eloekouj29ke0vouxean`.

6) On se connecte a flag08

```bash
su flag08
quif5eloekouj29ke0vouxean
```

```bash
getflag
25749xKZ8L7DkSCwJkT9dyv6f
```
