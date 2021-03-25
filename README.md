# Aptus Pi - Assembly Instructions

### Hardware
* Raspberry Pi 4 Model B board: https://www.raspberrypi.org/products/raspberry-pi-4-model-b/
* Class 10 Micro SD card: https://www.amazon.ca/b/ref=dp_bc_aui_C_5?ie=UTF8&node=3381334011
* Case: https://thepihut.com/collections/raspberry-pi-cases
* Power supply: https://www.raspberrypi.org/products/type-c-power-supply/
* Micro SD card reader: https://www.amazon.ca/Anker-Reader-RS-MMC-Support-Warranty/dp/B006T9B6R2 
* Power bank(optional): https://www.powerbankexpert.com/best-raspberry-pi-power-bank/

### Setting up Raspberry Pi
Source: https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up
1. Download latest Raspberry Pi OS lite: https://www.raspberrypi.org/software/operating-systems/
1. Write to a micro SD card with one of the following tools:
   1. Raspberry Pi Imager:https://www.raspberrypi.org/software/
   1. Win32Diskimager: https://sourceforge.net/projects/win32diskimager/
   1. Etcher: https://www.balena.io/etcher/
1. Insert the micro SD card and boot Raspberry Pi.

### Create a SUDO user account
Source: https://linuxize.com/post/how-to-create-a-sudo-user-on-debian/

Login Raspberry Pi with default username and password: pi/raspberry

Execute the following commands in a terminal:
```console
sudo su
adduser aptus
usermod -aG sudo aptus
```
### Enable SSH
Source: https://www.raspberrypi.org/documentation/remote-access/ssh/

Launch raspi-config by entering the following command in a terminal:
```console
sudo raspi-config
```
* Select Interfacing Options
* Navigate to and select SSH
* Choose Yes
* Select Ok
* Choose Finish

Alternatively, use systemctl to start the service
```console
sudo systemctl enable ssh
sudo systemctl start ssh
```
### Web Administration Portal(optional)
Source: https://pimylifeup.com/raspberry-pi-webmin/

### WiFi Hotspot and DNS
Source: https://github.com/RaspAP/raspap-webgui

Set the WiFi country in raspi-config's Localisation Options
```console
sudo raspi-config
```

Connect Aptus Pi to Internet via a wired or wirless connection

Install RaspAP from a terminal
```console
curl -sL https://install.raspap.com | bash
```

Complete the installation by rebooting the device.

Connect to the device via wireless network:
```
SSID: raspi-webgui
password: ChangeMe
```

Access Raspap dashboard by entering the following URL in a browser: 

Default credentials for Raspap dashboard:
```
Username: admin
Password: secret
```
Configure the wireless access point network as follows:
```
IP address: 192.168.169.2
Username: aptus
Password: ******
DHCP range: 192.168.169.10 to 192.168.169.250
SSID: Aptus-PI
Password: ********
http port: 81
```

Reboot the device and verify the changes.

### Captive Portal (optional)
Source:https://www.maketecheasier.com/turn-raspberry-pi-captive-portal-wi%E2%80%90fi-access-point/

### LAMP stack
Source: https://projects.raspberrypi.org/en/projects/lamp-web-server-with-wordpress

#### Apache2
```console
sudo apt-get install apache2 -y
sudo a2enmod rewrite
sudo a2dismod reqtimeout
sudo systemctl restart apache2
```
Enable htaccess by editing Apache2 configuration

Default configuration file location: /etc/apache2/sites-available/000-default.conf
```bash
<VirtualHost *:80>
    <Directory /var/www/html>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
    </Directory>

    . . .
</VirtualHost>
```
#### or Nginx (optional)

#### PHP 7.3
```console
$sudo apt install ca-certificates apt-transport-https 
$wget -q https://packages.sury.org/php/apt.gpg -O- | sudo apt-key add -
$echo "deb https://packages.sury.org/php/ stretch main" | sudo tee /etc/apt/sources.list.d/php.list
$sudo apt update
$sudo apt install php7.3
$sudo apt install graphviz aspell ghostscript clamav php7.3-pspell php7.3-curl php7.3-gd php7.3-intl php7.3-mysql php7.3-xml php7.3-xmlrpc php7.3-ldap php7.3-zip php7.3-soap php7.3-mbstring
$sudo apt install sqlite3 php7.3-sqlite3
```
/etc/apache2/mods-enabled/dir.conf
```bash
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
/etc/php/7.3/apache2/php.ini
```bash
post_max_size = 2000M
upload_max_size = 2000M
max_input_time = 3600
max_execution_time = 3600
memory_limit = 256M
```



