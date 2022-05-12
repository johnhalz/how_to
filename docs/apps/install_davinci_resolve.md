# How to Install DaVinci Resolve

## Ubuntu

### Install Dependencies and Libraries

Before doing anything else, you need to manually install all the dependencies and libraries needed for the software to run. If you try to install DaVinci Resolve before doing this, it won’t work.

Run these commands in your terminal and be sure to do them all:

``` bash
sudo apt-get install libssl-dev

sudo ln -s /usr/lib /usr/lib64

sudo ln -s /usr/lib/x86_64-linux-gnu/libgstreamer-1.0.so.0 /usr/lib/libgstreamer-0.10.so.0

sudo ln -s /usr/lib/x86_64-linux-gnu/libgstbase-1.0.so.0 /usr/lib/libgstbase-0.10.so.0

sudo ln -s /lib/x86_64-linux-gnu/libssl.so.1.0.0 /usr/lib/libssl.so.10

sudo ln -s /lib/x86_64-linux-gnu/libcrypto.so.1.0.0 /usr/lib/libcrypto.so.10

sudo apt-get install build-essential && sudo apt-get install linux-source && sudo apt-get install linux-headers-generic
```

### Download the MakeResolveDeb File

DaVinci Resolve comes with a SHELL file that you need to run as a root in your terminal. Sometimes it works, sometimes it doesn’t. That’s why an amazing dude called Daniel Tufvesson created a file to convert the installer into a Debian package.

Download the MakeResolveDev file and follow the instructions in his blog post, right [here](https://www.danieltufvesson.com/makeresolvedeb)!

The installation process can take a while, so, sit back, enjoy, and keep all your finger crossed.

### Known Limitations

DaVinci Resolve, and mostly the free version for Linux, doesn’t currently support MP4 file extensions with H264 codecs. So, if you like me use OBS to record your videos, you’ll need to change the settings in order to record videos that can be imported and used in DaVinci Resolve. It's best to use FFMPEG to convert the files to `.mov`.

Since also exporting doesn’t currently support H264 Codec and MP4 extension, rendering a video for YouTube or Vimeo can be a bit of pain. But fear not, as the beloved and extremely power FFMPEG library can take care of it.

After rendering your uncompressed video, run this command in your terminal to convert it into a high quality video file for streaming services.

``` bash
ffmpeg -i input.mov -vf yadif -codec:v libx264 -crf 1 -bf 2 -flags +cgop -pix_fmt yuv420p -codec:a aac -strict -2 -b:a 384k -r:a 48000 -movflags faststart output.mp4
```

If the file size is too big, you can control the quality with the `-crf 1` option, changing the number up until 25, where a higher number means lower quality.

### Unpack downloaded archives

Open up a new terminal window and go to the directory where you downloaded the Resolve installer and MakeResolveDeb and unpack both archives.

``` bash
cd Downloads
unzip DaVinci_Resolve*
tar zxvf makeresolvedeb*
```

You should now have the following files in the directory:

``` bash
DaVinci_Resolve_Studio_XX.X_Linux.run
DaVinci_Resolve_Studio_XX.X_Linux.zip
Linux_Installation_Instructions.pdf
makeresolvedeb_XX.X-X.sh.tar.gz
makeresolvedeb_XX.X-X.sh
```

### Run MakeResolveDeb

When you have unpacked the archives and have all the needed files in the directory it's time to assemble the new `*.deb` file using MakeResolveDeb. In order to start the process MakeResolveDeb needs to know if you are installing Resolve or Resolve Studio. This is done by giving the MakeResolveDeb script the argument `lite` for Resolve or `studio` for Resolve Studio. The choice you make here must match the installer archive you downloaded. Execute the MakeResolveDeb script and give the argument `lite` or `studio` to start.

For example:

``` bash
./makeresolvedeb_16.0-1.sh studio
```

or

``` bash
./makeresolvedeb_16.0-1.sh lite
```

The conversion can take a few minutes depending on computer and storage performance. If there are errors during the process it will be displayed on the terminal. A successful conversion is indicated by the last line saying `[DONE]` and the reported number of errors 0.

> **NOTE:** If you abort MakeResolveDeb you may end up in an inconsistent state that can cause errors. If error count is not zero and you have aborted MakeResolveDeb at some earlier point just re-run MakeResolveDeb again and the consistency errors should be fixed.

### Installing the Debian package

A successful conversion generates a `*.deb` file that can be installed to your system. Since the Black Magic Design installer does not provide any package dependencies you will have to make sure that you have all the required packages installed that are required by Resolve before continuing. To install Resolve you can use `dpkg`.

For example:

``` bash
sudo dpkg -i davinci-resolve-studio_16.0-1_amd64.deb
```

or

``` bash
sudo dpkg -i davinci-resolve_16.0-1_amd64.deb
```

To find out more or to see common problems, you can check the link below:

[https://www.danieltufvesson.com/makeresolvedeb](https://www.danieltufvesson.com/makeresolvedeb)

## Arch Linux

You can install Davinci Resolve from the AUR:
``` bash
sudo yay -S davinci-resolve
```