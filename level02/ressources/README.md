### Objectif

Conteneur Ubuntu avec client SSH et sudo pour se connecter à la VM SnowCrash sur le port 4242.

### Prérequis

- Docker et Docker Compose installés sur l'hôte
- Vos clés SSH dans `~/.ssh` (ou un mot de passe si vous préférez l'invite)
- Adresse IP de la VM SnowCrash (ex: 192.168.56.101)

### Build

```bash
cd "$(dirname "$0")"
docker compose build
```

### Lancer un shell dans le conteneur

```bash
docker compose run --rm snowcrash-cli
```

Vous serez connecté en tant que `dev` avec droits sudo (sans mot de passe).

### Connexion SSH à la VM (port 4242)

- Avec clé: `ssh -p 4242 <utilisateur>@<IP_VM>`
- Avec mot de passe: même commande, l'invite demandera le mot de passe

Exemple:

```bash
ssh -p 4242 level02@192.168.56.101
```

Le conteneur utilise `network_mode: host`, il voit donc les mêmes interfaces réseau que l'hôte. Utilisez l'adresse IP de la VM (pas `127.0.0.1`).

### Option: entrée dans `~/.ssh/config`

Ajoutez ceci sur l'hôte pour simplifier la commande:

```
Host snowcrash
  HostName 192.168.56.101
  Port 4242
  User level02
```

Ensuite:

```bash
ssh snowcrash
```

### Dépannage

Dans la VM SnowCrash (via console/tty de la VM):

```bash
# 1) Statut du service SSH
sudo systemctl status ssh --no-pager

# 2) Port SSH configuré (doit être 4242)
sudo grep -E "^Port\s+" /etc/ssh/sshd_config

# 3) Vérifier l'écoute du port 4242
sudo ss -lntp | grep 4242 || sudo netstat -lntp | grep 4242

# 4) Pare-feu
sudo ufw status
sudo ufw allow 4242/tcp  # si nécessaire

# 5) IP de la VM
ip a
```

Depuis le conteneur (ou l'hôte):

```bash
ping -c 3 192.168.56.101
nc -vz 192.168.56.101 4242
ssh -p 4242 level02@192.168.56.101
```

VirtualBox:

- Host-Only: se connecter via l'IP Host-Only (ex: `192.168.56.x`).
- NAT avec redirection: configure `4242 (host) -> 4242 (guest)` et connectez-vous à `127.0.0.1 -p 4242` depuis l'hôte. Depuis le conteneur en réseau hôte, `127.0.0.1:4242` pointe vers l'hôte; utilisez l'IP de l'hôte si nécessaire.

### Notes

- Le dossier du projet de l'hôte `/home/jgavairo/Documents/snowCrash` est monté dans `/workspace`.
- Le montage `~/.ssh` est en lecture seule. Assurez-vous que les permissions de `id_rsa` sont 600.


