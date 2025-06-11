# Change Resolution

## Change Image Resolution
You can use ImageMagick's `convert` function to change the resolution of an image:
``` bash
convert original.png -resize 100x100 new.png
```

To resize an image to specific dimensions, without maintaining the original image’s aspect ratio, include a bang `!` at the end of the dimensions:
``` bash
convert original.png -resize 100x100! new.png
```

You can also use percentages with the resize flag. For example, if you want to make an image smaller by 50%, all you need to type is:
``` bash
magick original.png -resize 50% new.png
```

## Change Video Resolution
If you want to change the resolution of a video to 480x320, you can do this simply by entering the command:
``` bash
ffmpeg -i input.mp4 -vf "scale=480:320" output_320.mp4
```

Alternatively, if you know the width you want and you want to keep the same aspect ratio, you can use `-1`:
``` bash
ffmpeg -i input.mp4 -vf "scale=480:-1" output_320.mp4
```

If you want to change the aspect ratio, you can use the `setdar` flag:
``` bash
ffmpeg -i input.mp4 -vf "scale=480:320,setdar=4:3" output_320.mp4
```