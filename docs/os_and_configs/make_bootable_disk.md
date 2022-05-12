# Make Bootable Disk in Terminal

> **Note: This document is not finalised yet!**

## On MacOS
1. Locate your USB disk
    ``` bash
    diskutil list
    ```
    Then find your disk info as below:
    ``` bash
    /dev/disk2 (external, physical):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:     Apple_partition_scheme                        *31.0 GB    disk2
        1:        Apple_partition_map                         4.1 KB     disk2s1
        2:                  Apple_HFS                         2.4 MB     disk2s2
    ```

2. Erase the disk
   Erasing disks from the command-line can be a dangerous process as there aren’t any warnings or confirmations. One typo could lead to irreversible data loss if there’s no backup to restore from. If you’re not familiar with the command-line, Disk Utility is just as capable.

   To erase an entire disk:
   ``` bash
    diskutil eraseDisk {filesystem} {Name_to_use} /dev/{disk_identifier}
   ```

   Erasing a whole disk will clear any partitions and create a new, single partition, before formatting it as a volume.
   - `{filesystem}`:
     You can specify the filesystem to format the partition in by using any that are supported. To find out which file-systems you can use, enter:

     `diskutil listFilesystems`

     Usually we will use FAT32.

   - `{Name_to_use}`: This simply refers to the name of the volume that will be created. In this instance, I’ve just labelled the volume as “Test”.
   - `{disk_identifier}`: Only the primary part of the identifier (i.e. `disk1`, `disk2`, `disk3`...) is needed.

    If successful, it will show information like this:
    ``` bash
    Started erase on disk2
    Unmounting disk
    Creating the partition map
    Waiting for partitions to activate
    Formatting disk2s2 as MS-DOS (FAT32) with name KF-USB
    512 bytes per physical sector
    /dev/rdisk2s2: 60161312 sectors in 1880041 FAT32 clusters (16384 bytes/cluster)
    bps=512 spc=32 res=32 nft=2 mid=0xf8 spt=32 hds=255 hid=411648 drv=0x80 bsec=60190720 bspf=14688 rdcl=2 infs=1 bkbs=6
    Mounting disk
    Finished erase on disk2
    ```

    Meanwhile the USB is automatically mounted to you system as well. In my case, it’s mounted at `/Volumes/KF-USB`.

    Now use command `diskutil list` again to show the disk partition information, you’ll see the new disk’s info:

    ``` bash
    /dev/disk2 (external, physical):
        #:                       TYPE NAME                    SIZE       IDENTIFIER
        0:      GUID_partition_scheme                        *31.0 GB    disk2
        1:                        EFI EFI                     209.7 MB   disk2s1
        2:       Microsoft Basic Data KF-USB                  30.8 GB    disk2s2
    ```

3. Create the Bootable Drive
    Assuming our flash drive is still `disk2`, then
    ``` bash
    diskutil unmountDisk disk2
    ```

    The `unmountDisk` command unmounts all volumes of the given disk drive but keeps the drive itself visible to the computer (as opposed to the eject option that disconnects it entirely).

    Then, run the following command to create the bootable USB:

    ``` bash
    sudo dd if=/Users/kyle/Downloads/Linux.iso of=/dev/disk2 bs=8m
    ```

    - `sudo` tells the system to use root level (that is the system’s highest level) privileges to perform the following action.
    - `dd`  is an extremely basic, but powerful block level copy command built into all Linux and Unix operating systems (MacOS is UNIX based)
    - `if` stands for input file (a.k.a the source file or location). In our use-case, this is the .ISO file. In MacOS, if you have a finder window open, you can drag and drop the .iso into the terminal and it will auto-fill this file path.
    - `of` stands for “output file” (a.k.a the destination file or location). For us, this is our USB drive, `disk2`. The specific path for external drives is in `/dev`, hence `/dev/disk2`
    - `bs` stands for block size. dd copies data in blocks rather than on a file by file basis (this is why it’s so fast) and this command gives you the option to set how big each block is. There is a science to the ideal block size, but I don’t know it. 8m (MegaBytes) has consistently worked well for my uses.

    The command will not show any progress until it’s done, but you can press control+t for status updates. With an average computer, this takes less than 5min to complete. Once complete:

    ``` bash
    diskutil eject disk2
    ```

    The USB drive can now safely be removed. Assuming that the iso is EFI-compatible, you can reboot your mac to test it.

## On Linux
1. Find and format the disk using `fdisk`
   ``` bash
   fdisk -l
   ```

   This will give you a list of all the disk drives that are available on the system. You’ll see a separate section with just a single disk path like `/dev/sdb1` with a mount path that’s different from the common ones in Linux (like `/home/`, `/etc/`, `/boot/` etc.). With Ubuntu, the default mount point is in the `/media/` directory. Mine was mounted on the `/mnt/`

2. Unmount the disk
    We need to unmount it so we can proceed to write the ISO to the USB. We use the umount command for this purpose. This action can be executed in two different methods. 

    ``` bash
    sudo umount /path/where/mounted
    ```

    Alternatively, we can use the device’s name in this format.
    ``` bash
    sudo umount /device/name
    ```

3. Write the ISO to the disk
   Our USB disk has been unmounted and our ISO file is already downloaded on our system. Now we will make this USB drive bootable for Ubuntu 20.04 using one single command. This is how you enter this command in the terminal.

   ``` bash
   sudo dd bs=4M if=/path/to/ISOfile of=/dev/sdx status=progress oflag=sync
   ```

   This should start the process of writing the ISO image file on your USB disk and converting it into a bootable drive. You should see a screen as given below.