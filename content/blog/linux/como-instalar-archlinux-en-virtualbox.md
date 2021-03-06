---
ID: archvbox-install
title: "Como Instalar Archlinux en Virtualbox"
date: 2020-07-16T19:39:45-05:00
description: "Hoy vamos a instalar ArchLinux en una máquina virtual en VirtualBox. La instalación es muy parecida a la que deberás llevar a cabo cuando lo instalemos en una laptop o PC. Bien, entoces, comencemos..."
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- archlinux
- linux
series:
- Como instalar Arch Linux
categories:
- Linux
- Tutorial
image: /images/posts/archvbox-install/Arch-Linux-VirtualBox.png
---

<!-- ![Como instalar Arch Linux en VirtualBox](/images/posts/archvbox-install/Arch-Linux-VirtualBox.png) -->

{{< youtube b5gSiNBSLNc>}}

## Bajar el ISO

El primer paso obviamente es bajar el ISO del [sitio oficial](https://www.archlinux.org/download/)

## Crear una máquina virtual en VirtualBox

Vamos a crear una máquina virtual en VirtualBox, las espicificaciones pueden variar dependiendo de tu computadora, en mi caso estoy corriendo VBox en una laptop con 8GB de Memoria y suficiente espacio en disco.

### Crea una máquina virtual nueva con el siguiente nombre y tipo

Presiona "New" para crear una nueva maquina virtual, la llamaremos "ArchLinux", sera de tipo "Linux" por supuesto y VBox deberá seleccionar por defecto la version, si no lo hace asegurate de que la versión sea "Arch Linux (64'bit)

![Nueva Maquina](/images/posts/archvbox-install/nueva-vm-min.jpg)

Presiona "Next" o "Siguiente" para continuar,

### Asigna la memoria

Asigna la memoria que requieras o que tengas disponible. La guía oficial de Arch Linux recomenda como mínimo 530 MB de RAM y 2GB de espacio en disco, pero esta máquina nos va a servir para los siguientes tutoriales yo le voy a asignar 4GB de RAM y 10GB en disco; después le instalaremos un escritorio o manejador de ventanas.

![Selecciona Memoria](/images/posts/archvbox-install/memoria-vm-min.jpg)

Y pesiona "Siguente" o "Next"

### Crea un disco virtual

Como estamos creando una máquina totalmente nueva, seleccionaremos crear un disco virtual Nuevo y presiona "Crear"

![Crea un disco nuevo](/images/posts/archvbox-install/disco-vm-min.jpg)

Selecciona el tipo de disco que sea VDI (VirtualBox Disk Image) y presiona "Next"

![VDI Disk](/images/posts/archvbox-install/disco-vm02-min.jpg)

Ahora selecciona el tipo de almacenamiento dinámico, esto hará que VirtualBox cree un archivo el cual utilizará para almacenar los datos y que irá creciendo conforme vayamos agregando más cosas en nuestra máquina virtual, esto quiere decir que aunque hayamos creado un disco virtual de 10GB, si solo utilizamos 5GB, esto será lo que estaremos utilizando físicamente en nuestra computadora.

![Almacenamiento Dinámico](/images/posts/archvbox-install/disco-vm03-min.jpg)

Por último asigna el espacio y presiona "Crear".

![10GB de Almacenamiento](/images/posts/archvbox-install/disco-vm04-min.jpg)

### Configura la nueva máquina virtual

Antes de iniciar nuestra nueva máquina virtual tenemos que hacer unas modificaciones. La intención de este tutorial es que adquieras los conocimientos para instalar Arch Linux en tu laptop o PC de escritorio. Las computadoras actuales utilizan [UEFI](https://es.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface) que es lo que viene a reemplazar lo que antes era el [BIOS](https://es.wikipedia.org/wiki/BIOS) de las computadoras antiguas.

#### Habilita EFI

Dicho esto, en la pantalla principal de VirtualBox, selecciona tu nueva máquina virtual y presiona "Configuración" o "Settings".

Luego selecciona "Sistema" o "System".

Aquí asegurate de habilitar la opción de **EFI**

![Habilita EFI](/images/posts/archvbox-install/configura-sistema-min.jpg)

#### Modifica el controlador de gráficos

A continuación vamos a modificar el controlador de gráficos de nuestra máquina virtual, para esto Selecciona "Display" y en el controlador de gráficos selecciona **VBoxSVGA** Y presiona "OK"

![Selecciona VBoxSVGA](/images/posts/archvbox-install/graphic-controller-min.jpg)

VirtualBox te dirá que la configuración es inválida, no hagas caso, no pasa nada, confía en lo que lees en Internet #Sarcasmo

#### Cambia el tipo de conexión a la red

Por último debemos cambiar el tipo de conexión a la red para que sea tipo "Bridged".

![Bridged Network](/images/posts/archvbox-install/bridge_network-min.jpg)

## Instala Arch Linux

### Inicia la máquina virtual

Ahora si vamos a comenzar con la instalación de Arch Linux.

Una vez que inicias la máquina virtual, como no tiene sistema operativo VirtualBox debe darte la opción de seleccionar un ISO para arrancar el sistema.

![Selecciona ISO](/images/posts/archvbox-install/startup_disk-min.jpg)

Si por alguna razón no lo hace, lo que puedes hacer es seleccionar el ISO de Arch Linux y asignarlo Disco Óptico como se ve en la siguiente pantalla

Seleciona "Storage" luego el controlador IDE y luego selecciona "Seleccionar Disco..."

![Selecciona ISO](/images/posts/archvbox-install/Optic_Disc-min.jpg)

### Inicia la Instalación de Arch Linux

Para iniciar la instalación selecciona Arch Linux archiso x86_64 UEFI CD

Una vez iniciado el sistema comienza lo bueno, ahora sí, vamos a instalar Arch Linux.

![Pantalla de Inicio](/images/posts/archvbox-install/install01-min.jpg)

#### Configura el Teclado

En mi caso estoy utilizando un teclado en Inglés así que no requiero en este paso, pero si tu estás utilizando un teclado en Español lo mejor es que lo configures para no batallar y hacer la instalación más sencilla.

```bash
loadkeys es
```

#### Verifica UEFI en la maquina virtual

Siguiente vamos a verificar que tengamos configurada nuestra máquina virtual con UEFI activado, para eso debemos teclear:

```bash
ls /sys/firmware/efi/efivars
```

El commando deberá regresar un montón de variables en la pantalla, si no es así regresa al paso donde debes habilitar UEFI en la máquina virtual

![UEFI Vars](/images/posts/archvbox-install/uefi_vars-min.jpg)

#### Sincroniza el reloj

A continuación vamos a sincronizar el reloj con internet, para eso escribimos

```bash
timedatectl set-ntp true
```

#### Selecciona el servidor más cercano para instalar los paquetes (Mirror)

Para esto vamos a necesitar un paquete que se llama [**Reflector**](https://wiki.archlinux.org/index.php/Reflector), así que vamos a instalarlo primero escribiendo

```bash
sudo pacman -Syy reflector
```

![Instala Reflector](/images/posts/archvbox-install/instala_reflector-min.jpg)

Ahora selecciona el servidor más cercano a donde te encuentras, para ver una lista de países escribe

```bash
reflector --list-countries | more
```

![Reflector lista de países](/images/posts/archvbox-install/reflector_paises-min.jpg)

Ya que tengas identificado el país que te queda más cerca escribe lo siguiente:

(En mi caso como estoy en Estados Unidos, escogeré United States por supuesto, pero tú puedes cambialo por el país de la lista anterior que te quede más cerca)

```bash
reflector -c "United States" -a 6 --sort rate --save /etc/pacman.d/mirrorlist
```

![Reflector actualiza mirrorlist](/images/posts/archvbox-install/reflector_actualiza_mirrorlist-min.jpg)

#### Actualiza los servidores de paquetes

Siguente teclea

```bash
pacman -Syyy
```

![pacman -Syyy](/images/posts/archvbox-install/actualiza_pacman-min.jpg)

### Crea Particiones en el Disco

Primero vamos a verificar cual es el dispositivo donde se encuentra el disco de nuestra máquina virtual

```bash
lsblk
```

![lsblk](/images/posts/archvbox-install/lsblk01-min.jpg)

Mi disco está en /dev/**sda**

Tenemos que crear 2 particiones, una para los archivos de inicio o "boot" y otra donde estará instalado Arch Linux, como este es un tutorial de "entrenamiento" no vamos a crear partició de SWAP ni tampoco partición para el directorio de "home", vamos a hacer una instalación sencilla.

La primera partición deberá de ser de tipo **EFI** y la segunda tipo **Linux**

Para crear las particiones voy a utilizar **cfdisk**

```bash
cfdisk
```

Selecciona **gpt**

![cfdisk tipo de etiquetas](/images/posts/archvbox-install/cfdisk_label_type-min.jpg)

Luego creamos la primera partición.

1. Selecciona [  New  ]
2. El tamaño de la primera partición será de **250M**

![particion 1](/images/posts/archvbox-install/cfdisk_part1_250Mjpg-min.jpg)

3. Selecciona [  Type  ] y luego selecciona el tipo "EFI System"

![particion tipo EFI](/images/posts/archvbox-install/cfdisk_part1_tipo_efi-min.jpg)

4. Selecciona el espacio disponible (Free Space), luego selecciona [  New  ] , presiona ENTER y luego ENTER de nuevo para seleccionar todo el espacio disponible, en mi caso los 9.8G restantes

![particion 2](/images/posts/archvbox-install/cfdisk_part2-min.jpg)

5. Selecciona [  Type  ] y luego selecciona **"Linux filesystem"** de la lista

![particion 2](/images/posts/archvbox-install/cfdisk_part2_tipo_linux-min.jpg)

6. Selecciona [  Write  ] para guardar los cambios y crear las particiones. cfdisk te preguntara si estás seguro de que deseas crear las nuevas particiones. Teclea "yes" y presiona ENTER

![Guarda los Cambios en cfdisk](/images/posts/archvbox-install/cfdisk_write_yes-min.jpg)

#### Verifica las particiones

Para verificar que todo haya ido bien teclea lsblk y asegurate de que las particiones estén como sigue

![lsblk](/images/posts/archvbox-install/lsblk02-min.jpg)

#### Formatea las particiones

1. La primera partición es nuestra partición tipo EFI por lo tanto debemos formatearla de tipo FAT32

```bash
mkfs.fat -F32 /dev/sda1
```

![Formatea la primera partición](/images/posts/archvbox-install/mkfs_vfat32-min.jpg)

2. Formatea la segunda partición, pero en esta ocasión el tipo será EXT4

```bash
mkfs.ext4 /dev/sda2
```

![Formatea la segunda partición](/images/posts/archvbox-install/mkfs_ext4-min.jpg)

### Monta las particiones

Una vez creadas y formateadas nuestras particiones ahora debemos montarlas en nuestro sistema, para eso necesitmos hacer lo siguiente:

1. Montar la segunda partición en **/mnt** que es nuestro directorio para la instalación

```bash
mount /dev/sda2 /mnt
```

2. Siguiente debemos crear el directorio donde estará nuestro partición de inicio, para eso vamos a crear un directorio nuevo en **/mnt/boot/efi** (El directorio boot no existe, así que debemos crearlo primero o podemos crear ambos directorios al mismo tiempo con el siguente comando)

```bash
mkdir -p /mnt/boot/efi
```

3. Monta la primera partición en **/mnt/boot/efi**

```bash
mount /dev/sda1 /mnt/boot/efi
```

4. Verifica que las particiones estén montadas correctamente utilizando lsblk

```bash
lsblk
```

![Monta las Particiones](/images/posts/archvbox-install/monta_particiones-min.jpg)

#### Instala los paquetes "base"

Una vez montadas las particiones comenzamos la instalación de los paquetes base, los paquetes que vamos a instalar son "base, linux, linux-firmware y vim (puedes instalar nano si lo deseas)

```bash
pacstrap /mnt base linux linux-firmware vim
```

![Instalación base](/images/posts/archvbox-install/instalacion_base-min.jpg)

![Instalación base](/images/posts/archvbox-install/instalacion_base02-min.jpg)

#### Crear fstab

Listo, la instalación base ya está lista, ahora lo que necesitamos hacer es crear nuestro archivo **fstab**

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

![Crea fstab](/images/posts/archvbox-install/crea_fstab-min.jpg)

### Crea el archivo de SWAP

Listo, ya que veas en tu archivo de fstab que todo está correcto, que tienes ambas particiones configuradas correctamente, vamos a crear nuestro archivo de swap.

1. Primero entra a la instalación base

```bash
arch-chroot /mnt
```

Fíjate bien y el prompt cambió, ahora está entre corchetes **[root@archiso /]**

2. Vamos a crear el archivo de swap utilizando el comando **dd**, el archivo que voy a crear será de 2GB, es decir 2 bloques de 1GB cada uno.

```bash
dd if=/dev/zero of=/swapfile bs=1G count=2 status=progress
```

3. Ahora hay que cambiar los permisos del archivo

```bash
chmod 600 /swapfile
```

4. Cambia el tipo de archivo a **swap**

```bash
mkswap /swapfile
```

5. Habilita el archivo con:

```bash
swapon /swapfile
```

![Crea swapfile](/images/posts/archvbox-install/crea_swapfile-min.jpg)

6. Agrega el archivo al sistema de archivos, es decir, al archivo **fstab**. (Yo voy a utilizar vim, si instalaste nano, utiliza nano)

```bash
vim /etc/fstab
```

Al final del archivo agrega lo siguiente:

```txt
/swapfile none swap defaults 0 0
```

![Agrega swapfile a fstab](/images/posts/archvbox-install/agrega_swapfile_fstab-min.jpg)

### Configura tu zona horaria

Ahora podemos continuar con la configuración de nuestra instalación, así que toca el turno de la zona horaria.

1. Primero vamos a ver un listado de las zonas horarias disponibles. La zona horaria donde me encuentro es la zona de **Chicago**, sustituye Chicago por la ciudad que te corresponda, por ejemplo Mexico

```bash
timedatectl list-timezones | grep Chicago
```

2. Una vez identificada la zona horaria, creamos un enlace simbolico en **etc/localtime**

```bash
ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
```

3. Ahora ya podemos sincronizar el reloj del sistema con el reloj de hardware

```bash
hwclock --systohc
```

### Configura tu "Localización"

Esto es para decirle a Arch Linux donde te encuentras y cual es el grupo de caracteres que corresponden a tu idioma por ejemplo.

En mi caso voy a utlizar Inglés de Estados Unidos, el código es **"en_US.UTF-8"** pero por ejemplo, si te encuentras en México y estás utilizando teclado en Español y deberás utilizar el código de **"es_MX.UTF-8"**.

**Nota:** Puedes habilitar más de un código de localización.

1. Para habilitar el código de localización deseado, edita el siguente archivo y descomenta la línea donde se encuentre el código que andas buscando. (De nuevo, yo utilizo vim, tu puedes hacerlo con nano o con el que hayas instalado al inicio)

```bash
vim /etc/locale.gen
```

![vim locale.gen](/images/posts/archvbox-install/vim_locale_gen-min.jpg)

2. Genera la localización en el sistema

```bash
locale-gen
```

![locale-gen](/images/posts/archvbox-install/locale_gen-min.jpg)

3. Ahora necesitamos crear el archivo de configuración en nuestra instalación para eso:

```bash
echo LANG=en_US.UTF-8 >> /etc/locale.conf
```

**Nota:** Sustituye "en_US.UTF-8" por el código que hayas habilitado en el paso anterior

4. (Opcional) Si al comienzo de la instalación seleccionaste el tipo de teclado, deberás agregarlo a tu instalación con el siguente comando:
(Suponiendo que seleccionaste "es")

```bash
echo KEYMAP=es >> /etc/vconsole.conf
```

### Configura la Red

1. Crea el archivo "hostname" para darle un nombre a tu máquina virutal. En mi caso voy a ser creativo y le daré el nombre de "archlinux"

```bash
vim /etc/hostname
```

![hostname](/images/posts/archvbox-install/hostname-min.jpg)

2. Agrega lo siguiente a tu archivo /etc/hosts

```bash
vim /etc/hosts
```

```txt
127.0.0.1	localhost
::1		localhost
127.0.1.1	archlinx.localdomain	archlinux
```

![hosts](/images/posts/archvbox-install/hosts-min.jpg)

### Crea un password para root

El siguiente paso de la instalación es darle un password a nuestro usuario de root.

Como ya estamos en nuestra instalación, para cambiar el password solo debemos teclear:

```bash
passwd
```

En el prompt escribe el nuevo password (Uno que no se te olvide) y en el siguente prompt vuelve a escribir el mismo password para confirmarlo.

![Password root](/images/posts/archvbox-install/passwd_root-min.jpg)

### Instala los paquetes finales

Es momento de instalar nuestro "boot loader" y paquetes finales antes de reiniciar la máquina. GRUB será nuestro boot loader.

1. Instala los siguientes paquetes. Puedes agregar paquetes que creas necesarios.

```bash
pacman -S grub efibootmgr networkmanager network-manager-applet dialog os-prober mtools dosfstools base-devel linux-headers cups reflector openssh git xdg-utils xdg-user-dirs virtualbox-guest-utils
```

Acepta todos los prompts por defecto. Y presiona ENTER para continuar con la instalación

![Instala Grub y Otros Paquetes](/images/posts/archvbox-install/paquetes_finales-min.jpg)

![Instala Grub y Otros Paquetes](/images/posts/archvbox-install/paquetes_finales2-min.jpg)

Si te aparece la siguiente pantalla, presiona ENTER para aceptar las llaves PGP.

![Instala Grub y Otros Paquetes](/images/posts/archvbox-install/paquetes_finales3-min.jpg)

### Instala GRUB

Es momento de instalar grub en el directorio /boot/efi que creamos al inicio, que es la partición de inicio

```bash
grub-install --target=x86_64-efi --efi-directory=/boot/efi --botloader-id=GRUB
```

3. Ahora necesitamos crear el archivo de configuración

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

![Instala Grub](/images/posts/archvbox-install/grub-install-min.jpg)

### Habilita los servicios

Llegó el momento de habilitar los servicios que correrán cada que reinicies la máquina.

1. Habilita el manejador de la red
2. Continua con el servicio de SSH
3. Luego el servicio para imprimir

```bash
systemctl enable NetworkManager
systemctl enable sshd
systemctl enable org.cups.cupsd
```

![Habilita Servicios](/images/posts/archvbox-install/habilita_servicios-min.jpg)

### Crea tu usuario

Todo lo hemos hecho con el usuario de root, pero siempre es conveniente tener tu propio usuario para no utilizar root para las tareas diarias. Voy a crear mi usuario que se llamará "jorge".

1. Crea el usuario y asignalo al grupo "wheel"
2. Crea un password para tu usuario

```bash
useradd -mG wheel jorge
passwd jorge
```

3. Ahora para darle privilegios de "sudo" y puedas ejecutar comandos como superusuario "root", debemos descomentar la linea que dice **"%wheel ALL=(ALL) ALL"** con el siguiente comando

Recuerda que puedes sustituir "vim" por "nano" si lo prefieres.

```bash
EDITOR=vim visudo
```

![](/images/posts/archvbox-install/visudo-min.jpg)

### Sal de la instalación y reinicia la máquina virtual

Con esto terminamos la instalación, así que solo queda:

1. Salir de la instalación
2. Desmontar las particiones
3. Reiniciar la máquina virtual

```bash
exit
umount -a
reboot
```
![](/images/posts/archvbox-install/reboot-min.jpg)

## Instalación Exitosa

Después de reiniciar la máquina virtual verás que GRUB está instalado y debe estar funcionando correctamente.

![](/images/posts/archvbox-install/grub_inicio-min.jpg)

Entra a tu nueva instalación de Arch Linux con el usuario y password que creaste para verificar que todo esté funcionando correctamente.

![](/images/posts/archvbox-install/instalacion_completa-min.jpg)

Si encontraste este tutorial útil no olvides que puedes compartirlo con tus compañeros, familiares y amigos utilizando los botones al final del artículo.

No dejes de visitarme porque estaré subiendo más tutoriales, ahora lo que sigue es instalar un escritorio en esta máquina virtual. ¿Cuál te gustaría instalar? ¿Gnome? ¿KDE? ¿O talvez un manejador de ventanas como i3 o awesome?
