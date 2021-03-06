# Instructions for installing OpenWRT and TOR on the wt3020 (Nexx) router

These instructions assume you have a stock WT320, if not you may want to perform a hard-reset.

## Step 00: Update your device
Before you start, you need to be on a version of the stock firmware that supports https requests with wget, and also has access to the wtd_write command. 

First option: login to the web admin portal for the router and update the firmare.

Second choice: download the stock nexx firmware from this repo and upload it in the same web admin portal for the router.

Third choice, download the latest binary from: http://www.nexx.com.cn/index.php/home/page?p=222, and upload it using the same web admin portal for the router.

## Step 01: Find latest OpenWRT binary

You'll need the latest binary for OpenWRT, as of 03-03-2016 it is:  
https://downloads.openwrt.org/chaos_calmer/15.05/ramips/mt7620/openwrt-15.05-ramips-mt7620-wt3020-8M-squashfs-factory.bin  
-Replace 15.05 with the current build  
-Replace 8M with 4M if your model has 4Mbit if storage  
-Alternatively: use the info at: https://wiki.openwrt.org/toh/nexx/wt3020  

## Step 02: Ethernet Setup

Connect LAN Ethernet to your computer, or any network without DHCP

Connect WAN Ethernet to some sort of internet, a LAN with DHCP is fine

## Step 03: Replace stock firmware with OpenWrt

```
telnet 192.168.8.1
User: nexxadmin
Password: y1n2inc.com0755
```
Note: the password appears to be a string of the developers(?) domain, + unix permission 0755, not a very good password.

After login, you should see somthing like: 

```
BusyBox v1.12.1 (2015-02-05 18:04:51 HKT) built-in shell (ash)
```

Download and write firmware to flash:
```
cd /tmp
wget https://downloads.openwrt.org/chaos_calmer/15.05/ramips/mt7620/openwrt-15.05-ramips-mt7620-wt3020-8M-squashfs-factory.bin
wtd_write -r write openwrt-15.05-ramips-mt7620-wt3020-8M-squashfs-sysupgrade.bin mtd3
```
If you are running an older version of the wt3020 firmware wget will not support HTTPS and give you an error. Also, the wtd_write command will not not be avilable. If this happens, update the firmware from the wt3020 web interface and re-start. (See step 00)

Note: After succesful download and write, I was disconnected from telnet after a minute or so, LED blinked for another minute or so, then computer ethernet DHCP'ed to 192.168.1.1.

If you're still able to connect to 192.168.8.1, something went wrong! Verify that flashing was successful, or retry flash with the binary attached to this repo.

## Step 04: Set password and SSH access

Connect to: http://192.168.1.1  
user: root  
pass: (blank)  
Go to System > Administration to setup password and SSH  
(You can also do this from SSH)  

## Step 05: Install TOR (optional)

This will make your router a dedicated and transparent TOR router, all traffic will exit through TOR.

Connect to SSH and run: 
```
wget -qO - http://onionwrt.us.to/install | sh -
```

Confirm by connecting to the OnionWRT wifi, then visiting https://check.torproject.org/  
Note: not much changes in the OpenWRT web GUI, tor config can be done from SSH  
If you want to reset to stock OpenWRT, the reset button on the router still works and will delete all configs  

## Notes: Updating OpenWRT

Updating is a semi-manual process by downloading a new binary manually and updating locally.  
Specific instructions at: https://wiki.openwrt.org/doc/howto/generic.sysupgrade

## Notes: More OpenWRT info at

Basic Config: https://wiki.openwrt.org/doc/howto/basic.config  
Share USB storage: https://wiki.openwrt.org/doc/recipes/usb-storage-samba-webinterface  
Upgrading: https://wiki.openwrt.org/doc/howto/generic.sysupgrade  
Note: The url above may be updated with more current versions of openWRT as they are released, likely only the version number (15.05) will be changed  

## Notes: Source Links

More about openWrt: https://wiki.openwrt.org/toh/nexx/wt3020  
Notes on similar http://www.securityskeptic.com/2016/01/how-to-turn-a-nexx-wt3020-router-into-a-tor-router.html  
https://commotionwireless.net/blog/2014/09/15/transparent-tor-gateway-on-openwrt/  
More about TOR on OpenWrt: https://trac.torproject.org/projects/tor/wiki/doc/OpenWRT 
Stock firmware images: http://www.nexx.com.cn/index.php/home/page?p=222
