# Make Grub Menu Appear for Dual Boot Systems

Open the grub config file: 

```bash
sudo vim /etc/default/grub
```

Change the lines:

    GRUB_TIMEOUT_STYLE=hidden
    GRUB_TIMEOUT=0

to

```bash
GRUB_TIMEOUT_STYLE=menu
#GRUB_TIMEOUT=0
```

Exit the file, then run the command

```bash
sudo update-grub
```

So that the changes can be registered. If you have a drive with another OS on it, it should be detected and displayed in the grub menu the next time you boot up. Good luck!
