# Instructions for installing OpenWRT and TOR on the wt3020 NexxRouter

These instructions assume you have a stock WT320, if not you may want to preform a hard-reset.

## Step one - Find latest OpenWRT binary
First you'll need the latest binary for OpenWRT, as of 03-03-2016 it is:
https://downloads.openwrt.org/chaos_calmer/15.05/ramips/mt7620/openwrt-15.05-ramips-mt7620-wt3020-8M-squashfs-factory.bin
-Replace 15.05 with the current build
-Replace 8M with 4M if your model has 4Mbit if storage
-or- Use the info at: https://wiki.openwrt.org/toh/nexx/wt3020

## Ethernet Setup
Connect LAN ethernet to your computer
Connect WAN ethernet to some sort of internet

## Update Firmware:
```
telnet 192.168.8.1
User: nexxadmin
Password: y1n2inc.com0755
```

After login, you should see somthing like: 
```
BusyBox v1.12.1 (2015-02-05 18:04:51 HKT) built-in shell (ash)
```

Run commands:
```
cd /tmp
wget https://downloads.openwrt.org/chaos_calmer/15.05/ramips/mt7620/openwrt-15.05-ramips-mt7620-wt3020-8M-squashfs-factory.bin
wtd_write -r write openwrt-15.05-ramips-mt7620-wt3020-8M-squashfs-sysupgrade.bin mtd3
```

Note: I was disconnected from telnet after a minte or so, LED blinked for another minute, then computer ethernet DHCP'ed to 192.168.1.1
If you're still able to connect to 192.168.8.1, something went wrong

## Set password and SSH access
Connect to: http://192.168.1.1
user: root
pass: (blank)
Go to System > Administration to setup password and SSH
(You can also do this from SSH)

## Install TOR (if you want)
Connect to SSH and run: 
wget -qO - http://onionwrt.us.to/install | sh -


Confirm by connecting to the OnionWRT wifi, then visiting https://check.torproject.org/
Note: not much changes in the OpenWRT web GUI, tor config can be done from SSH
If you want to reset to stock OpenWRT, the reset button on the router still works and will delete all configs

## More OpenWRT info at: 
Basic Config: https://wiki.openwrt.org/doc/howto/basic.config
Share USB storage: https://wiki.openwrt.org/doc/recipes/usb-storage-samba-webinterface
Upgrading: https://wiki.openwrt.org/doc/howto/generic.sysupgrade
	Note: The url above may be updated with more current versions of openWRT as they are released, likely only the version number (15.05) will be changed

## Source Links:
https://wiki.openwrt.org/toh/nexx/wt3020 - can get new wget path
http://www.securityskeptic.com/2016/01/how-to-turn-a-nexx-wt3020-router-into-a-tor-router.html
https://commotionwireless.net/blog/2014/09/15/transparent-tor-gateway-on-openwrt/
https://trac.torproject.org/projects/tor/wiki/doc/OpenWRT
