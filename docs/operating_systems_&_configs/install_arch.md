# Install Arch Linux
!!! info
    This guide is based off of the [DistroTube Arch Linux Installation Guide 2020](https://youtu.be/PQgyW10xD8s)

!!! tip
    There is a new method for installing Arch linux using a script, while this is still in beta, they are slowly starting to roll it out to users. You can find more infor below:

    - [Installation video](https://odysee.com/@DistroTube:2/install-arch-linux-the-easy-way-with-the:3)
    - [Arch Wiki Link](https://wiki.archlinux.org/title/Archinstall)

## Base System Install
1. Set keyboard layout:
    ``` bash
    loadkeys fr_CH
    ```

2. Check that internet services are working:
    ``` bash
    ping 1.1.1.1
    ```

3. Update the system clock:
    ``` bash
    timedatectl set-ntp true && timedatectl status
    ```

4. Partition the disks
    1. List the disks in your system:
        ``` bash
        fdisk -l
        ```

    2. Open `fdisk` to create the partitions (in this example, we will assume that the disk is called `/dev/sda`)
        ``` bash
        fdisk /dev/sda
        ```

    3. Inside `fdisk` - Create a new GPT partition table: Type `g`
        We will create 3 new partitions, as shown in table below:
        
        | Partition #   | Type   | Size     |
        | ------------- | ------ | -------- |
        | Partition 1   | boot   | 550M     |
        | Partition 2   | swap   | 2G       |
        | Partition 3   | `/`    | (rest)   |

        1. Inside `fdisk` - Create Partition 1:
            - Type `n` to create a new partition
            - Type `1` to add a number to partition (`1` will also be chosen as default)
            - Type `(Enter)` to select the default starting point for partition
            - Type `+550M` to select the size of the partition
        
        2. Inside `fdisk` - Create Partition 2:
            - Type `n` to create a new partition
            - Type `2` to add a number to partition (`2` will also be chosen as default)
            - Type `(Enter)` to select the default starting point for partition
            - Type `+2G` to select the size of the partition

        3. Inside `fdisk` - Create Partition 3:
            - Type `n` to create a new partition
            - Type `3` to add a number to partition (`3` will also be chosen as default)
            - Type `(Enter)` to select the default starting point for partition
            - Type `(Enter)` to select the remaining disk space to be allocated to partition 3

        4. Inside `fdisk` - Select partition types
            - Type `t` to start the partition type selection process
            - Type `1` to select which partition we will set
            - Type `1` to select the `EFI System` type
            - Type `t` to start the partition type selection process
            - Type `2` to select which partition we will set
            - Type `19` to select the `Linux swap` type

            Partition 3 should be set as `Linux filesystem`, however as that is the default setting when the partition was created, we don't need to change it.

        5. Inside `fdisk` - Write the partitions to the disk
            - Type `w` to write the partitions. This will also exit `fdisk` when executed

5. Make the three filesystems for the three partitions that were created. Execute the following commands in order:
    ``` bash
    mkfs.fat -F32  /dev/sda1    # Make FAT32 filesystem for 1st partition
    mkswap /dev/sda2            # Make swap filesystem for 2nd partition
    swapon /dev/sda2            # Turn on swap filesystem for 2nd partition
    mkfs.ext4 /dev/sda3         # Make Ext4 filesystem for 3rd partition
    ```

6. Mount the Linux filesystem partition
    ``` bash
    mount /dev/sda3 /mnt
    ```

7. Install base system for Arch:
    ``` bash
    pacstrap /mnt base linux linux-firmware
    ```

8. Generate file system table:
    ``` bash
    genfstab -U /mnt >> /mnt/etc/fstab
    ```

9. Change into the root directory:
    ``` bash
    arch-chroot /mnt
    ```

10. Set the time-zone (we set the Zurich timezone here):
    ``` bash
    ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime && hwclock --systohc
    ```

    !!! tip
        You can list the regions by running: `ls /usr/share/zoneinfo/`

11. Set the locale
    1. Start by installing the `vim` editor:
        ``` bash
        pacman -S vim
        ```

    2. Edit the `/etc/locale.gen`
        ``` bash
        vim /etc/locale.gen
        ```

        - Uncomment the line of your choice (line with `en_US.UTF-8 UTF-8` or line with `fr_CH.UTF-8 UTF-8`)
        - Save and exit the file
    
    3. Run the command:
        ``` bash
        locale-gen
        ```

12. Set the hostname
    1. Create and edit the `/etc/hostname` file:
        ``` bash
        vim /etc/hostname
        ```

    2. Write the hostname of your computer (this is the name of the computer)
        ``` bash
        <hostname>
        ```

        Where `<hostname>` is the chosen hostname of the computer

    3. Edit the `/etc/hosts` file:
        ``` bash
        vim /etc/hosts
        ```

        - There should be a few lines commented in this file
        - At the end of the file add the following
            ```
            127.0.0.1       localhost
            ::1             localhost
            127.0.1.1       <hostname>.localdomain      <hostname>
            ```
        - Save and exit the file

13. Create password for root:
    ``` bash
    passwd
    ```

14. Create non-root user and password for that user:
    ``` bash
    useradd -m <username> && passwd <password>
    ```

    Where <username> and <password> are the chosen username and password.

15. Give non-root user sudo rights:
    ``` bash
    usermod -aG wheel,audio,video,storage,optical <username>
    ```

16. Install `sudo`:
    ``` bash
    pacman -S sudo
    ```

17. Allow members for the wheel group to access sudo
    1. Open the `visudo` config file with `vim`:
        ``` bash
        EDITOR=vim visudo
        ```

    2. Inside `visudo` - Uncomment the line about the wheen group (this will be under the `User priviledge specification` subtitle in the file, maybe line 82)
    3. Exit and save the file

18. Install grub:
    ``` bash
    pacman -S grub
    ```

19. Install EFI associate packages:
    ```bash
    pacman -S efibootmgr dosfstools os-prober mtools
    ```

20. Make EFI directory and boot directory:
    ``` bash
    mkdir /boot/EFI
    ```

21. Mount partition:
    ``` bash
    mount /dev/sda1 /boot/EFI
    ```

22. Run grub-install command:
    ``` bash 
    grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck --no-nvram --removable
    ```

23. Generate grub config file:
    ``` bash
    grub-mkconfig -o /boot/grub/grub.cfg
    ```

## Other Stuff Before Rebooting
24. Install network manager:
    ``` bash
    pacman -S networkmanager git
    ```

25. Enable network manager with systemd:
    ```bash
    systemctl enable NetworkManager
    ```

26. Exit out of `chroot`:
    ``` bash 
    exit
    ```

27. Unmount the `/mnt` directory:
    ``` bash
    umount -l /mnt
    ```

28. Shutdown the system (don't reboot this way you have time to remove installation media):
    ``` bash
    shutdown now
    ```

## Install AUR and `yay` Utility
29. Instal makepkg capabilities:
    ``` bash
    sudo pacman -S base-devel
    ```

29. Install AUR:
    ``` bash
    git clone https://aur.archlinux.org/yay-git.git
    ```

30. Go into the `yay-git` directory and make the arch package:
    ``` bash
    cd yay-git
    makepkg -si
    ```
