+++
title = "Installing Arch Linux on MSI GP63 (nvidia video setup)"
date = 2018-08-14T05:17:02Z
tags = []
categories = []
+++

Recently got the MSI GP63, so I installed the arch on it right away. Below is some experience that would like to share, so other people would feel life is easier when installing linux.

I will skip the basic stuff, such as alter the BIOS setting, set up locale etc, cause these information are all over the net. I will focus more on the video and audio.

### Booting with the USB:
I put below 2 kernel parameters to bring up the laptop.
```
  modprobe.blacklist=nouveau
  pcie_aspm=off
```

### Video card
Since this notebook is nvidia optimus, and personally I need nvidia (sorry, no nouveau). And bumblebee is not my favour, so I stick nvidia with xorg. Arch Wiki has many reference for it. But let's share what I did.

Install below packages 
```
linux-headers nvidia-dkms xorg-xrandr nvidia-settings nvidia-utils xorg-server xterm xsel xorg-twm  xorg-xinit xf86-input-libinput xbindkeys xdotool xorg-xev xorg-xwd xorg-xwininfo xorg-xwud xf86-video-intel xorg-xset xorg-xsetroot xorg-xvinfo xorg-xrefresh xorg-xrdb  xorg-xpr xorg-xprop xorg-xmodmap xorg-xlsclients xorg-xlsatoms xorg-xkill xorg-xkbutils xorg-xkbevd xorg-xhost xorg-xinput xorg-xkbcomp xorg-xgamma xorg-xdriinfo xorg-xdpyinfo xorg-xauth xorg-xcursorgen xorg-xcmsdb xorg-xbacklight xorg-x11perf xorg-smproxy xorg-setxkbmap xorg-sessreg xorg-iceauth xfsprogs lib32-nvidia-utils lib32-opencl-nvidia opencl-headers opencl-nvidia 
```

Since I picked dkms, so the pacman hook is added as listed in the [wiki](https://wiki.archlinux.org/index.php/NVIDIA#Pacman_hook).

Create /etc/X11/xorg.conf with below content. And you can see the intel is also added to it (just device) cause this guy handle backlight.
```
Section "Module"
  Load "modesetting"
EndSection

Section "Device"
  Identifier "nvidia"
  Driver "nvidia"
  BusID "PCI:1:0:0"
  Option "AllowEmptyInitialConfiguration"
EndSection

Section "Device"
    Identifier "Intel Graphics"
    Driver     "intel"
    BusID      "PCI:00:02:0"
    Option     "Backlight" "intel_backlight"
EndSection
```
 
Add below kernel parameter to grub /etc/default/grub
```
modprobe.blacklist=nouveau
nvidia-drm.modeset=1
```

Add modules to /etc/mkinitcpio.conf
```
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Rebuilt initramfs and update grub config
```
sudo mkinitcpio  -p linux
sudo grub-mkconfig  -o /boot/grub/grub.cfg
```

Inside xinitrc, add below
```
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
xrandr --dpi 96
```

### Final
Hopefully, you can successfully reboot and enjoy using nvidia for X. Sound is 98% work out of the box after pulseaudio and alsa installed. The 2% left were very annoying to me, will talk about it in next post.

### Reference
Arch Linux Nvidia [Wiki](https://wiki.archlinux.org/index.php/NVIDIA).

Arch Linux Optimus [Wiki](https://wiki.archlinux.org/index.php/NVIDIA_Optimus)
