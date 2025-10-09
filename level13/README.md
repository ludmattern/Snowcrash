# level13

1. Fichiers trouvés avec `ls` :

```bash
level13
```

2. Analyse du binaire `level13`

```bash
./level13
UID 2013 started us but we we expect 4242
```

- Nous allons donc voir comment l'uid est récupéré avec `strings`.

```bash
strings level13
[...]
getuid
[...]
```

- l'uid est donc récupéré avec `getuid()`.

```getuid() returns the real user ID of the calling process. ```

- On comprend donc qu'il ne sera pas possible de tromper getuid qui interroge directement le noyau système et qu'il va donc falloir procéder autrement.

3. Le plan

- Le plan est donc d'intercepter grâce à `gdb` le moment où getuid envoie sa valeur de retour et de la changer au sein du programme.

- - On lance le programme avec gdb : 

``` gdb ./level13 ```

- - On fixe un breakpoint à l'entrée de `getuid()` : 

``` break getuid ```

- - on modifie la valeur qui va être retournée : 

``` return (int)4242 ```

- - De retour dans le main on termine l'exécution du programme : 

``` continue ```

```bash
Continuing.
your token is 2A31L79asukciNyi8uppkEuSx
```

4. Token decouvert

- Le token est : `2A31L79asukciNyi8uppkEuSx`
