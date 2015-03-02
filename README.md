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

**--> If you want anonymity disable cookies, scripts and ads 
and run your web browser in private mode.**

** --> Don't use your private log in and password on public websites! 
Don't trust anything. Be aware!**

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

After some seconds you will notice the boot loader on the screen. Another reboot helps sometimes if it fails.
 
### Configure your Onion Pi!

1.  **Expand the file system** and **change the user password**

2.  Set the **international options**: **timezone** and **keyboard layout**

3.  **Advanced Options**: **Change** the **host name** and **enable ssh**

It's time for another *reboot*!

The log in is **pi** and the password is in your mind.

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

If you miss a *wlan0*:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
shutdown -h now
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Plug in a USB WiFi module and start the Pi. Make sure that you have *wlan0* working before continuing...

How to use it?
--------------
