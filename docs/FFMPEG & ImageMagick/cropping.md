# Cropping Images and Video

Cropping images is something that is commonly done, however cropping many images at a time can become tedious. Furthermore cropping a video can be seen to be almost impossible without a non-linear video editing app at hand. This guide will show you the tools that imagemagick and ffmpeg provide to make thes tasks almost effortless.

## Cropping an Image with ImageMagick
### Removing Pixels from Edge of Image
Say we want to remove a certain number of pixel from each border of our image. Let's start by removing 200 pixels from the left and right of the image

 The command is the following:

``` bash
convert original.jpg -shave 200x0 output.jpg
```

To remove 200 pixels from the top and bottom, the command is:
``` bash
convert original.jpg -shave 0x200 output.jpg
```

But what if we want different values for each side? Then we can combine two crop flags:
``` bash
                      #left,top      right,bottom
convert test.png -crop +180+140 -crop -60-140 cropped.png
```

### Removing White Regions of an Image
If you just want to remove the white regions of an image, ImageMagick has a dedicated command for that:
``` bash
convert -trim input.jpg output.jpg
```

You can also use:
``` bash
mogrify -trim file.jpg
```
if you don't want to produce a copy of the original file.

### Remove Transparent Border of Image
If you ever stumble upon a png file with a large transparent border, you add in the `+repage` flag to remove the transparent border aswell:
``` bash
convert input.png -trim +repage output.png
```

## Cropping a Video with FFMPEG
Cropping videos in FFMPEG requires a little more thinking, let's start with the command:
``` bash
ffmpeg -i input.mp4 -filter:v "crop=w:h:x:y" output.mp4
```

What this all means:

- `-i input.mp4` specifies the input video (`input.mp4` being the input / original video in this case)

- `-filter:v` (can be abbreviated to `-vf`) specifies we're using a video filter

- `"crop=W:H:X:Y"` means we're using the `"crop"` video filter, with 4 values:
    
    - `w` the width of the output video (so the width of the cropped region), which defaults to the input video width (input video width = `iw`, which is the same as `in_w`); out_w may also be used instead of `w`
    
    - `h` the height of the output video (the height of the cropped region), which defaults to the input video height (input video height = `ih`, with `in_h` being another notation for the same thing); `out_h` may also be used instead of `h`
    
    - `x` the horizontal position from where to begin cropping, starting from the left (with the absolute left margin being `0`)
    
    - `y` the vertical position from where to begin cropping, starting from the top of the video (the absolute top being `0`)

- `output.mp4` is the new, cropped video file.

A few notes:

- The filter will automatically center the crop if `x` and `y` are omitted, so `x` defaults to `(iw-w)/2`, and `y` to `(ih-h)/2`

- There is also an optional `keep_aspect` option that you can set to `1` to force the output display aspect ratio to be the same of the input (example usage: `"crop=100:100:0:0:keep_aspect=1"`). This won't work with images, that's why you don't see a separate example with screenshot here.

- FFmpeg gets the original input video width (`iw`) and height (`ih`) values automatically, so you can perform mathematical operations using those values (e.g. `iw/2` for half the input video width, or `ih-100` to subtract 100 pixels from the input video height).