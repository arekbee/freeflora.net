Onion Pi ( Raspberry Pi + Tor )
===============================

### wireless access point distribution

Freeflora.net will help you to run an anonymous wireless access point (AP) on a Raspberry Pi.

Tor (The Onion Router) is the largest deployed anonymity network to date.

It's simple: If you combine a Raspberry Pi with Tor then you get an **Onion Pi**.

-   https://www.torproject.org
-   https://metrics.torproject.org
-   http://www.raspberrypi.org

Please help me understand how Onion Pi works?
---------------------------------------------

Let's take a look at a private network and a typical use case at home:

1.  A device connects to a wireless access point (**AP**) - physically a router

2.  The router operates the private network, called **LAN** (Local Area Network)

3.  Networks at home are connected to **WAN** (Wide Area Network)

Now, the device navigates to a website and will be soon connected to a web server.

The information will pass the **AP**, through the **LAN** and will go to the **WAN** and back.

### *Welcome to the internet!*

Tor will use a tunnel instead and will encrypt and send your data to other Tor nodes to provide anonymity.

So far, so good if you just read the information from the internet and don't share cookies or other private information.

In reality all websites have user identification and that's a serious problem for anonymity.

That means it's not private at all! **Yes, Onion Pi has these problems, too!**

**If you want anonymity disable cookies, scripts and ads and run your web browser in private mode.**

**Don't use your private log in and password on public websites! Don't trust anything. Be aware!**

*For example:
If you log in at some social network site with your private account, seriously, sell your Onion Pi!*

What do I need for an Onion Pi?
-------------------------------

Lets begin, sure a **Raspberry Pi**! There are several Raspberry Pi packages to
buy online.

If you get one, please check if you have all that is required to make an onion
of it:

### Requirements

-   Raspberry Pi (2) / any model

-   SD Card \> 2GB / microSD for Pi 2

-   MircoUSB power supply cable

-   Ethernet cable

-   USB WiFi module (check Pi compatibility and AP capability)

-   A nice case for your Pi (check PI model)
-   USB keyboard for **setup only**
-   HDMI cable for **setup only**

How to install an Onion Pi?
---------------------------

The heart of the Onion Pi is the operating system. We use **Raspbian**.

Raspbian is a Linux operating system based on Debian and optimized for the Raspberry Pi hardware: https://www.raspbian.org

### Raspbian

1.  Download the latest Raspbian image from:

    -  http://downloads.raspberrypi.org/raspbian_latest

2.  If you are using Windows, this tool can help you to copy the image to your
    SD card:

    -  Win32DiskImager: https://sourceforge.net/projects/win32diskimager

3.  Here is a complete installation guide for Mac OS, Linux and Windows:

    -  http://www.raspberrypi.org/documentation/installation/installing-images/README.md

And of course here is the standard setup:

-   Put the SD card (Raspbian image) into your Pi
-   Connect the Pi with the Ethernet cable to your router
-   Plug in the USB keyboard and the USB WiFi module
-   Don't forget the HDMI cable for your screen and power on the Pi

After some seconds you will notice the boot finished on the screen.
 
### Configure Raspbian!

1.  **Expand the file system** and **change the user password**

2.  Set the **international options**: **time zone** and **keyboard layout**

3.  **Advanced Options**: **Change** the **host name** and **enable ssh**

Finish it with TAB. It's time for another *reboot*!

The log in is **pi** and the password is in your mind.

### Connect with ssh

If you enabled *ssh* you will find the IP address on the end of the boot.

You can always check the IP address when you are logged in, find *eth0*:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo ifconfig -a
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Reboot your Onion Pi

Sometimes you need to reboot your Pi:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo reboot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Update your Onion Pi

It's always good to have the latest libraries on your system:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Keep your Onion Pi up to date. Do updates frequently!

### USB WiFi module check

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo ifconfig -a
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you miss a *wlan0*, shut down the Pi first:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo shutdown -h now
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plug in a USB WiFi module and start the Pi. 

