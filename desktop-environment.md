# Installing the XFCE4 Desktop Environment

---

In this file will be all the instructions to get a complete GUI going with XFCE. We will install LightDM as a display manager to login.

## Install Xorg
Run `sudo pacman -S xorg-server`


## Install graphic drivers
* For arch on a VM that's:
    - install the drivers `sudo pacman -S virtualbox-guest-utils xf86-video-vmware`
    - enable the Virtualbox guest additions `systemctl enable vboxservice`

* For a desktop/laptop with a nvidia card:
    - install `sudo pacman -S nvidia`
    - **Need to test if that's all**
    

## Install XFCE4 and lightdm 
* `sudo pacman -S xfce4 xfce4-goodies`
* `sudo pacman -S lightdm lightdm-gtk-greeter`
* `systemctl enable lightdm`

### Reboot the machine