# Instalation of RPI 3 as Router, PLEX Media Server and NAS, addinga USB drive

_This guide will be only based in Linux OS, planning to replicate it to Windows and OSX_

---
[installing](Install_Minibian_from_scratch.md)
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
*
