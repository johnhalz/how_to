# Port Arch Linux to Jetson

This guide will show you how to port Arch Linux onto the Nvidia Jetson Nano Devkit. Including installing the kernel, drivers, and configuration from the Linux for Nano release provided by NVIDIA. It will also provide steps for testing that the NVIDIA drivers have been installed correctly and are working correctly.

## On host computer (preferably running Linux)

### Obtaining Drivers and Image
1. Download the latest Nano Driver Package from https://developer.nvidia.com/linux-tegra
    ``` bash
    wget https://developer.nvidia.com/embedded/l4t/r32_release_v6.1/t210/jetson-210_linux_r32.6.1_aarch64.tbz2 Linux_for_Tegra.tbz2
    ```

    > *Note:* The link is different for the AGX Xavier Jetson.

2. Extract the downloaded Jetson Nano driver packages (from step 1) to a folder called `Linux_for_Tegra`:
    ``` bash
    sudo tar jxpf Linux_for_Tegra.tbz2
    ```

3. Go into the directories created from the extracted tar file (from step 2):
    ``` bash
    cd Linux_for_tegra  # We will refer to this directory as <path_to_L4T_TOP_DIR>
    cd rootfs           # We will refer to this directory as <path_to_L4T_rootfs>
    ```

4. Enter the `<path_to_L4T_rootfs>` folder, download and extract the arch image file:
    ``` bash
    cd rootfs
    wget http://os.archlinuxarm.org/os/ArchLinuxARM-aarch64-latest.tar.gz
    sudo tar -xpf ArchLinuxARM-aarch64-latest.tar.gz
    ```

### Changing the Configuration Scripts
Compared to Ubuntu a few configuration changes are needed as Arch Linux has a different directory structure and init program. This section outlines the changes that need to be made to the archives in nv_tegra folder of the driver package of Release. Many of the files / directories need to be relocated as the file system hierarchy differs between Arch Linux and Ubuntu.

5. Modify the `nv_customize_rootfs.sh`
    ``` bash
    sudo vim Linux_for_Tegra/nv_tools/scripts/nv_customize_rootfs.sh
    ```

    Find the part of the script where:
    ``` bash
    if [ -d "${LDK_ROOTFS_DIR}/usr/lib/arm-linux-gnueabihf/tegra" ]; then
        ARM_ABI_DIR_ABS="usr/lib/arm-linux-gnueabihf"
    elif [ -d "${LDK_ROOTFS_DIR}/usr/lib/arm-linux-gnueabi/tegra" ]; then
        ARM_ABI_DIR_ABS="usr/lib/arm-linux-gnueabi"
    elif [ -d "${LDK_ROOTFS_DIR}/usr/lib/aarch64-linux-gnu/tegra" ]; then
        ARM_ABI_DIR_ABS="usr/lib/aarch64-linux-gnu"
    else
        echo "Error: None of Hardfp/Softfp Tegra libs found"
        exit 4
    fi
    ```

    And add the two lines:
    ``` bash
    if [ -d "${LDK_ROOTFS_DIR}/usr/lib/arm-linux-gnueabihf/tegra" ]; then
        ARM_ABI_DIR_ABS="usr/lib/arm-linux-gnueabihf"
    elif [ -d "${LDK_ROOTFS_DIR}/usr/lib/arm-linux-gnueabi/tegra" ]; then
        ARM_ABI_DIR_ABS="usr/lib/arm-linux-gnueabi"
    elif [ -d "${LDK_ROOTFS_DIR}/usr/lib/aarch64-linux-gnu/tegra" ]; then
        ARM_ABI_DIR_ABS="usr/lib/aarch64-linux-gnu"
    elif [ -d "${LDK_ROOTFS_DIR}/usr/lib/tegra" ]; then                     # Add this line
        ARM_ABI_DIR="${LDK_ROOTFS_DIR}/usr/lib"                             # Add this line
    else
        echo "Error: None of Hardfp/Softfp Tegra libs found"
        exit 4
    fi
    ```

6. Extract the tars of the nv_tegra folder into directories of the same name:
    ``` bash
    cd Linux_for_Tegra/nv_tegra
    sudo mkdir nvidia_drivers config nv_tools nv_sample_apps/nvgstapps

    sudo tar -xpjf nvidia_drivers.tbz2 -C nvidia_drivers/
    sudo tar -xpjf config.tbz2 -C config/
    sudo tar -xpjf nv_tools.tbz2 -C nv_tools/
    sudo tar -xpjf nv_sample_apps/nvgstapps.tbz2 -C nv_sample_apps/nvgstapps/
    ```


#### Changes to `nvidia_drivers` Package
7. The `lib` folder will need to be moved into `/usr`:
    ``` bash
    cd Linux_for_Tegra/nv_tegra/nvidia_drivers
    sudo mv lib/* usr/lib/
    sudo rm -r lib
    ```

8. Everything in `usr/lib/aarch64-linux-gnu` will need to be moved into `usr/lib`:
    ``` bash
    sudo mv usr/lib/aarch64-linux-gnu/* usr/lib/
    sudo rm -r usr/lib/aarch64-linux-gnu
    ```

