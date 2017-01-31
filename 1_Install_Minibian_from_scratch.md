## Installing minibian from scratch
* Format the SD card from console or using **GPARTED**
  * Using console:
    * Use _df_ to find the location of the SD card
    ```bash
    $ df
      Filesystem     1K-blocks      Used Available Use% Mounted on
      udev             3984952         0   3984952   0% /dev
      tmpfs             801824      9948    791876   2% /run
      /dev/nvme0n1p6 162060828 122155480  31650016  80% /
      tmpfs            4009108      3304   4005804   1% /dev/shm
      tmpfs               5120         4      5116   1% /run/lock
      tmpfs            4009108         0   4009108   0% /sys/fs/cgroup
      /dev/nvme0n1p2    262144     43820    218324  17% /boot/efi
      /dev/nvme0n1p4  74295292  51744792  22550500  70% /mnt/56C4E9A5C4E98797
      cgmfs                100         0       100   0% /run/cgmanager/fs
      tmpfs             801824        12    801812   1% /run/user/121
      tmpfs             801824        36    801788   1% /run/user/1000
      /dev/sdb2         801824        36    801788   1% /media/SD8
    ```
    * Once that you know the logical location of the SD card, unmount it
    ```bash
    $ sudo umount /media/SD8
    ```
    * Once that the SD card was already unmounted, proceed to format the SD card in fat32
    ```bash
    $ mkdosfs /dev/sdb2 -fat32
    ```
* Burning Minibian to SD card
  * Download minibian last version from [Minibian Website] (https://minibianpi.wordpress.com/)
  * Unzip the file downloaded:
  ```bash
		>$ tar -xvzf 2016-03-12-jessie-minibian.tar.gz
		> x 2016-03-12-jessie-minibian.img
		>$
  ```
  * Based on some error burning the .img file using Linux console, I used Etcher
    * Etcher can be downloaded from [Etcher Website](https://etcher.io/)
    * From Etcher:
      1. Select the image to burning
      2. Select the SD card
      3. Burn the image
  * Once the image is already burned, insert the SD card on RPi3, plug Eth cable, and power cable.

## Admin Tasks
* From the router, find the current ip of the RPi3.
* Once that we got the IP, proceed to login using **SSH**
```bash
$ ssh root@{device IP}
 root@192.168.1.10s password: raspberry
 The programs included with the Debian GNU/Linux system are free software;
 the exact distribution terms \for each program are described \in the
 individual files \in /usr/share/doc/*/copyright.

 Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
 permitted by applicable law.
 Last login: Fri May 27 20:20:19 2016 from 10.0.0.20
 root@minibian:~#
```
* Expand the Filesystem to use the 100% of the free space on SD card
  * Use fdisk to edit the mbr from menu
    ```bash
    $ fdisk /dev/mmcblk0
    ```
  * List partitions
    ```bash
    % p #to list partitions
			Disk /dev/mmcblk0: 15 GiB, 16088301568 bytes, 31422464 sectors
			Units: sectors of 1 * 512 = 512 bytes
			Sector size (logical/physical): 512 bytes / 512 bytes
			I/O size (minimum/optimal): 512 bytes / 512 bytes
			Disklabel type: dos
			Disk identifier: 0x00000000

			Device         Boot  Start     End Sectors  Size Id Type
			/dev/mmcblk0p1          16  125055  125040 61.1M  b W95 FAT32
		/	dev/mmcblk0p2      125056 1626111 1501056  733M 83 Linux
    ```
  * Delete Primary partition, and create a new Once
    ```bash
    % d,2 (to delete primary partition)

	% n p 2 #to create a new primary partition, then we need to insert the begin of the old      primary partition, then insert the new size or the last cylinder available

    % w #write the partition table
    ```
  * Reboot the RPi3
    ```bash
    $ reboot
    ```
  * Resize the filesystem
    ```bash
    $ resize2fs /dev/mmcblk0p2 #this task will take some time depending on the speed of the SD card

    $ df -h # once that the resize2fs is done, check the new size of the partition
    ```
* Proceed to update the system to last version of packages
  * Update the system
    ```bash
    $ apt-get update
    $ apt-get upgrade
    $ apt-get dist-upgrade
    $ apt-get install sudo iptables nano libexpat1 #installing sudo iptables and nano
    ```
  * Create "pi" user, and assign it to admin group
    ```bash
    $ add user pi
    $ add user pi sudo
    $ add user pi adm
    ```
