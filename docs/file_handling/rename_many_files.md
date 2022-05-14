# Advanced File Renaming in Terminal

## Install `rename` Utility

=== "macOS"
    ``` bash
    brew install rename
    ```

=== "Ubuntu"
    ``` bash
    sudo apt install rename
    ```

=== "Arch"
    (Rename is built into Arch)

## Remove First n Characters from Filenames

To remove the first 8 characters from filename, type the following

``` bash
rename -n -v 's/^(.{8})//' *
```

`-n`is for no action and `-v`is to show the changes. If you are satisfied with the results you can remove the flags:

``` bash
rename 's/^(.{8})//' *
```

## Remove Last n Characters from Filenames (while keeping the extension you want)

If we want to remove the last 29 characters from all of the mkv files in a folder, however we want to keep the mkv extension, we can write:

``` bash
rename -n -v 's/.{29}$/.mkv/' *.mkv
```

Again, `-n` is for no action and `-v`is to show the changes. If you are satisfied with the results you can remove those two flags.

# Delete Part of a Filename
The `rename` option also allows you to delete a part of the filename by omitting the replacement part of the expression. For instance, if we want to shorten `example` into `ex`:

``` bash
rename -v 's/ample//' *.txt
```

# Rename Files with Similar Names
Another use for the `rename` option is to rename files with similar names. For instance, if we want to rename files with `example` and `sample` in their name to test:

``` bash
rename -v 's/(ex|s)ample/test/' *.txt
```