## Format and mount USB drive (EXT4 version)
```bash
	$ sudo fdisk -l
	$ sudo mkfs.ext4 /dev/sda1 -L MINIBIAN
	$ sudo mkdir /mnt/usbstorage
	$ sudo chown -R pi:pi /mnt/usbstorage
	$ sudo chmod -R 775 /mnt/usbstorage
	$ sudo setfacl -Rdm g:pi:rwx /mnt/usbstorage
	$ sudo setfacl -Rm g:pi:rwx /mnt/usbstorage
	$ sudo blkid
	$ sudo mount -o uid=pi,gid=pi /dev/sda1 /mnt/usbstorage

	*** In case of errors ***
	$ sudo mount -t uid=pi,gid=pi /dev/sda1 /mnt/usbstorage
	$ sudo ls -l /dev/disk/by-uuid/
	$ sudo nano /etc/fstab
	  UUID=ccc458b0-e09d-4af6-9ef9-43caf9699008    /mnt/usbstorage    ext4   nofail,defaults    0   0
	$ sudo mount -a
	$ sudo reboot
```
