# Arch basic installation guide

---

This guide is based upon the installation guide on the arch linux wiki. I had some issues with installing it from the wiki itself so I decided to make this guide to have a clear path to follow.

The guide on the wiki has some details that are hidden on other pages and also lacks some guidance on for example making a proper user account.

## Initial steps:

These steps are needed to boot to the live environment and to set some intitial values and settings before starting the partitioning and installation process.

Note: The wiki article mentions setting your keyboard layout. This is not needed as long as I have a WASD keyboard as the US layout is the default.

1. Get the latest ISO from https://archlinux.org/download/
2. Verify the ISO for security using the methods explained on the downloads page
3. Boot to live environment
4. Verify boot mode with:
   `ls /sys/firmware/efi/efivars`
   if this does returns an error the system is not booted in UEFI mode
5. Check internet connection with:
   - `ip link` -> this shows you the list of network interfaces
   - `ping google.com` -> to test actual connection
6. Update the system clock:
   - `timedatectl set-ntp true`
   - to check if it worked run: `timedatectl status`
7. Install git with `pacman -Sy git`
8. Clone this repo with `git clone https://github.com/DMKesseler/arch-install.git`
9. Enter the repo folder with `cd arch-install`
10. Run the base installation script to get everything set up with `bash base-install`

## System configuration

Now that Arch is installed we need to edit and/or create some settings, after this we can also create a root password and add our user account

### Steps

1. Generate fstab file with the command `genfstab -U /mnt >> /mnt/etc/fstab`
2. Run `arch-chroot /mnt` to change into the root directory of the new installation
3. Install vim in order to be able to edit files `pacman -S vim`
4. Setting time and locale:
   - Set the timezone with `ln -sf /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime`
   - Run `hwclock --systohc` to set the hardware clock correctly
   - Edit /etc/locale.gen by uncommenting the line that has `en_US.UTF-8 UTF-8` on it
   - Run `locale-gen` to generate the locales
   - Create /etc/locale.conf and insert `LANG=en_US.UTF-8`
5. Set host name by creating /etc/hostname and inserting your desired hostname, remember this name for the next step
6. Edit the file /etc/hosts and insert:

   > 127.0.0.1 localhost
   >
   > ::1 localhost
   >
   > 127.0.1.1 _hostnamefromstep5_

7. Set root password and create a user account
   - Set password for root user with `passwd`
   - Create new user with `useradd -m -G wheel,audio,video,optical,storage *username*`
   - Set password for new user with `passwd *username*`
8. Use `EDITOR=vim visudo` and uncomment the line ` %wheel ALL=(ALL) ALL`
9. Install and configure GRUB
   - Download GRUB with `pacman -S grub efibootmgr dosfstools os-prober mtools`
   - Make EFI directory with `mkir /boot/EFI`
   - Mount boot directory there with `mount /dev/sda1 /boot/EFI`
   - Install grub with `grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck`
   - Run `ls -l /boot/grub` and check if folder locale exists, if not create it
   - Run `cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
   - Run `grub-mkconfig -o /boot/grub/grub.cfg` to create configuration file
10. Install programs to ensure cable and wifi both work
    - `pacman -S networkmanager wpa_supplicant wireless_tools netctl dialog`
    - Enable networkmanager `systemctl enable NetworkManager`
11. If on a production device I need to enable microcode updates by installing the intel-ucode package (or amd-ucode if I change cpu to AMD)
    Steps:
    - `pacman -S intel-ucode`
    - `grub-mkconfig -o /boot/grub/grub.cfg` to reregenerate the config file
12. Exit the chroot with `exit`
13. Run `umount -a` to unmount
14. Shutdown the computer/vm and take out the USB with the live image
