# Looping through Files and Performing Tasks

To be able to easily perform a command onto many files in a folder, you can use a simple shell `for` loop:
``` bash
for f in *.png; do convert $f -trim +repage $f; done
```

Remeber to put a semi-colon between each new line!