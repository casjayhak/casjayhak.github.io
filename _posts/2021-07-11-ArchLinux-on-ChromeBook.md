---
layout: post
title: ArchLinux-on-ChromeBook
categories: [blog,howto]
tags: [chromeos]
date: 2021-07-11 22:10
comments: true
---

## ArchLinux on ChromeBook

### Setup vmc
```shell
vmc destroy termina
vmc start termina; vmc start termina
```

### Download the container 
```shell
vmc container termina penguin https://us.images.linuxcontainers.org archlinux/current
```

### Start the container
```shell
lxc start penguin
```

### Enter into container
```shell
lxc exec penguin -- bash
```

### Enable networking
```shell
dhcpcd eth0
systemctl disable systemd-networkd
systemctl disable systemd-resolved
unlink /etc/resolv.conf
touch /etc/resolv.conf
systemctl enable dhclient@eth0
systemctl start dhclient@eth0
```

### Udpate packages
```shell
pacman -Syyu --noconfirm
```

### Install packages
```shell
pacman -S --needed base-devel git sudo
```

### Uncomment the following line: %wheel ALL=(ALL) ALL
```shell
nano /etc/sudoers
```

### Setup user
```shell
NEW_USER="GMAIL_USER"
OLD_USER="$(grep 1000:1000 /etc/passwd|cut -d':' -f1)"
pkill -9 -u "$OLD_USER
groupmod -n $NEW_USER $OLD_USER
usermod -d /home/$NEW_USER -l $NEW_USER -m -c $NEW_USER $OLD_USER
passwd $NEW_USER
usermod -aG wheel $NEW_USER
```

### Exit
```shell
exit
```

### Re-enter container
```shell
lxc console pengiun
```

### Install additional packages
```shell
pacman -S --needed base-devel git
git clone https://aur.archlinux.org/yay.git ~/.cache/yay; cd .cache/yay/; makepkg -si
yay -S cros-container-guest-tools-git
```


### Enable services
```shell
systemctl --user enable sommelier@0.service sommelier-x@0.service sommelier@1.service sommelier-x@1.service
```

### Reboot your chromebook
