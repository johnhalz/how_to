# Create Bootable Drive in Terminal

## Linux
1. Download the iso into your `Downloads` folder

2. 

## Windows
The process is relatively simple; however, a limitation of this method is the issue of the installer being 5.2GB. You cannot burn a file bigger than 4GB on a FAT32 formatted drive, which is the only format that works with both Windows and macOS.

A workaround for this is to split the installer into smaller files, which requires the installation of a package manager, `wimlib`, that is installed through Homebrew. This will split the Windows installer file while creating the bootable disk.

1. Install `wimlib`:
    ``` bash
    brew install wimlib
    ```

2. Now, make sure your USB is connected to your Mac. Run the following command:
    ``` bash
    diskutil list
    ```

    This will bring up a list of drives connected to your Mac. Find and note down the USB drive's disk identifier, which should appear to the left of (external, physical), and should resemble disk2, disk3, and so on.

3. Use the following command to format the USB stick in Terminal (replace disk2 with your disk identifier):
    ``` bash
    diskutil eraseDisk MS-DOS WINDOWS11 GPT /dev/disk2
    ```

4. Download Windows iso file: [https://www.microsoft.com/en-us/software-download/](https://www.microsoft.com/en-us/software-download/)

5. Mount the Windows 11 ISO from the Downloads folder on your Mac. You can do this by double-clicking on the ISO file, which should then show up in your Mac's connected devices as CCCOMA_X64FRE_EN-US_DV9 or similar. Remember to match the file name exactly to the one above. If it's different (due to a different language preference), make sure to change it accordingly in the commands below.

6. Since the installer file is bigger than 4GB, we'll be using two separate commands to create the bootable disk. The first command will copy all the files apart from the install.wim file (which is 4.2GB) in size. The second command will use wimlib to split and copy the install.wim file to the USB stick.

    Use the following command to copy the content of the ISO image—excluding the install.wim file—onto the USB drive:
    ``` bash
    rsync -vha --exclude=sources/install.wim /Volumes/CCCOMA_X64FRE_EN-US_DV9/* /Volumes/WINDOWS11
    ```

7. Then run the following command to split and copy the install.wim file:
    ``` bash
    wimlib-imagex split /Volumes/CCCOMA_X64FRE_EN-US_DV9/sources/install.wim /Volumes/WINDOWS11/sources/install.swm 3000
    ```

    That's it! Terminal should successfully create the bootable disk, which you can now use to boot a fresh Windows installation.