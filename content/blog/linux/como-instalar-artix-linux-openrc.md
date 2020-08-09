---
title: "Como Instalar Artix Linux Openrc"
date: 2020-08-05T21:28:36-05:00
description: En esta ocasión vamos a instalar Artix Linux con OpenRC y MATE como escritorio. Básicamente Artix es Arch pero sin Systemd.
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- artix
- tutorial
series:
-
categories:
- Artix
- Tutorial
- Instalación Base
image: /images/posts/artix-mate/artix-mate.png
---
## Bajar el ISO

Lo primero es descargar el ISO del sitio <a href="https://artixlinux.org/download.php" target="_blank">web oficial de Artix Linux</a>

Artix tiene varias versiones, en esta ociasión vamos a realizar una instalación base, también instalaremos OpenRC en lugar de systemd y por último pondermos Mate como escritorio:

![descarga el ISO que dice "openrc"](/images/posts/artix-mate/download.png)
## Comienza la Instalación

Ya que arrancamos nuestra máquina, ya sea virtual o física comencemos la instalación entonces.

![Artix Pantalla Inicial](/images/posts/artix-mate/login-screen.png)

### Verificiar que tengas conexión a Internet.

```bash
ip a
```

Este comando debe regresar los nombres de los dispositivos de red y una dirección de IP.

### Crear particiones

Antes de crear las particiones y comezar la instalación, debemos cambiarnos al usuario de "root"

```bash
sudo su -
```

Para verificar el nombre del dispositivo, es decir el disco donde instalaremos Artix, teclea:

```bash
lsblk
```

 En mi caso el disco que donde voy a instalar Artix es "vda"

 ![lsblk](/images/posts/artix-mate/lsblk.png)

 ```bash
 cfdisk /dev/vda
 ```

Como es una instalación sobre UEFI, selecciona "gpt"

Crea una partición para "boot" y otra para "/"

La partición de "boot" será de 100 MB y el resto del disco para "raíz".

Recuerda que la partición de "boot" debe ser tipo EFI system

![Así deben quedar las particiones](/images/posts/artix-mate/cfdisk.png)

### Formatear particiones

Formateamos la primera partición de tipo "FAT32"

```bash
mkfs.fat -F 32 /dev/vda1
```

Luego formateamos la segunda partición de tipo "ext4"

```bash
mkfs.ext4 /dev/vda2
```
### Montar las particiones

Primero montamos la partición 2 **"vda2"** en /mnt

```bash
mount /dev/vda2 /mnt
```

Luego creamos el directorio de **boot** y montamos la primera partición

```bash
mkdir /mnt/boot
mount /dev/vda1 /mnt/boot
```
### Instalar el sistema base

```bash
basestrap /mnt base base-devel linux linux-firmware openrc vim amd-ucode (intel-ucode si es tu caso)
```

### Generar tabla de sistema de archivos (fstab)

```bash
fstabgen -U /mnt >> /mnt/etc/fstab
```

## Configurar el sistema base

Para esto hay que entrar a **nuestra instalación** tecleando:

```bash
artools-chroot /mnt
```

### Crear archivo de Swap

```bash
dd if=/dev/zero of=swapfile bs=1G count=2 status=progress
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

### Editar /etc/fstab

Agrega lo siguiente al final del archivo

```text
/swapfile   none    swap    defaults    0 0
```

### Configurar el reloj del sistema

```bash
ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
```

#### Sincroniza el reloj del hardware con el del sistema

```bash
hwclock --systohc
```

### Localización 

```bash
vim /etc/locale.gen
```

Genera el "locale"

```bash
locale-gen
```

Necesitamos ahora poner este "locale" en el archivo de configuración

```bash
vim /etc/locale.conf
```

agrega lo siguiente

```text
LANG="en_US.UTF-8"
```

### GRUB

Para instalar GRUB

```bash
pacman -S grub os-prober efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub (para sistemas con UEFI)
grub-mkconfig -o /boot/grub/grub.cfg
```

### Crea un password para usuario root

```bash
passwd
```

### Crea tu usuario

```bash
useradd -mG wheel jorge
passwd jorge
```

### Habilita el grupo "wheel"

```bash 
EDITOR=vim visudo
```

Descomenta la linea que dice:

```text
%wheel ALL=(ALL) ALL
```

### Configura la red

Edita el archivo 

```bash
vim /etc/hostname
```

Agrega solo el nombre de tu computadora, sin dominio ni nada más

Después edita el archivo 

```bash
vim /etc/hosts
```

Para que quede como sigue:

```text
# Static table lookup for hostnames.
# See hosts(5) for details.

127.0.0.1	localhost
::1	localhost
127.0.0.1	artix.localdomain	artix
```


### Instala connman

```bash
pacman -S connman-openrc connman-gtk (or cmst for Qt-based DEs)
rc-update add connmand
```

### Resetea la computadora

```bash
exit
umount -R /mnt
reboot
```

## Configuración post-instalación e Instalación de Escritorio (MATE)

### Instala xorg

```bash
sudo pacman -Syyy
sudo pacman -S xorg --ignore xorg-server-xdmx
```

### Instala el controlador de tu tarjeta de video

Si tienes tarjeta **intel** instala:

```bash
sudo pacman -S xf86-video-intel
```

Si tienes tarjeta **AMD** instala:

```bash
sudo pacman -S xf86-video-amdgpu
```

Si tienes tarjeta **NVIDIA** instala: 

```bash
sudo pacman -S nvidia-lts
```

### Instala el escritorio

```bash
sudo pacman -S mate mate-extra system-config-printer blueman connman-gtk
```

### Instala el Administrador de Inicio de Sesión (Display Manager)

```bash
sudo pacman -S displaymanager-openrc sddm
```

### Habilita el Administrador de Inicio de Sesión en OPENRC

```bash
sudo rc-update add xdm default
sudo vim /etc/conf.d/xdm 
```

Edita para que quede así:

```text
DISPLAYMANAGER="sddm"
```

### Reinicia el sistema.

```bash 
sudo reboot
```