Make sure that you have *wlan0* working before continuing...

### Install a DHCP server

A DHCP (Dynamic Host Configuration Protocol) server will manage the IP addresses of your devices when you connect to the AP.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo apt-get install hostapd isc-dhcp-server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Don't worry if you get a fail on starting the DHCP server. We need to configure it first:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/dhcp/dhcpd.conf 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add a *#* before, so we will comment it in the configuration and ignore the lines:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#option domain-name "example.org";
#option domain-name-servers ns1.example.org, ns2.example.org;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To activate a line in the configuration, remove the *#* in the line:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
authoritative;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

And at the end of the file add these lines (you can copy and paste it in your ssh client):

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
subnet 192.168.42.0 netmask 255.255.255.0 {
    range 192.168.42.10 192.168.42.50;
    option broadcast-address 192.168.42.255;
    option routers 192.168.42.1;
    default-lease-time 600;
    max-lease-time 7200;
    option domain-name "local";
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    }
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To save a file in *nano*, use this combo: **CTRL-X -> Y + ENTER**

So, save the file and set the DCHP server interfces to the USB WiFi module:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/default/isc-dhcp-server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Write *wlan0* at the end of the file:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
INTERFACES="wlan0"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Save it. We finished the DHCP server installation and configuration. Congrats!

### Fix IP address for the AP 

The wireless access point needs a static IP address, first shut down the interface:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo ifdown wlan0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Change the settings in the interface configuration:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/network/interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*After* the line **allow hotplug wlan0** add these lines and remove the rest:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
iface wlan0 inet static
address 192.168.42.1
netmask 255.255.255.0 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Save it and start the interface again:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo ifconfig wlan0 192.168.42.1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Setup the AP

Create a new configuration file for the access point:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/hostapd/hostapd.conf
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add these lines that describe the access point settings:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
interface=wlan0
driver=rtl871xdrv
ssid=freeflora.net
hw_mode=g
channel=6
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=YOUR_AP_PASSWORD
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are using another driver, change it for example: *driver=nl80211*

Change the **wpa_passphrase** setting and keep it in your mind. 

Adjust other settings if you know what you are doing.

Don't forget to set the new configuration file for the access point:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/default/hostapd
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Uncomment the line and add the path to the configuration file:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
DAEMON_CONF="/etc/hostapd/hostapd.conf"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Save it.

### IP forward & IP tables

First, IP forward is needed, open this file:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/sysctl.conf
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Uncomment this setting and enable IP forward:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
net.ipv4.ip_forward=1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Save the file. Change the following IP tables: 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can check the current IP tables with:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo iptables -t nat -S
sudo iptables -S
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next, save the new IP tables:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Open the network interface configuration again:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo nano /etc/network/interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

And add this new line at the bottom to load the saved tables automatically on next boot:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
up iptables-restore < /etc/iptables.ipv4.nat
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Install adafruit hostapd

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
wget http://www.adafruit.com/downloads/adafruit_hostapd.zip 
unzip adafruit_hostapd.zip 
sudo mv /usr/sbin/hostapd /usr/sbin/hostapd.ORIG 
sudo mv hostapd /usr/sbin
sudo chmod 755 /usr/sbin/hostapd
sudo /usr/sbin/hostapd /etc/hostapd/hostapd.conf
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check if you find the AP running. Use **CTRL + C** to stop the access point.

Now it's time for another reboot. 

### Checkpoint! Finish AP!

You can check the status of the services with:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo service hostapd status
sudo service isc-dhcp-server status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

And if you need to start the services manually:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo service hostapd start
sudo service isc-dhcp-server start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run the following commands to finish it:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo update-rc.d hostapd enable
sudo update-rc.d isc-dhcp-server enable
sudo mv /usr/share/dbus-1/system-services/fi.epitest.hostap.WPASupplicant.service ~/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Reboot! Your access point is now ready! Next: **Tor!**

You can always check the system log for errors:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
tail -f /var/log/syslog
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
