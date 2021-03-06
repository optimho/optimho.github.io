---
layout: post
title: Raspberry Pi running debian jessie 
---
 
 **static wireless and wired ip addresses**
 
![an image alt text]({{ site.baseurl }}/images/ethernet_transparent.png "an image title")

Using a realtek EDIMAX usb to wifi adapter is a recomended wifi adapter.

  type the fillowing command:
        dmesg | more 
  to see if the raspberry has recognised your wireless adapter, if it is there then you are good to go.
  
  edit your network interfaces file.
  There is a host of ways of configuring a wireless adapter, some better and some not so good.
  I am not sure which catagory this one will fall into.

  Firstly you will need to know what your ip address of your home router is. 
  from your home computer or the computer that you are using to read this type the command:
  ipconfig 
  
  this will return a lot of information, but the important information is 
  ethernet local adapter will have IPV4 address, a default gateway, subnetmask - 192.168.2.16 , 192.168.2.1 and 255.255.255.0     respectively in my case.
  
  So I know from this data that 192.168.2.16 is an address that is used so I cannot use this address.
  The gateway address is 192.168.2.1
  The subnet mask is 255.255.255.0
  
  now you must be careful of using an address that some other device is using type arp -a into the windows command prompt
  This will show a range of addresses that are being used in your network. 
  
  The IP address that you choose must be in the same range of your wireless IP address gateway 192.168.2.(2-254)
  leave address 0 , 1 and 255 alone as they have been used already 16 too in my case.

 To see what networks are available run the following command.
 
 **sudo iwlist wlan0 scan | grep ESSID**
 
 I think that this command will work if raspbian recognises the wireless adapter, even if it is not properly configured yet,  so can   be used as a method to see if you wireless adapter is working and that the operating system recognises the adapter.  
 
 then:
 
{**sudo nano /etc/dhcpcd.conf**}

add this at the end of the file leave all other settings as they are

_interface eth0_

_static ip\_address_=192.168.2.29/24_

_static routers=192.168.2.1_

_static domain\_name\_servers_=192.168.2.1_


{ctrl-o to save}
{ctrl-x to quit}

This will make eth0 (the wired ethernet port) static

then: 

**sudo nano /etc/network/interfaces**



_auto lo_

_iface lo inet loopback_


_auto eth0_

_allow-hotplug eth0_

_iface eth0 inet manual_


_allow-hotplug wlan0_

_auto wlan0_

_iface wlan0 inet static_

_address 192.168.2.30_

_netmask 255.255.255.0_

_broadcast 192.168.2.255_

_gateway 192.168.2.1_

wpa-conf /etc/wpa\_supplicant/wpa\_supplicant.conf


{ctrl-o to save}

{ctrl-x to quit}



**then:** 

{**sudo nano /etc/wpa\_suplicant/wpa\_suplicant.conf**}



ctrl\_interface=DIR=/var/run/wpa\_supplicant GROUP=netdev

update_config=1

network={

        ssid="name of wireless ssid"
        
        psk="password or key"
        
        key_mgmt=WPA-PSK
        
}

network={

        ssid="alternative connection ssid"
        
        psk="password or key"
        
        key_mgmt=WPA-PSK
        
}


{ctrl-o to save}

{ctrl-x to quit}

sudo reboot  #to restart pi

**iwconfig** 
This will help identify that the wifi has connected to your network. If it displays ESSID = "your network" you are a winner

**netstat -nr** 
This is a good way to check which IP addresses have been configured on the PI

**ifconfig** 
This is another network command that works well to see your network card configuration and if it is functioning.

 
