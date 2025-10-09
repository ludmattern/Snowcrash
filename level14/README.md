# level14

1. Fichiers trouvés dans `~/` : aucun

- Analyse du binaire `getflag` avec `ghydra`.

On peut voir que le binaire est protégé par un mécanisme de détection de débogueur avec `ptrace`.
```c
	lVar2 = ptrace(PTRACE_TRACEME,0,1,0); //echoue en mode debug
	if (lVar2 < 0) {
	puts("You should not reverse this");
	uVar3 = 1;
	}
```

Ici le binaire utilise `getuid()` pour récupérer l'UID de l'utilisateur comme dans l'exercice précédent et retourne la bon token en fonction.
```c
	_Var6 = getuid();
	__stream = stdout;
	if (_Var6 == 0xbbe /*3006 en décimal*/) {
	pcVar4 = (char *)ft_des("H8B8h_20B4J43><8>\\ED<;j@3");
	fputs(pcVar4,__stream);
	}
	else if ...
	...
```

3. Exploitation

- Le plan est donc d'intercepter grâce à `gdb` le moment où getuid envoie sa valeur de retour et de la changer au sein du programme. tout en bypassant la protection anti debugger.

- On lance le programme avec gdb : 

``` gdb /bin/getflag ```

- On fixe deux breakpoints à l'entrée de `getuid()` et de `ptrace`: 

``` c
break getuid
break ptrace
```

- on modifie les valeurs qui vont être retournées et on obtient le token.

``` c
return (int)1 //pour bypasser la protection anti-debugger
continue
return (int)3014 //uid de l'utilisateur level14
continue
```

4. Token decouvert

- Le token est : `7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ`
