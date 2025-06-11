# Stop macOS from Creating `.DS_Store` Files
## Introduction
macOS creates a `.DS_Store` file in every folder you view in Finder. This file stores metadata about that folder’s contents as well as user customizations for things like view type and icon size.

These `.DS_Store` files are hidden from you in macOS so they won’t clutter up your folder views. But in mixed-OS environments, the `.DS_Store` files can become a problem. That’s because your Mac creates these files even for shared network locations. So if you’re sharing a NAS at your office with people using Windows PCs, they may suddenly see a bunch of `.DS_Store` files littering the shared directories.

## Prevent `.DS_Store` Creation
To configure your Mac to not create `.DS_Store` files on shared network drives, log into macOS, launch the Terminal, and enter the following command:

``` bash
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

Once you’ve executed the command, save any open work and log out of your macOS user account. When you log back in, reconnect to your shared network drives. Existing `.DS_Store` files may still be present and will need to be manually deleted, but your Mac won’t create any new `.DS_Store` files as you browse the shared directories going forward.

## Re-Enable `.DS_Store` Creation

If you’ve used the command above to disable the creation of `.DS_Store` files on shared network drives, you can re-enable the creation of these files with the following command:

``` bash
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool FALSE
```

As before, make sure to log out and then reconnect your shared network drives after running the command.