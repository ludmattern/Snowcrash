# level14

1. Files found in `~/` : none

    - Analysis of the `getflag` binary with `ghidra`.

    We can see that the binary is protected by a debugger detection mechanism with `ptrace`.

    ```c
    lVar2 = ptrace(PTRACE_TRACEME,0,1,0); //fails in debug mode
    if (lVar2 < 0) {
        puts("You should not reverse this");
        uVar3 = 1;
    }
    ```

    Here the binary uses `getuid()` to retrieve the user's UID as in the previous exercise and returns the correct token accordingly.

    ```c
    _Var6 = getuid();
    __stream = stdout;
    if (_Var6 == 0xbbe /*3006 in decimal*/) {
        pcVar4 = (char *)ft_des("H8B8h_20B4J43><8>\\ED<;j@3");
        fputs(pcVar4,__stream);
    }
    else if ...
    ...
    ```

2. Exploitation

    The plan is therefore to intercept with `gdb` the moment when getuid sends its return value and change it within the program, while bypassing the anti-debugger protection.

    - Launch the program with `gdb` and Set two breakpoints at the entry of `getuid()` and `ptrace`: 

    ```c
    gdb /usr/bin/getflag
    break getuid
    break ptrace
    ```

    - modify the values that will be returned and get the token.

    ``` c
    return (int)1 //to bypass anti-debugger protection
    continue
    return (int)3014 //uid of level14 user
    continue
    ```

3. Token discovered

    The token is : `7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ`
