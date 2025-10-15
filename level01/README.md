# level01

1. Files found in `~/` : none

- Information found in `/etc/passwd` or with `getent passwd` :

```bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

The field `42hDRfypTqqnw` corresponds to an old password hash encrypted in **DES crypt(3)** format.

2. Hash extraction and testing with **John the Ripper**

A Docker Ubuntu 22.04 container is used to avoid altering the system :

```bash
docker run -it --rm ubuntu:22.04 bash -c "apt-get update && apt-get install -y john && bash"
```

In the container :

```bash
echo "42hDRfypTqqnw" > hash.txt
john --format=descrypt hash.txt
john --show hash.txt
```

3. Exploitation

```bash
su flag01
abcdefg
[...]
getflag
```

4. Token discovered

The token is : `abcdefg`