9. `nv_tegra_release` in `etc/` will need to be updated to include the correct path to the tegra libraries:
    ``` bash
    sudo vim Linux_for_Tegra/nv_tegra/nvidia_drivers/etc/nv_tegra_release
    ```

    The line in `nv_tegra_release` need to be replaced. Find the line:
    ``` bash
    0c165125388fbd943e7f8b37a272dec7c5d57c15 */usr/lib/aarch64-linux-gnu/tegra/libnvmm.so
    ```

    Replace it with the correct path:
    ``` bash
    0c165125388fbd943e7f8b37a272dec7c5d57c15 */usr/lib/tegra/libnvmm.so
    ```

10. Now go into the folder `Linux_for_Tegra/nv_tegra/nvidia_drivers/etc/ld.so.conf.d/`:
    ``` bash
    sudo vim Linux_for_Tegra/nv_tegra/nvidia_drivers/etc/ld.so.conf.d/nvidia-tegra.conf
    ```

    Point to the right directory and add the `tegra-egl` entry. The contents of `nvidia-tegra.conf` should look like this:
    ``` bash
    /usr/lib/tegra
    /usr/lib/tegra-egl
    ```

#### Changes to the `nv_tools` Package
*Note:* In more recent versions of the nvidia drivers package (downloaded in step 1) you may not need to do step 11 as it has already done for you. Check the directories before running the commands in step 11.

11. The tegrastats script should be moved from `home/ubuntu` into the `/usr/bin` directory. This removes the dependency on a user called `ubuntu`:
    ``` bash
    cd Linux_for_Tegra/nv_tegra/nv_tools
    mkdir -p usr/bin
    mv home/ubuntu/tegrastats usr/bin/
    rm -r home
    ```

#### Changes to `nvgstapps` Package
*Note:* In more recent versions of the nvidia drivers package (downloaded in step 1) you may not need to do step 12 as it has already done for you. Check the directories before running the commands in step 12.

