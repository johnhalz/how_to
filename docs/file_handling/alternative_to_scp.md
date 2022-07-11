# Alternative to `scp` Command

The `scp` command has become outdated, and not as secure by today's standards. On this page we show an alternative to the `scp`, known as `rsync`. `rsync` is built into Unix systems now, and is built to be a fast and reliable way of copying files and folders to remote machines, you can read more about it [here](https://linux.die.net/man/1/rsync).

## Simple Copying

Simply copying a file can be done with the following command:

``` bash
rsync <source> <destination>
```

## Viewing Progress When Syncing

You can view the progress of the copy operation by passing the `--progress`, `-P` flag. You will also need to turn non the verbose option aswell:

``` bash
rsync -v --progress <source> <destination>
```

## Recursive Copying
Recursive copying is useful if you are copying an entire folder. You can do this with the `-r` flag:

``` bash
rsync -rv --progress <source> <destination>
```

## Other Flags & Example
The other flags and their descriptions for them can be found [here](https://linux.die.net/man/1/rsync).

Below we show an example of a good `rsync` command:

``` bash
rsync -rhv --progress ~/Downloads/some.pdf john@192.168.1.27:/home/Downloads/
```
