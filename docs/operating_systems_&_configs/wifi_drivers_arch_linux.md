# Get Wifi Drivers on Mac Hardware for Arch Linux

The problem is that, by default, Linux uses the BCMA driver. Even if you install the `broadcom-wl` driver, it doesn’t use it by default. Therefore you need to do the following:

1. Install the correct `broadcom-wl` driver. `broadcom-wl-dkms` did not work for me but you could try it. `broadcom-wl` is kernel dependent so make sure you install the correct one.

2. Blacklist the BCMA driver in `/etc/modprobe.d/blacklist.conf` adding the text "blacklist bcma".

3. Install `broadcom-wl`. To do this, type `sudo pacman -S broadcom-wl` in the terminal. It will show a few options. The numbers you see before broadcom-wl are kernel versions. Find your kernel version using `uname -srm` and use the version for your kernel (e.g kernel 5.10.x uses 510, etc.).
