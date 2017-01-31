## configuration of Rpi 3 as Router/AP
---
* In case of use dongle TP-link TW725N, follow the link
https://www.raspberrypi.org/forums/viewtopic.php?p=462982
---
* Installing WIFI and Bluethoot drivers
```bash
$ apt-get install firmware-brcm80211 #Driver wifi for RPi3
$ apt-get install pi-bluetooth # Optional in case that you want to activate bluethoot
# $ apt-get install wpasupplicant #In case that you want to connect to external wifi
```
* Install tools for WIFI
```bash
	$ sudo apt-get install iw
	$ sudo iw list
```
* Remove WPA Supplicant(if it was installed):
```bash
	$ sudo apt-get purge wpasupplicant
```
* Instalar dhcp server and hostapd para hosting access point
```bash
	$ sudo apt-get install isc-dhcp-server
	$ sudo apt-get install hostapd
```
* Modify dhcpd.conf and add the following lines at the end:
```bash
	$ sudo nano /etc/dhcp/dhcpd.conf
	 subnet 10.10.0.0 netmask 255.255.255.0 {
	 range 10.10.0.25 10.10.0.50;
	 option domain-name-servers 8.8.4.4;
	 option routers 10.10.0.1;
	 interface wlan0;
	 }
	#CTRL+x, Y, Enter to save the file
```
* Create hostapd config file and add the following lines:
```bash
	$ sudo nano /etc/hostapd/hostapd.conf
	 interface=wlan0
	 driver=nl80211
	 ssid={AP name}
	 hw_mode=g
	 channel=11
	 wpa=1
	 wpa_passphrase={wifi password}
	 wpa_key_mgmt=WPA-PSK
	 wpa_pairwise=TKIP CCMP
	 wpa_ptk_rekey=600
	 macaddr_acl=0
	# CTRL+x, Y, Enter to save the file
	$ sudo reboot
```
* To test:
```bash
	$ sudo ifconfig wlan0 10.10.0.1
	$ sudo /etc/init.d/isc-dhcp-server restart
	$ sudo hostapd -d /etc/hostapd/hostapd.conf #-d parameter is to enable debug and see errors
```
* Make the changes persistent editing *interfaces* file:
```bash
	$ sudo nano /etc/network/interfaces
	 auto wlan0
	 iface wlan0 inet static
	 address 10.10.0.1
	 netmask 255.255.255.0
	#CTRL+x, Y, Enter to save the file
```
* Edit */etc/rc.local* file to routing to eth0 interface with internet connection:
```bash
	$ sudo nano /etc/rc.local
	 exec 2> /tmp/rc.local.log      # send stderr from rc.local to a log file
	 exec 1>&2                      # send stdout to the same log file
	 set -x                         # tell sh to display commands before execution
	#CTRL+x, Y, Enter to save the file
```
* Enable port forwarding
```bash
	$ hostapd -B /etc/hostapd/hostapd.conf
	$ iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
	$ sudo nano /etc/sysctl.conf
	 net.ipv4.ip_forward = 1
	#CTRL+x, Y, Enter to save the file
	$ sudo reboot
```
