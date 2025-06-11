# SSH Errors & How to Fix Them

## Remote Host ID has Changed

If you used `ssh` command to connect to the remote server, but you got the following error message causing you can not connect:

``` bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:wruvASsAzradn/Acy0g0YwuDGBZb7ierhmL/fhhsSu4.
Please contact your system administrator.
Add correct host key in /Users/clay/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/clay/.ssh/known_hosts:13
ECDSA host key for ohio.cs.nccu.edu.tw has changed and you have requested strict checking.
Host key verification failed.
```

The solution is to run the following command:
``` bash
ssh-keygen -R <ip-address>
```

Where `<ip-address>` is the IP of the remote connection you were attemptingto connect to. Re-run the standard SSH command and you should be good to go!