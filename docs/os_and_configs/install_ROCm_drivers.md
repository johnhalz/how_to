# How to Install ROCm for AMD GPUs

> **NOTE:** You cannot have ROCm and AMDGPU-Pro driver installed at the same time, if you already have AMDGPU-Pro installed, run the command:
> 
> ```bash
> amdgpu-pro-uninstall
> ```



1. Add apt repo:
   
   ```bash
   wget -qO - http://repo.radeon.com/rocm/apt/debian/rocm.gpg.key | sudo apt-key add - && echo 'deb [arch=amd64] http://repo.radeon.com/rocm/apt/debian/ xenial main' | sudo tee /etc/apt/sources.list.d/rocm.listbash
   ```

2. ```bash
   sudo apt update && sudo apt install rocm-dkms
   ```

3. To access the GPU, you must be a user in the video group. Ensure your user account is a member of the video group prior to using ROCm. To add your user to the video group, use the following command for the sudo password:
   ```bash
   sudo usermod -a -G video $LOGNAME
   ```

4. By default, add any future users to the video group. Run the following command to add users to the video group:
   ```bash
   echo 'ADD_EXTRA_GROUPS=1' | sudo tee -a /etc/adduser.conf
   
   echo 'EXTRA_GROUPS=video' | sudo tee -a /etc/adduser.conf
   ```

5. Restart the system
   ```bash
   reboot
   ```

6. After restarting the system, run the following commands to verify that the ROCm installation is successful. If you see your GPUs listed by both commands, the installation is considered successful.
   ```bash
   /opt/rocm/bin/rocminfo
   
   /opt/rocm/opencl/bin/x86_64/clinfo
   ```

7. To run the ROCm programs more efficiently, add the ROCm binaries in your `PATH`.
   ```bash
   echo 'export PATH=$PATH:/opt/rocm/bin:/opt/rocm/profiler/bin:/opt/rocm/opencl/bin/x86_64' | sudo tee -a /etc/profile.d/rocm.sh
   ```

X. To remove ROCm:
```bash
sudo apt autoremove rocm-dkms

sudo apt autoremove rocm-libs miopen-hip cxlactivitylogger
```
