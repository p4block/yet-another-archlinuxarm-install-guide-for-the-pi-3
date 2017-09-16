# yet-another-archlinuxarm-install-guide-for-the-pi-3
Really tired of bad tutorials

You will need a root terminal for this.

If your PC has a good device reader the sdcard will show as an emmc device. If not, as a regular sd device.
Find where your computer detects the sdcard by using the `lsblk` command.

We will need to partition the card in a very specific way that is of the taste of the raspberry's dumb bootloader.

Open the sdcard using fdisk.

`fdisk /dev/sdx` or `fdisk /dev/mmcblkn`

We will be using the mmcblk route for this tutorial as it's the proper one. 

At the fdisk prompt, use the following commands to setup the partitions.

    Type o. This will clear out any partitions on the drive.
    Type p to list partitions. There should be no partitions left.
    Type n, then p for primary, 1 for the first partition on the drive, press ENTER to accept the default first sector, then type +100M for the last sector.
    Type t, then c to set the first partition to type W95 FAT32 (LBA).
    Type n, then p for primary, 2 for the second partition on the drive, and then press ENTER twice to accept the default first and last sector.
    Write the partition table and exit by typing w.

Once it is done, `lsblk` should show two partitions inside your sdcard.

Format them:

`mkfs.vfat /dev/mmcblk0p1`
`mkfs.ext4 /dev/mmcblk0p2`

Mount the soon-to-be root partition at /mnt or a destination of your choice

`mount /dev/mmcblk0p2 /mnt`

Create a boot folder inside where the first partition will reside once the system is booted

`mkdir /mnt/boot`

Mount the first partition into it

`mount /dev/mmcblk0p1 /mnt/boot`

Download the pi3's pure armv8 rootfs. It will be downloaded to your current directory.

`wget http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-3-latest.tar.gz`

Unpack the rootfs into the sdcard

`bsdtar -xpf ArchLinuxARM-rpi-3-latest.tar.gz -C /mnt`

Actually write to disks. This will lock up the terminal until finished.

`sync`

Unmount the partitions and put the sd card in the Pi. You are ready to go.

`umount -R /mnt`


