# `yt-dlp` Guide

`yt-dlp` is a simple terminal tool that allows you to download videos from YouTube onto your computer.

## Downloading a YouTube Video

You can download a youtube video with the simple command:
``` bash
yt-dlp "<url-of-video>"
```

There are other options for downloading YouTube videos with this command line tool:
- Download the best format (video + audio) that is equal to or greater than 720p width. Save this file as video_id.extension (`1La4QzGeaaQ.mp4`):
    ``` bash
    yt-dlp -f "best[height>=720]" "<video-url>" -o '%(id)s.%(ext)s'
    ```

- Download and merge the best video stream with the best audio stream:
    ``` bash
    yt-dlp -f 'bv*+ba' "<video-url>" -o '%(id)s.%(ext)s'
    ```

- Download 1080p video and merge with best audio stream:
    ``` bash
    yt-dlp -f 'bv*[height=1080]+ba' "<video-url>" -o '%(id)s.%(ext)s'
    ```

- Download 1080p video that is mp4 format and merge with best m4a audio format:
    ``` bash
    yt-dlp -f 'bv[height=1080][ext=mp4]+ba[ext=m4a]' --merge-output-format mp4 "<video-url>" -o '%(id)s.mp4'
    ```

- Embed video thumbnail into video file using `--embed-thumbnail`:
    ``` bash
    yt-dlp -f 'bv[height=1080][ext=mp4]+ba[ext=m4a]' --embed-thumbnail --merge-output-format mp4 "<video-url>" -o '%(id)s.mp4'
    ```

- Embed subtitles to video file (if they exist) using `--embed-subs`:
    ``` bash
    yt-dlp -f 'bv[height=1080][ext=mp4]+ba[ext=m4a]' --embed-subs --merge-output-format mp4 "<video-url>" -o '%(id)s.mp4'
    ```

- Embed metadata about the video using `--embed-metadata`:
    ``` bash
    yt-dlp -f 'bv[height=1080][ext=mp4]+ba[ext=m4a]' --embed-metadata --merge-output-format mp4 "<video-url>" -o '%(id)s.mp4'
    ```

The video will then be downloaded to the directory where you run the command in.

## Downloading Audio only from YouTube Video
There are flags to add to your command if you want to download the audio only from a video:
- As you will more than likely be wanting the best quality audio the format (`-f`) selector will be for best audio (`ba`).
- To save the highest quality audio as an mp3 file you need to define `--audio-format mp3` with `-x` which is extract audio.

``` bash
yt-dlp -f 'ba' -x --audio-format mp3 "<video-url>"  -o '%(id)s.%(ext)s'
```
