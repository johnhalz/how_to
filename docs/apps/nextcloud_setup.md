# How to set up Nextcloud

## Preparing drives for storage

## Installing and Setting up Nextcloud On Ubuntu

!!! warning
    This document is not completed! Instructions need to be revised
    > TODO: Add instructions for Arch Linux

You can install nextcloud server with snapd:

``` bash
sudo apt update
sudo apt install snapd
sudo snap refresh
sudo snap install nextcloud
```

Assuming that there are no other web apps running, you can type in the IP address of the server onto a web browser and then you can set up the admin account.

## DuckDNS

DuckDNS is a free DDNS that you could use for Nextcloud. You can find out how to install it on a Linux server and on a router [here](http://www.duckdns.org/install.jsp).

## Turn Off and On Again
You can disable nextcloud you can type in the command:

``` bash
sudo snap disable nextcloud
```

An to enable again you can type in the obvious command:
``` bash
sudo snap enable nextcloud
```

## How to Access Nextcloud when Outside of Local Network
To do this, we have to setup a dynamic DNS (domain name system).

## More Info

Forum link: [Access Nextcloud outside of LAN - support - Nextcloud community](https://help.nextcloud.com/t/access-nextcloud-outside-of-lan/19570)
