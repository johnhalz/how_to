# Install Etcher on Ubuntu with apt

1. Add Etcher debian repository:
   
   In command terminal fire the below command to add the Etcher repository:
   
   ```bash
   echo "deb https://deb.etcher.io stable etcher" | sudo tee /etc/apt/sources.list.d/balena-etcher.list
   ```
   
   Import the GPG key:
   
   ```bash
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 379CE192D401AB61
   ```

2. Update the system:
   
   ```bash
   sudo apt-get update
   ```

3. Install Etcher on Ubuntu:
   
   ```bash
   sudo apt-get install balena-etcher-electron
   ```



X. Uninstall Etcher:

```bash
sudo apt-get remove balena-etcher-electron
sudo rm /etc/apt/sources.list.d/balena-etcher.list
sudo apt-get update
```
