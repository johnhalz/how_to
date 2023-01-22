# Scan for IP Addresses on Local Network

Using the [`arp-scan`](https://github.com/royhills/arp-scan), you can easily get a list of all of the IP addresses of the devices connected to the local network.

## Installing Utility

You can easily install `arp-scan` with the following commands:

=== "Ubuntu"

    ``` bash
    sudo apt install arp-scan
    ```

=== "macOS"

    ``` bash
    brew install arp-scan
    ```

## Using Tool

After installing, you can use the following command to output the IP addresses:

``` bash
sudo arp-scan --localnet
```

You should then get a list of all IP addresses on your local network.

--------------

For more information on `arp-scan`, you can run the following command:

``` bash
arp-scan --help
```

or checkout the documentation on the app's [github page](https://github.com/royhills/arp-scan).