# Advanced File Counting in Terminal

## Count Number of Files and Directories (without hidden files)
You can simply run the combination of the `ls` and `wc` command and it will display the number of files:
``` bash
ls | wc -l
```

## Count Number of Files and Directories (including hidden files)
You can simply run the combination of the `ls` and `wc` command with the `-A` flag and it will display the number of files (includeing the hidden files):
``` bash
ls -A | wc -l
```

## Count Only Files (not directories or subdirectories)
All you have to do is to add the `depth` of your find. If you set it at 1, it won’t enter the subdirectories.
``` bash
find . -maxdepth 1 -type f | wc -l
```

## Count files of Specific Type (in this directory)
Unfortunately this benign problem is difficult to solve in a way which supports all file names and is portable. This is safe (it handles hidden files, paths containing spaces, dashes and even newlines). In this example, we want to find all of the files with the `toml` extension:
``` bash
find . -mindepth 1 -maxdepth 1 -type f -name "*.toml" | uniq -c
```

## Count Number of Files and Directories (including the subdirectories)
If you want to count the number of files and directories in all the subdirectories, you can use the tree command:
``` bash
tree -a
```

### Count Only Files (not directories, including subdirectories)
The above command searched for all the files (type `f`) in current directory and its subdirectories.
``` bash
find . -type f | wc -l
```