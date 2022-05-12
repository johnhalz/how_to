# Stop Screen Tearing on Ubuntu

## Intel Graphics

Intel graphics on Linux usually aren’t too much of a problem. That’s probably because integrated graphics usually have fewer features, and the Intel driver stack is mostly open source. For screen tearing on Intel, the solution usually comes in the form of some additional configuration.

Because Intel uses open source drivers, the Xorg configuration is going to be your most direct route. Create a file at `/etc/X11/xorg.conf.d/20-intel.conf` or `/usr/share/X11/xorg.conf/20-intel.conf`, then place the following block of code inside:

```bash
Section "Device"
    Identifier "Intel Graphics"
    Driver "intel"
    Option "TearFree" "true"
EndSection
```

When you’re done, save and reboot.

```bash
reboot
```

## NVIDIA Graphics

1. Use the NVIDIA proprietary driver
2. Open the `NVIDIA X Server Settings` app
3. Select the `X Server Display Configuration`
4. Click on the `Advanced` button and check `Force Composition Pipeline` option
5. Click on the button `Save to X Configuration file` to set it at startup

### Fix For Firefox

1. Go to `about:config`
2. Select `layers.acceleration.force-enabled` and set to `true`


## AMD Graphics

1. Create a new configuration directory `/etc/X11/xorg.conf.d/`
   
   ```bash
   sudo mkdir -p /etc/X11/xorg.conf.d/
   ```

2. Use your favorite text editor to create a configuration file called `20-radeon.conf`:
   
   ```bash
   sudo vim /etc/X11/xorg.conf.d/20-radeon.conf
   ```

3. Put the information below inside the `20-radeon.conf` file and then save and exit the file:
   
   ```bash
   Section "Device"
       Identifier "Radeon"
       Driver "radeon"
       Option "TearFree" "on"
   EndSection
   ```

4. Reboot your system for the new configuration to take effect.
   
   ```bash
   reboot
   ```



X. If you still notice some screen tearing after preforming the steps above, then you'll need to edit the `20-radeon.conf` file to include an extra option. Use your text editor to edit the `20-radeon.conf` file to include the `AccelMethod` and `DRI` options and then reboot your system.

```bash
sudo vim /etc/X11/xorg.conf.d/20-radeon.conf
```

```bash
Section "Device"
    Identifier "Radeon"
    Driver "radeon"
    Option "TearFree" "on"
    Option "DRI" "3"
    Option "AccelMethod" "glamor"
EndSection
```

```bash
reboot
```