12. Move the contents of `usr/sbin` into `usr/bin``
    ``` bash
    cd Linux_for_Tegra/nv_tegra/nv_sample_apps/nvgstapps/
    sudo mv usr/sbin/* usr/bin
    sudo rm -r usr/sbin
    ```

13. Move the contents of `usr/lib/aarch64-linux-gnu` to `usr/lib`
    ``` bash
    sudo mv usr/lib/aarch64-linux-gnu/* usr/lib/
    sudo rm -r usr/lib/aarch64-linux-gnu
    ```

#### Finalizing Configuration Changes
14. When you have finished making all the listed changes, repackage the files:
    ``` bash
    cd <path_to_L4T_TOP_DIR>/nv_tegra
    ```

    ``` bash
    cd nvidia_drivers
    sudo tar -cpjf ../nvidia_drivers.tbz2 *
    cd ..
    ```

    ``` bash
    cd config
    sudo tar -cpjf ../config.tbz2 *
    cd ..
    ```

    ``` bash
    cd nv_tools
    sudo tar -cpjf ../nv_tools.tbz2 *
    cd ..
    ```

    ``` bash
    cd nv_sample_apps/nvgstapps
    sudo tar -cpjf ../nvgstapps.tbz2 *
    cd ../..
    ```

### Changes to `rootfs`
The following are changes that will be made to contents in your rootfs directory.

#### Initialization Script​
As Arch Linux uses systemd rather than upstart, the init script will need to be converted into a systemd service. Information on systemd and how to create services can be found on the [Arch Linux Wiki page for systemd](https://wiki.archlinux.org/title/Systemd)

15. To create the systemd service, we will need the service descriptor file, that tells systemd about the service. Hence need to create a service file as below in `<path_to_L4T_TOP_DIR>/rootfs/usr/lib/systemd/system/nvidia-tegra.service`:

    *Self-Note:* Path may actually be `<path_to_L4T_TOP_DIR>/usr/lib/systemd/system/nvidia-tegra.service`. Make try the first one before showing this guide to anyone else!!

    Create and open file in vim editor:
    ``` bash
    sudo vim Linux_for_Tegra/usr/lib/systemd/system/nvidia-tegra.service
    ```

    File contents:
    ``` bash
    ##Location
    ## /usr/lib/systemd/system/nvidia-tegra.service

    [Unit]
    Description=The NVIDIA tegra init script

    [Service]
    type=oneshot
    RemainAfterExit=yes
    ExecStart=/usr/bin/nvidia-tegra-init-script

    [Install]
    WantedBy=multi-user.target
    ```

16. Similarly, create a new file at `<path_to_L4T_TOP_DIR>/rootfs/usr/bin/nvidia-tegra-init-script`, and place these contents inside:

    Create and open fil in vim editor:
    ``` bash
    sudo vim Linux_for_Tegra/rootfs/usr/bin/nvidia-tegra-init-script
    ```

    File contents:
    ``` bash
    #!/bin/bash

    if [ -e /sys/power/state ]; then
        chmod 0666 /sys/power/state
    fi

    if [ -e /sys/devices/soc0/family ]; then
        SOCFAMILY="`cat /sys/devices/soc0/family`"
    fi

    if [ "$SOCFAMILY" = "Tegra210" ] &&
        [ -e /sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq ]; then
        sudo bash -c "echo -n 510000 > /sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq"
    fi

    if [ -d /sys/devices/system/cpu/cpuquiet/tegra_cpuquiet ] ; then
        echo 500 > /sys/devices/system/cpu/cpuquiet/tegra_cpuquiet/down_delay
        echo 1 > /sys/devices/system/cpu/cpuquiet/tegra_cpuquiet/enable
    elif [ -w /sys/module/cpu_tegra210/parameters/auto_hotplug ] ; then
        echo 1 > /sys/module/cpu_tegra210/parameters/auto_hotplug
    fi

    if [ -e /sys/module/cpuidle/parameters/power_down_in_idle ] ; then
        echo "Y" > /sys/module/cpuidle/parameters/power_down_in_idle
    elif [ -e /sys/module/cpuidle/parameters/lp2_in_idle ] ; then
        echo "Y" > /sys/module/cpuidle/parameters/lp2_in_idle
    fi

    if [ -e /sys/block/sda0/queue/read_ahead_kb ]; then
    echo 2048 > /sys/block/sda0/queue/read_ahead_kb
    fi
    if [ -e /sys/block/sda1/queue/read_ahead_kb ]; then
        echo 2048 > /sys/block/sda1/queue/read_ahead_kb
    fi

    for uartInst in 0 1 2 3
    do
        uartNode="/dev/ttyHS$uartInst"
        if [ -e "$uartNode" ]; then
            ln -s /dev/ttyHS$uartInst /dev/ttyTHS$uartInst
        fi
    done

    machine=`cat /sys/devices/soc0/machine`
    if [ "${machine}" = "jetson-nano-devkit" ] ; then
        echo 4 > /sys/class/graphics/fb0/blank
                BoardRevision=`cat /proc/device-tree/chosen/board_info/major_revision`
                if [ "${BoardRevision}" = "A" ] ||
                        [ "${BoardRevision}" = "B" ] ||
                        [ "${BoardRevision}" = "C" ] ||
                        [ "${BoardRevision}" = "D" ]; then
                        echo 0 > /sys/devices/platform/tegra-otg/enable_device
                        echo 1 > /sys/devices/platform/tegra-otg/enable_host
                fi
    fi

    if [ -e /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors ]; then
        read governors < /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors
        case $governors in
            *interactive*)
                echo interactive > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
                if [ -e /sys/devices/system/cpu/cpufreq/interactive ] ; then
                    echo "1224000" >/sys/devices/system/cpu/cpufreq/interactive/hispeed_freq
                    echo "95" >/sys/devices/system/cpu/cpufreq/interactive/target_loads
                    echo "20000" >/sys/devices/system/cpu/cpufreq/interactive/min_sample_time
                fi
                    ;;
            *)
                    ;;
        esac
    fi

    echo "Success! Exiting"
    exit 0
    ```

17. Enable the script before flashing / first boot by create the following symbolic link to enable the service:
    ``` bash
    cd Linux_for_Tegra/rootfs/etc/systemd/system/sysinit.target.wants/
    ln -s /usr/lib/systemd/system/nvidia-tegra.service nvidia-tegra.service
    ```

    This should be executed after apply_binaries, so the nvidia-tegra service is in place.

### Pacman Configuration
18. As we have installed a custom kernel to boot linux on the jetson-nano-devkit, it is necessary to update pacman.conf to ignore updates to the kernel package. To do so add `linux` as an ignored package to your `<path_to_L4T_rootfs>/etc/pacman.conf` as below:

    Open file in vim editor:
    ``` bash
    sudo vim Linux_for_Tegra/rootfs/etc/pacman.conf
    ```

    Edit the following line to:
    ``` bash
    IgnorePkg=linux
    ```

## Flashing the Jetson
The steps for flashing the Arch Linux image to the Jetson are no different than flashing the image for Ubuntu.

19. Enable recovery mode on the Jetson:

    - Enabling recovery Mode on the Jetson:
        1. Jumper the J48 power select pin first and plug the power jack
        2. Jumper the recovery pin
        3. Jumper the reset pin
        4. Remove the jumper of reset pin
        5. Remove the jumper of recovery pin.

        You should be able to see an nvidia device with `lsusb` command on your host computer.

20. Plug the jetson in via USB to the host computer (where you have done steps 1-18).

21. Apply the NVIDIA specific configuration, binaries and the L4T kernel:
    ``` bash
    sudo ./apply_binaries.sh
    ```

22. Create the image from the `rootfs` directory and flash the image to the Jetson:
    ``` bash
    sudo ./flash.sh jetson-nano-devkit mmcblk0p1
    ```

    *Self-Note:* You may need to change the big `if` statement of the `apply_binaries.sh` script to get it to apply the changes you made in the previous steps. Test and see.

23. Your device should reboot and prompt you to login. The default login for Arch Linux ARM is:
    Username: root
    Password: root