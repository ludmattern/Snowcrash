# level09

1. Files found in `~/` :

```bash
token  level09
```

2. Analysis of the `token` file

The `token` file contains the following string `f4kmm6p|=<82>^?p<82>n<83><82>DB<83>Du{^?<8c><89>`.
The token is obfuscated.

Each byte of the file has been shifted by its index (we added i to the i-th character). We guess this by executing :

```bash
level09@SnowCrash:~$ ./level09 aaaaaa
abcdef # +0, +1, +2, etc...
level09@SnowCrash:~$ ./level09 111111
123456
```

Another clue by doing :

```bash
level09@SnowCrash:~$ strings level09
[...]
You should not reverse this #so we will do it
[...]
```

3. Exploitation

Apply the inverse treatment to get the `password` of `flag09` :

```bash
level09@SnowCrash:~$ python -c 'import sys; d=bytearray(open("token","rb").read()); sys.stdout.write("".join(chr((b-i)&0xff) for i,b in enumerate(d)))'
f3iji1ju5yuevaus41q1afiuq #flag09 password
```

```bash
su flag09
f3iji1ju5yuevaus41q1afiuq
getflag
```

4. Token discovered

The token is : `s5cAJpM8ev6XHw998pRWG728z`
