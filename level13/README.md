# level13

1. Files found in `~/` :

    ```bash
    level13
    ```

2. Analysis of the `level13` binary

    ```bash
    ./level13
    UID 2013 started us but we we expect 4242
    ```

    We will therefore see how the uid is retrieved with `strings`.

    ```bash
    strings level13
    [...]
    getuid
    [...]
    ```

    The uid is therefore retrieved with `getuid()`.

    ```getuid() returns the real user ID of the calling process.```

    We therefore understand that it will not be possible to fool getuid which directly queries the system kernel and that we will therefore have to proceed differently.

3. Exploitation

    The plan is therefore to intercept with `gdb` the moment when getuid sends its return value and change it within the program.

    ```bash
    gdb ./level13
    break getuid
    return (int)4242
    continue
    ```

    ```bash
    Continuing.
    your token is 2A31L79asukciNyi8uppkEuSx
    ```

4. Token discovered

    The token is : `2A31L79asukciNyi8uppkEuSx`
