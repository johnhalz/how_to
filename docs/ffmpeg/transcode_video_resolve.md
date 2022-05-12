# Transcode Video Footage to make Compatible for Resolve

For a single file you can use:

``` bash
ffmpeg -i GOPROxxx.MP4 -vcodec mjpeg -q:v 2 -acodec pcm_s16be -q:a 0 -f mov OUTxxx.mov
```

For a directory with many files, you can use :

``` bash
mkdir transcoded; for i in *.mp4; do ffmpeg -i "$i" -vcodec mjpeg -q:v 2 -acodec pcm_s16be -q:a 0 -f mov "transcoded/${i%.*}.mov"; done
```