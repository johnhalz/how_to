# Working with External Drives on Ubuntu

1. To see all drives, you can type in the command:
    ```bash
    df -h
    ```

2. If you want to format a drive, you can type in the command:
    ```bash
    sudo umount /dev/sdXX
    ```
    
    Where `/dev/sdXX/` is the name of the drive you can find using the command `df -h`.
    
    You must then create a file system in the drive, and you can do this with the `mkfs` command.
   
   
