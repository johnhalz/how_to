# SSH into Raspberry Pi through USB

1. **Edit the image:** To access the Pi Zero over USB you have to edit the image first.

    - If you have the SD card in your Pi Zero, power it down and remove it
    - Put the SD card in an adapter and plug it into your computer
    - On a Mac the SD card should appear on your desktop
    - Open the SD card icon to explore the contents

2. **Access the micro SD card from the command line**.

    In the command line enter the following command:

    ``` bash
    ls -ls /Volumes/
    ```

    You should see something like this:

    ``` bash
    total 13
    8 lrwxr-xr-x  1 root   admin     1 Jul 28 09:41 Macintosh HD -> /
    5 drwxrwxrwx@ 1 mitch  staff  2560 Jul 28 11:47 boot
    ```

    The volume named `boot` should be the SD card with the Raspbian image on it.

3. **Enable SSH:** There was a security update to the Raspbian images. Now to enable ssh by default you have to do the following:

    ``` bash
    touch /Volumes/boot/ssh
    ```

    This will write an empty file to the root of your Raspbian image. That will enable ssh on startup.

4. **Edit `config.txt`**

    - In the root folder of the SD card, open `config.txt` (`/Volumes/boot/config.txt`) in a text editor.
    - Append this line to the bottom of it:
        
        ``` bash
        dtoverlay=dwc2
        ```

    - Save the file

5. **Edit `cmdline.txt`**

    - In the root folder of the SD card, open `cmdline.txt` (`/Volumes/boot/cmdline.txt`) in a text editor
    - After `rootwait`, append this text leaving only one space between `rootwait` and the new text (otherwise it might not be parsed correctly):

        ``` bash
        modules-load=dwc2,g_ether
        ```

    - If there was any text after the new text make sure that there is **only one space between that text and the new text**.
    - Save the file

6. **Boot the Pi**

    - Give the Pi plenty of time to bootup (can take as much as 90 seconds or more)
    - You can monitor the RNDIS/Ethernet Gadget status in the System Preferences / Network panel (note that the IP address listed is not the host)

7. **Login over USB**

    - If this is **not** the first time connecting to your raspberry pi, refresh the SSH key by typing the command: 
        
        ``` bash
        ssh-keygen -R <hostname>.local
        ```

        If this is the first time connecting to the Pi, you won't need to run this command.

    - Connect to the Pi:

        ``` bash
        ssh <username>@<hostname>.local
        ```