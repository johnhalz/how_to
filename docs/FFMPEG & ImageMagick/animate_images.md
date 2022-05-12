# Animate Images

## Create GIF from Images
Open the terminal and go to the directory where the images are located. For this example, let us say that we have a list of images called frame001.png, frame002.png, etc.

Type in the command:
``` bash
ffmpeg -i frame%03d.png -vf fps=10,scale=720:-1 output.gif
```

And you should have a gif named `output.gif`.

## Create Video from Images
Open the terminal and go to the directory where the images are located. For this example, let us say that we have a list of images called frame001.png, frame002.png, etc.

Type in the command:
``` bash
ffmpeg -framerate 30 -pattern_type glob -i '*.png' -c:v libx264 -pix_fmt yuv420p out.mp4
```