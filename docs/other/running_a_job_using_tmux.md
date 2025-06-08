# Running a Job Using Tmux

Is some cases, you might want to run a script that will take some time on a separate machine. You can connect to the machine via SSH and run commands, but if you exit from the SSh session, the terminal session will end, and any long-running command you started will be cancelled by the OS.

Tmux offers a solution to this.

## Steps

1. SSH into the remote machine:

    ``` bash
    ssh user@remote_host
    ```

2. Start a tmux session:

    ``` bash
    tmux new -s mysession
    ```

    **Note:** You can give a different name to the session than `mysession`.

3. Run your long-running command inside the tmux session.

4. Detach from the tmux session:

    Press Ctrl + b, then press d.

    (This will return you to the regular shell but leave the command running.)

5. Disconnect from SSH:

    ``` bash
    exit
    ```

6. Later, reconnect and resume the session:

    ``` bash
    ssh user@remote_host
    tmux attach -t mysession
    ```