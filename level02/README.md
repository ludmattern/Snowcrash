# level02

1) Information trouvée dans `/home/level02/` :

```bash
./level02.pcap
```

2) Extraction et analyse du fichier avec **tshark**

Un proot Ubuntu est utilisé pour éviter d’altérer le système :

```bash
scp -P 2222 level02@127.0.0.1:/home/level02/level02.pcap ./level02.pcap
```

Dans le proot :


```bash
tshark -r level02.pcap -q -z follow,tcp,ascii,0
```

Le flux `tshark` montre les frappes caractère-par-caractère.

Sur ce `.pcap` la sortie renvoie : `ft_wandr...NDRel.L0L` (mot de passe correct).

3) Mot de passe découvert :

Les points correspondent aux backspaces de l'utilisateur ce qui donne : `ft_waNDReL0L` (mot de passe correct).
