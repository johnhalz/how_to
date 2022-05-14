# Convert Video to GIF
FFMPEG can output a high quality GIF from videos if you use the options well. It can also extremely fast with such a command:
``` bash
ffmpeg -ss 30 -t 3 -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 output.gif
```

- This example will skip the first 30 seconds (`-ss 30`) of the input and create a 3 second output (`-t 3`).
- `fps` filter sets the frame rate. A rate of 10 frames per second is used in the example.
- `scale` filter will resize the output to 320 pixels wide and automatically determine the height while preserving the aspect ratio. The lanczos scaling algorithm is used in this example.
- `palettegen` and `paletteuse` filters will generate and use a custom palette generated from your input. These filters have many options, so refer to the links for a list of all available options and values.
- `split` filter will allow everything to be done in one command and avoids having to create a temporary PNG file of the palette.
- Control looping with `-loop` output option but the values are confusing. A value of 0 is infinite looping, -1 is no looping, and 1 will loop once meaning it will play twice. So a value of 10 will cause the GIF to play 11 times.

## ImageMagick `convert` Example
Another command-line method is to pipe from `ffmpeg` to `convert` (or `magick`) from ImageMagick.

``` bash
ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" -c:v pam -f image2pipe - | convert -delay 10 - -loop 0 -layers optimize output.gif
```

FFMPEG options:

- `-vf "fps=10,scale=320:-1:flags=lanczos"` is a filtergraph using the fps and scale filters. fps sets frame rate to 10, and scale sets the size to 320 pixels wide and height is automatically determined and uses a value that preserves the aspect ratio. The lanczos scaling algorithm is used in this example.
- `-c:v pam` Chooses the pam image encoder. The example outputs the PAM (Portable AnyMap) image format which is a simple, lossless RGB format that supports transparency (alpha) and is supported by convert. It is faster to encode than PNG.
- `-f image2pipe` chooses the image2pipe muxer because when outputting to a pipe FFMPEG needs to be told which muxer to use.

`convert` options:
- `-delay` See Setting frame rate section below.
- `-loop 0` makes an infinite loop.
- `-layers optimize` Will enable the general purpose GIF optimizer. See ImageMagick Animation Optimization for more details. It is not guaranteed that it will produce a smaller output, so it is worth trying without `-layers optimize` and comparing results.

### Setting Framerate
Set frame rate with a combination of the fps filter in `ffmpeg` and `-delay` in `convert`. This can get complicated because `convert` just gets a raw stream of images so no fps is preserved. Secondly, the `-delay` value in `convert` is in ticks (there are 100 ticks per second), not in frames per second. For example, with `fps=12.5` = 100/12.5 = 8 = `-delay 8`.

`convert` rounds the `-delay` value to a whole number, so 8.4 results in 8 and 8.5 results in 9. This effectively means that **only some frame rates are supported** when setting a uniform delay over all frames (a specific delay can be set per frame but that is beyond this answer).

`-delay` appears to be ignored if used as an output option, so it has to be used before - as shown in the example.

Lastly, browsers and image viewers may implement a minimum delay, so your `-delay` may get ignored anyway.