---
lang: en
layout: doc
permalink: /doc/uefi-troubleshooting/
ref: 177
title: UEFI Troubleshooting
---



## Successfully installed in legacy mode, but had to change some xen parameters

**Note**: If you make changes, you must boot from "Partition 1" explicitly from UEFI boot menu.

**Change the xen configuration on a USB media**

01. Attach the usb disk, mount the EFI partition (second partition available on the disk)
02. Open a terminal and enter the command `sudo su -`. Use your preferred text editor (e.g `vi`) to edit your xen config (`EFI/BOOT/grub.cfg`):

    ```
    vi EFI/BOOT/grub.cfg
    ```

03. Change the `multiboot2 /images/pxeboot/xen.gz` line to add your kernel parameters on the boot entry of your choice
04. Install using your modified boot entry

**Change xen configuration directly in an iso image**
01. Set up a loop device (replacing `X` with your ISO's version name): `losetup -P /dev/loop0 Qubes-RX-x86_64.iso`
02. Mount the loop device: `sudo mount /dev/loop0p2 /mnt`
03. Edit `EFI/BOOT/grub.cfg` to add your params to the `multiboot2 /images/pxeboot/xen.gz` line
04. Save your changes, unmount and dd to usb device

## Installation freezes before displaying installer

If you have an Nvidia card, see [Nvidia Troubleshooting](https://github.com/Qubes-Community/Contents/blob/master/docs/troubleshooting/nvidia-troubleshooting.md#disabling-nouveau).


## Installation from USB stick hangs on black screen

Some laptops cannot read from an external boot device larger than 8GB. If you encounter a black screen when performing an installation from a USB stick, ensure you are using a USB drive less than 8GB, or a partition on that USB lesser than 8GB and of format FAT32.

## Boot device not recognized after installing

Some firmware will not recognize the default Qubes EFI configuration.
As such, it will have to be manually edited to be bootable.

1. Boot Qubes OS install media into [rescue mode](/doc/uefi-troubleshooting/#accessing-installer-rescue-mode-on-uefi)

2. Press '3' to go to the shell

3. Find and mount the EFI system partition. (replace `/dev/sda` with your disk name. If unsure, use the `lsblk` command to display a list of disks):
    ```
    fdisk -l /dev/sda | grep EFI
    ```
    The output should look like this:
    ```
    /dev/sda1   2048    1230847 1228800 600M EFI System
    ```
    Then mount it:
    ```
    mkdir -p /mnt/sysimage/boot/efi
    mount /dev/sda1 /mnt/sysimage/boot/efi
    ```
    
4. Copy `grubx64.efi` to the fallback path:
    
    ```
    cp /mnt/sysimage/boot/efi/EFI/qubes/grubx64.efi /mnt/sysimage/boot/efi/EFI/BOOT/bootx64.efi
    ```
    
5. Type `reboot`

## "Qubes" boot option is missing after removing / attaching a disk or updating the BIOS
1. Boot Qubes OS install media into [rescue mode](/doc/uefi-troubleshooting/#accessing-installer-rescue-mode-on-uefi)

2. Press '3' to go to the shell
3. Create boot entry in EFI firmware (replace `/dev/sda` with your disk name and `-p 1` with `/boot/efi` partition number):

    ```
    efibootmgr -v -c -u -L Qubes -l /EFI/qubes/grubx64.efi -d /dev/sda -p 1 
    ```
    
## Accessing installer Rescue mode on UEFI

Choose "Resucue a Qubes OS system" from grub2 boot menu.
