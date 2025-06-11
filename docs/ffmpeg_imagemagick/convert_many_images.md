# Convert Many Image Files

If you want to convert many image files from `png` to `jpg` for example, you can type the following command using mogrify. `cd` into the folder where the images are situated and the the command:
``` bash
mogrify -format jpg *.png
```

Note that this will not delete the original files. You will need to run a `rm -rf *.png` to remove the original files in this example.