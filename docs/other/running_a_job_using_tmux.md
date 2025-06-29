# Running a Job Using Tmux

Is some cases, you might want to run a script that will take some time on a separate machine. You can connect to the machine via SSH and run commands, but if you exit from the SSh session, the terminal session will end, and any long-running command you started will be cancelled by the OS.

Tmux offers a solution to this.

## Steps

### SSH into the remote machine:

``` bash
ssh user@remote_host
```

### Start a tmux session:

``` bash
tmux new -s mysession
```

**Note:** You can give a different name to the session than `mysession`.

### Run your long-running command inside the tmux session.

``` bash
python run_long_script.py
```

### Detach from the tmux session:

Press ++ctrl+b++, and then press ++d++.

(This will return you to the regular shell but leave the command running.)

### Disconnect from SSH:

``` bash
exit
```

### Later, reconnect and resume the session:

``` bash
ssh user@remote_host
tmux attach -t mysession
```

### Killing a Session

``` bash
tmux kill-session -t mysession
```

## References

To learn more, you can visit the following cheat-sheet: <https://tmuxcheatsheet.com/>