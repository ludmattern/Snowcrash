# level00

1) Commande utilisée pour trouver le fichier appartenant à `flag00` :

```bash
find / -user flag00 -type f -ls 2>/dev/null
```

2) Résultat notable trouvé :

Fichier : `/usr/sbin/john`
Contenu : `cdiiddwpgswtgt`

3) Décodage :

La chaîne est un chiffrement de César (décalage 11). Décodée elle donne :

```
nottoohardhere
```
