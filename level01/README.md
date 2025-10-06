# level01

1) Information trouvée dans `/etc/passwd` :

```bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

Le champ `42hDRfypTqqnw` correspond à un ancien hash de mot de passe chiffré au format **DES crypt(3)**.

2) Extraction et test du hash avec **John the Ripper** :

Un conteneur Docker Ubuntu 22.04 est utilisé pour éviter d’altérer le système :

```bash
docker run -it --rm ubuntu:22.04 bash -c "apt-get update && apt-get install -y john && bash"
```

Dans le conteneur :

```bash
echo "42hDRfypTqqnw" > hash.txt
john --format=descrypt hash.txt
john --show hash.txt
```
3) Mot de passe découvert :

```
abcdefg
```
