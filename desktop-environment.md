# Installing the XFCE4 Desktop Environment
In this file will be all the instructions to get a complete GUI going with XFCE. We will install LightDM as a display manager to login.

`sudo pacman -S xorg-server`
`sudo pacman -S virtualbox-guest-utils xf86-video-vmware`
systemctl enable vboxservice

`sudo pacman -S xfce4 xfce4-goodies`
`sudo pacman -S lightdm lightdm-gtk-greeter`
`systemctl enable lightdm`
