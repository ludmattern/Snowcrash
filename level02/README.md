# level02

1. Files found in `~/` :

```bash
./level02.pcap
```

2. File extraction and analysis with **tshark**

An Ubuntu proot is used to avoid altering the system :

```bash
scp -P 2222 level02@127.0.0.1:/home/level02/level02.pcap ./level02.pcap
```

In the proot :

```bash
tshark -r level02.pcap -q -z follow,tcp,ascii,0
```

The `tshark` flow shows keystrokes character by character.

On this `.pcap` the output returns : `ft_wandr...NDRel.L0L` (correct password).

3. Exploitation

```bash
su flag02
ft_waNDReL0L
[...]
getflag
```

4. Token discovered

The token is : `ft_waNDReL0L`
