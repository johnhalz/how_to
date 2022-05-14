# How To Fix "System Program Problem Detected In Ubuntu"

1. Open a terminal and use the following command:
   ```bash
   sudo rm /var/crash/*
   ```
   
   This will delete all the content of directory `/var/crash`. This way you won’t be annoyed by the pop up for the programs crash that happened in the past.

2. Permanently get rid of system error pop up in Ubuntu. To disable the apport and get rid of system crash report completely, open a terminal and use the following command to edit the apport settings file:
   ```bash
   sudo gedit /etc/default/apport &
   ```

3. The content of the file is:
   ```bash
   # set this to 0 to disable apport, or to 1 to enable it
   # you can temporarily override this with
   # sudo service apport start force_start=1
   enabled=1
   ```
   
   Change the `enabled=1` to `enabled=0`. Save and close the file.
