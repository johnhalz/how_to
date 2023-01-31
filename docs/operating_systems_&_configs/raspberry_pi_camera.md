# Get Camera Working on Raspberry Pi

The built-in MIPI connector for cameras on the raspberry pi might give users some greif, os this is a short guide of things to try.

## Add user to video group

This is really simple and stupid, but it's not enabled by default. Enter in the command below and then reboot for the changes to take effect:

``` bash
sudo usermod -a -G video $USER
```

## Video Drivers

To ensure that you have the correct video drivers for linux, run the following command:

``` bash
sudo modprobe bcm2835-v4l2 
```

## Install DietPi

Dietpi is essentially a stripped down and modded version of Raspbian, but it's performance has been proven to be quite good after personal use.

Link: <https://dietpi.com>