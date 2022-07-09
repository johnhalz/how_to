# How to Setup Plex Server

Plex can be used as a personal media server to host movies, TV shows, podcasts, and more.

1. Install Plex Media Server onto a server

    === "Ubuntu/Debian"
        Add the Plex APT repository to your system and import the repository’s GPG key:

        ``` bash
        curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
        echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
        ```
        
        Once the repository is enabled, update the apt package list and install the latest server version:
        
        ``` bash
        sudo apt update
        sudo apt install plexmediaserver
        ```

    === "Arch"
        Install from the AUR:
        ``` bash
        sudo yay -Syu
        sudo yay -S plex-media-server
        ```

2. Check the status of the server:
    ``` bash
    sudo systemctl status plexmediaserver
    ```

    If the server is running, you should see something similar to:
    ``` bash title="Output"
    ● plexmediaserver.service - Plex Media Server
        Loaded: loaded (/lib/systemd/system/plexmediaserver.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2021-06-17 19:36:33 UTC; 23min ago
    ```

3. Adjusting the firewall
    
    !!! info
        If you do not have a firewall running on your system, skip this section.

    Now that Plex is installed and running on your server, you need to make sure the server firewall is configured to allow traffic on the Plex-specific ports.

    If you are using UFW to manage your firewall, the easiest option is to create a UFW application profile:
    ``` bash
    sudo vim /etc/ufw/applications.d/plexmediaserver
    ```

    ``` bash title="/etc/ufw/applications.d/plexmediaserver"
    [plexmediaserver]
    title=Plex Media Server (Standard)
    description=The Plex Media Server
    ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp

    [plexmediaserver-dlna]
    title=Plex Media Server (DLNA)
    description=The Plex Media Server (additional DLNA capability only)
    ports=1900/udp|32469/tcp

    [plexmediaserver-all]
    title=Plex Media Server (Standard + DLNA)
    description=The Plex Media Server (with additional DLNA capability)
    ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp|1900/udp|32469/tcp
    ```

    Save the file and update profiles list:
    ``` bash
    sudo ufw app update plexmediaserver
    ```

    Apply the new firewall rules:
    ``` bash
    sudo ufw allow plexmediaserver-all
    ```

    Finally, check if the new firewall rules are applied successfully with:
    ``` bash
    sudo ufw status verbose
    ```

    ``` bash title="Output"
    Status: active
    Logging: on (low)
    Default: deny (incoming), allow (outgoing), disabled (routed)
    New profiles: skip

    To                                      Action      From
    --                                      ------      ----
    22/tcp                                  ALLOW IN    Anywhere
    32400/tcp (plexmediaserver-all)         ALLOW IN    Anywhere
    3005/tcp (plexmediaserver-all)          ALLOW IN    Anywhere
    5353/udp (plexmediaserver-all)          ALLOW IN    Anywhere
    8324/tcp (plexmediaserver-all)          ALLOW IN    Anywhere
    32410:32414/udp (plexmediaserver-all)   ALLOW IN    Anywhere
    1900/udp (plexmediaserver-all)          ALLOW IN    Anywhere
    32469/tcp (plexmediaserver-all)         ALLOW IN    Anywhere
    ```

4. Configuring Plex Media Server
    Before starting the Plex setup wizard, you can first create the directories that will store the Plex media files:
    ``` bash
    sudo mkdir -p /media/{movies, tv_shows}
    ```

    The Plex Media Server runs as the user `plex`, which must have read and execute permissions to the media files and directories. To set the correct ownership , enter the following command:
    ``` bash
    sudo chown -R plex: /media
    ```

    !!! info
        You can choose any location to store the media files; just make sure you set the correct permissions.

    === "General Method"
        You can now proceed with the server configuration. Open your browser, type `ip.address.of.server:32400/web`, and you will be redirected to the plex website.

    === "If general method doesn't work"
        First create an SSH tunnel (setup can only be done from `localhost`):
        ``` bash
        ssh ip.address.of.server -L 8888:localhost:32400
        ```

        and then browse to `ip.address.of.server:8888/web`.

    You will then need to sign in or create an account, navigate to server settings, and make the server accessible from remote devices. You can then open your browser on another device and navigate to the webpage [https://app.plex.tv/desktop/](https://app.plex.tv/desktop/).
