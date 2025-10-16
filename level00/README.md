# level00

1. Files found in `~/` : none

    - Search for files belonging to `flag00` :

    ```bash
    find / -user flag00 -type f -ls 2>/dev/null
    ```

    - We find a file belonging to `flag00` :

    ```bash
    /usr/sbin/john
    ```

    - Containing the string : `cdiiddwpgswtgt`

2. Analysis and decoding

    The string `cdiiddwpgswtgt` is a Caesar cipher with a shift of 11.

    ```bash
    # Manual decoding or with a script
    # c -> n (shift +11)
    # d -> o (shift +11)
    # i -> t (shift +11)
    # etc...
    ```

3. Exploitation

    ```bash
    su flag00
    nottoohardhere
    [...]
    getflag
    Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
    ```

4. Token discovered

    The token is : `x24ti5gi3x0ol2eh4esiuxias`