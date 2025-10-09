# level00

## 1. Commande utilisée pour trouver le fichier appartenant à `flag00` :

```bash
find / -user flag00 -type f -ls 2>/dev/null
```

- Nous voyons donc un fichier appartenant a `flag00`

``/usr/sbin/john``

-   Contenant `cdiiddwpgswtgt`

## 2. Decodage : 

La chaîne est un chiffrement de César (décalage 11).


## 3. Password
Le password est `nottoohardhere`

## 4. Token

```bash
su flag00
nottoohardhere
[...]
getflag
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```

Le password est `x24ti5gi3x0ol2eh4esiuxias`