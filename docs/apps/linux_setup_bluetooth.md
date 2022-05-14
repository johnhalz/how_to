# Set Up Bluetooth in Linux

1. Start by installing bluez*, it adds a bunch of drivers and will most likely be the reason why your headset works now.
   ```bash
    sudo apt-get install bluez*
   ```

2. Install blueman, this will help connect to your headphones.
   ```bash
   sudo apt-get install blueman
   ```

3. Go into the file `/etc/bluetooth/main.conf`
   ```bash
   sudo vim /etc/bluetooth/main.conf
   ```

4. Change the following lines:
   ```bash
   ControllerMode = bredr
   ```
   or
   ```bash
   ControllerMode = dual
   ```

5. Save the file and exit, then run the command:
   ```bash
   sudo /etc/init.d/bluetooth restart
   ```
