---
title: "Como Instalar Archlinux en Raspberry Pi 4"
date: 2020-07-16T16:46:42-05:00
description: En esta ocasión vamos a instalar ArchLinux en una Raspberry Pi 4, la Raspberry que estoy utilizand tiene 8GB de memoria.
draft: true
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- archlinux
- raspberry
series:
-
categories:
- Tutorial
- Linux
image:
---

Guia de Instalacion Oficial: https://archlinuxarm.org/platforms/armv6/raspberry-pi

## Bajar Arch Linux para Raspberry Pi.

https://archlinuxarm.org/about/downloads
http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-4-latest.tar.gz

## Crear SD Card.
Una vez que tenemos arch en nuestro disco o si deseas mientras se descarga, comencemos a configurarla tarjeta SD, inserta la tarjeta en tu computadora.

Primero debemos verificar la tarjeta donde queremos instalar Arch.

```bash
lsblk
```

ATENCIÓN: En mi caso el comando lsblk me dice que mi tarjeta está en /dev/mmcblk0, de aquí en adelante debes sustituir mmcblk0 por el dispositivo en tu computadora. Todos los cambios hechos en tu sistema son tu responsabilidad y yo no puedo hacerme responsable por ellos. Por favor continúa bajo tu propio riesgo.

Vamos a crear las particiones en la tarjeta. En este caso la tarjeta no tiene particiones, es completamente nueva.

```bash
sudo fdisk /dev/mmcblk0
```

En el prompt presiona __o__.
Esto borrará cualquier partición que se encuentre en la tarjeta.

Luego, presiona **'p'** para listar las particiones.
Después presiona n, seguido de p para crear una partición primaria, 1 para designarla como la primera partición, presiona ENTER para aceptar los valores por defecto para el primer sector, después escribe +100M para crear la partición de 100 MB.

Presiona *t*, luego c para definir el tipo de partición como W95 FAT32. Esta primer partición será nuestra partición “boot”

Presiona n, para crear una nueva partición que será “root” y es donde estará el sistema operativo. Luego presiona p para definir una partición primaria, presiona 2 para definir que esta será nuestra 2da partición en la tarjeta, luego presiona ENTER dos veces para aceptar el sector de inicial y final de la partición.

Si deseas revisar los cambios que acabas de hacer, presiona p para listar las nuevas particiones.

Para guardar los cambios físicamente en la tarjeta y salir de fdisk, presiona w.

Ahora, primero, vamos a formatear la primera partición que creamos y montarla en un directorio local que llamaremos “boot”

$ sudo mkfs.vfat /dev/mmcblk0p1
$ mkdir boot
$ sudo mount /dev/mmcblk0p1 boot


Después tenemos que formatear nuestra 2da partición y montarla en un directorio local que llamaremos simplemente “root”

$ sudo mkfs.ext4 /dev/mmcblk0p2
$ mkdir root
$ sudo mount /dev/mmcblk0p1 root


Ahora vamos a descomprimir el archivo que dejamos bajando

$ sudo bsdtar -xpf ArchLinuxARM-rpi-4-latest.tar.gz -C root
$ sudo sync




Ahora necesitamos mover los archivos de root/boot a boot (directorio local)

$ sudo mv root/boot/* boot

(Opcional) Si deseas hacerle un overclock a tu raspberry, tenemos que modificar el archivo boot/config.txt y agregar los siguientes parámetros utilizando nano o vim.
Los siguientes parámetros han sido probados en una Raspberry Pi 4 con 8 GB de memoria.

over_voltage=6
arm_freq=2147
gpu_freq=750


Desmontamos los directorios locales para arrancar nuestra Raspberry Pi con Arch Linux.

$ sudo umount boot root

Inserta la tarjeta en la raspberry Pi y conectala a la corriente. Es preferible que tengas la Raspberry conectada a la red con cable.
En tu ruteador busca la Raspberry e identifica la dirección IP que le asignó
Conéctate utilizando SSH con el usuario por defecto llamado “alarm” el password es “alarm”

Después cambiate a root, el password es root.

Lo último que voy a cubrir en este documento es el inicializar pacman y crear las llaves.

$ pacman-key --init
$ pacman-key --populate archlinuxarm


Y ya para terminar, actualiza Arch





Agregar usuario “alarm” y habilitarel grupo “wheel” utilizando visudo

usermod –aG wheel UserName

Para verificar la temperatura:
$ /opt/vc/bin/vcgencmd measure_temp
https://linuxhint.com/raspberry_pi_temperature_monitor/#:~:text=If%20the%20temperature%20of%20the,for%20it%20to%20work%20properly.
Para verificar la velocidad del CPU:
$ /opt/vc/bin/vcgencmd measure_clock arm

Para el recuadro alrededor de la pantalla:

disable_overscan=1

Instalar i3
Instalar XFCE4
Instalar LDXM
Instalar i3-wm


Instalar i3-wm

[alarm@alarmpi ~]$ sudo pacman -S i3-wm i3status i3blocks dmenu terminator



community/i3status 2.13-3 (i3)
   Generates status bar to use with i3bar, dzen2 or xmobar
community/i3-wm 4.18.1-1 (i3)
   An improved dynamic tiling window manager
community/i3blocks 1.5-3 (i3)
   Define blocks for your i3bar status line
community/dmenu 4.9-2
   Generic menu for X

Instalar 

```bash
sudo pacman -S xorg-server xf86-video-fbdev lxdm
startx
sudo systemctl enable lxdm
sudo systemctl start lxdm
```
