
## Installation of PLEX MEDIA SERVER using repository:
* Enable *https* to avoid the error **E: The method driver /usr/lib/apt/methods/https could not be found.**
```bash
		$ sudo apt-get update && sudo apt-get install apt-transport-https binutils -y --force-yes
		```
* Get the gpg key and add uglymaoo's repository:
```bash
		$ wget -O - https://dev2day.de/pms/dev2day-pms.gpg.key | sudo apt-key add -
		$ echo "deb https://dev2day.de/pms/ jessie main" | sudo tee /etc/apt/sources.list.d/pms.list
		$ sudo apt-get update
		```
* Instalar PMS
```bash
		$ sudo apt-get install plexmediaserver -y
		$ sudo reboot
		```
### To update PMS from repository:
	Note: The repository contains the latest non-Plex Pass version. If you want the Plex Pass version you will have to build it yourself using the [manual method 3](http://www.htpcguides.com/install-plex-media-server-raspberry-pi-2-3-manually/)
```bash
$	sudo apt-get update && sudo apt-get upgrade -y
```

## Fix Plex Permission Issues

If Plex isn’t reading your drives or folders you can do one of two things, change the permissions of your external storage or change the user Plex is running as (which could mean you need to rescan for metadata).

First try adding your plex user to your regular user’s group that owns the mount point for your hard drive
```bash
$ sudo usermod -aG pi plex
```

Then add your regular user to the plex group if the plex group exists
```bash
$ sudo usermod -aG plex pi
```
Then set the permissions for the folder containing your media to 775 so the plex in the user’s group can read them
```bash
$ sudo chmod -R 775 /folder/with/media
```
If that still fails you can change the permissions where /mnt/usbstorage is your mount path for your hard drive so everybody can read, write and execute
```bash
$ sudo chmod -R 777 /mnt/usbstorage
```

**Alternatively you can change the user Plex runs as to your pi user**

To change which user Plex runs as, open this file
```bash
sudo nano /etc/default/plexmediaserver
```
Change this line, replace plex with your username you use to log on or the owner of the mounted drive, most likely pi
```bash
PLEX_MEDIA_SERVER_USER=pi

#Ctrl+X, Y and Enter to save
```
Change the owner of your Plex folders to the user you are changing
```bash
$ sudo chown -R pi:pi /var/lib/plexmediaserver
```
Then restart your Plex server
```bash
$ sudo service plexmediaserver restart
```
