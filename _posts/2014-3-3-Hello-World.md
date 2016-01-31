Raspberry Pi running debian jessie 
 static wireless ip addresses
---

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

sudo nano /etc/network/interfaces

#-----------------------------------------
auto lo
iface lo inet loopback

iface eth0 inet static

 address 192.168.2.29
 
 netmask 255.255.255.0
 
 network 192.168.2.0
 
 broadcast 192.168.2.255
 
 gateway 192.168.2.1
 
 
allow-hotplug wlan0
auto wlan0

iface wlan0 inet static

 address 192.168.2.30
 
 netmask 255.255.255.0
 
 broadcast 192.168.2.255
 
 gateway 192.168.2.1
 

wpa-ssid "your network name"

wpa-psk "your network pass word"

#--------------------------------------------


Alt-o to save
Alt-x to quit

sudo reboot  #to restart pi

iwconfig 
This will identify if the wifi has connected to your network if it displays ESSID = "your network"

netstat -nr 
This is a good way to check which IP addresses have been configured on the PI

ifconfig 
This is another network command that works well to see your network card configuration and if it is functioning.

 
