# DEPRECATED! Please use wms-linux-framework instead
## https://github.com/FZsolter-WAGO/wms-linux-framework

# WattsON Energy Linux framework installer

## Only Debian 10/11 and Ubuntu 22.04 are supported!

Anything can have a mascot

![snek](snek.png)

### Usage

0) Login as root via SSH, or in case of WAGO 752-9xxx Edge Computer via browser at https://<server_ip>:9090/
1) Configure network settings to access the Internet

   It is recommended to use a br0 bridge interface with a static MAC address of the primary physical interface (br0=X1+X2 with the MAC of X1)
2) Remove any Docker installation (the docker0 network interface is constantly changing its MAC address, so the license key will change every time the server is restarted)

   Reboot when the uninstallation is complete
   ```
   apt purge docker* -y && apt autoremove -y && reboot
   ```
3) Update the system, and install curl
   ```
   apt update && apt full-upgrade -y && apt install curl -y
   ```
4) Upload the desired "wattson_x.x.x.x.tar.gz" package

   It is recommended to upload the package to /root/wattson_x.x.x.x.tar.gz using SFTP (with FileZilla for example)
   
   Otherwise navigate to the file with "cd /path/to/package/" in the terminal

5) Set the timezone of the server
   ```
   /usr/sbin/dpkg-reconfigure tzdata
   ```
   Change the system language (optional, en_US.UTF-8 is recommended)
   ```
   /usr/sbin/dpkg-reconfigure locales
   ```
6) Run the installer script
   ```
   curl -s https://raw.githubusercontent.com/FZsolter-WAGO/wattson-linux-framework/main/bin/install.sh | bash
   ```
   
   If something went wrong, or the password is lost then execute this line again. It will start over the non-framework related part of the installation (dropping the databases and user, cleaning /var/www)
7) Continue with WattsON's normal self-host install process
8) Run the post config script
   ```
   wattson_post_config
   ```
9) Access the management site at http://<server_ip>.<management_port>/

### Tip
This should be used only once during the initial setup of the system, since the recommended way of installing WattsON Energy on Linux is using a fresh new minimized OS install on a dedicated hardware only for this one purpose.
